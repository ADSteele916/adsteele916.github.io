 +++
title = "Category Theory for Fun and Profit"
subtitle = "or, The Design of `rand-functors`"
summary = "A dive into the design of my `rand-functors` and discussion of the value of the more \"theoretical\" areas of CS."
authors = []
tags = ["Academics", "Software Engineering"]
categories = []
date = "2024-04-07"
lastmod = "2024-04-07"
featured = false
draft = false

[image]
caption = ""
focal_point = ""
preview_only = false

projects = []
+++

Algorithms for zero-sum imperfect-information games are a longstanding interest of mine. I've got a couple of hobby projects in that area which I work on whenever I have free time. A class of strategies I often use as a starting point for such projects is model-based search. This typically requires a good simulator of the game, which I often end up needing to write myself.

I'm currently writing a simulator library for a game in Rust, with plans to expose a Python API through [PyO3](https://pyo3.rs/) for rapid experimentation (PyO3 has rapidly become one of my favourite libraries---my days of fighting pybind11 might just be over). I want to make my library as performant as possible. I don't know what algorithms will work best, but I want online search to be feasible at a reasonable depth.

In these sorts of projects, I often find myself avoiding programming in a purely object-oriented or purely functional paradigm. Each has its merits but they share a drawback: indirection. A virtual method call requires the use of an indirect call Assembly instruction since the address of the desired function is not known at compile-time. It also prevents any sort of inlining. The same applies to lambda functions in functional paradigms. The latter group also tends to rely on many heap allocations which are later garbage-collected. None of these properties are acceptable in terms of performance. Instead, I try to adopt whatever architecture will work best in a given scenario, whether it fits into a particular paradigm or not.

With these constraints in mind, I was faced with something of a dilemma when figuring out how to generate the next nodes in the game tree given a current node. You see, I wanted my simulator to be useful for both evaluation and exploration. In the former use case, I'd want it to quickly sample one possible outcome of a random process. In the latter, I'd want all possible outcomes, so I could construct the game tree properly. These goals resulted in two functions that (drastically simplified) looked something like these:
```rust
use rand::prelude::*;

fn next_state(mut state: u8) -> u8 {
    state = state.wrapping_add(random());
    if random() {
        state %= 3;
    }
    state
}

fn next_states(state: u8) -> Vec<u8> {
    let mut out: Vec<_> = (0..=255).map(|r| state.wrapping_add(r)).collect();
    out.append(&mut out.iter().copied().map(|i| i % 3).collect());
    out
}
```
These two functions may seem different enough, but upon closer inspection, one can find common behaviour. Note how both sum their input with some `u8` with a `wrapping_add` and then reduce modulo 3 if some `bool` is true. The only differences relate to where those `u8` and `bool` values come from. In `next_state`, they are random, but in `next_states`, all possible values are looped over. The former is used for quick evaluation; the latter is used for complete exploration. However, the process remains the same, and that worries me.

Since the same process is described in code twice, the above listing flies in the face of the principle of DRY: "Don't repeat yourself." This may seem like unproductive fretting over writing the cleanest possible code and, in this example, it probably is. But in my actual codebase, sharing this process description meant paired chains of helper functions operating on parameters of types `State` and `Vec<State>` respectively. These chains of functions were tightly coupled to each other---a change in the `State` version of a function almost always induced a change in the `Vec<State>` version. It also meant that, if I wanted to add the option to produce a `HashMap<State, usize>` or employ some clever strategy for collecting a sample of some fixed size, I would need to create a third set of duplicate functions.

I wanted some way to abstract over both sampling from a distribution and collecting its possible outcomes in a `Vec` or a `HashMap`. I spent a good couple of hours thinking about the problem during my long run that afternoon. Eventually, I made a connection that I wouldn't have expected.

Last fall, I took a course on Programming Languages Theory (CPSC 311) taught by Professor [Ron Garcia](https://www.cs.ubc.ca/~rxg/). In the course, we made extensive use of monads to abstract out effectful parts of our languages and keep our interpreters simple (Ron insistently called monads "effect abstractions" for this reason). What I thought I wanted was some sort of monad that could abstract over these different behaviours and return types, which I could implement using Rust's trait system and monomorphize away at compile time.

After a good amount of category theory review and some valuable input from Ron and from my old friend [Sean Bocirnea](https://passingti.me/) (whom I'm sure could have done this himself in fewer attempts), I was able to put together a first-pass implementation.

It turned out what I actually needed was something between a [functor](https://en.wikipedia.org/wiki/Functor_(functional_programming)) and an [applicative functor](https://en.wikipedia.org/wiki/Applicative_functor) (monads are a special case of applicatives). In short, I needed a collection of some arbitrary inner type which I could both create from a single instance of the inner type (`Functor::pure`) and map over (`Functor::fmap`). The names "functor", `pure`, and `fmap` are needlessly opaque for our purposes here. Here is a concrete example of implementations of those methods for lists in Python:
```python
def list_fmap(f, lox):
    return [f(x) for x in lox]

def list_pure(x):
    return [x]
```
These functors could be used to contain the state for a random process. A version of the [Strategy pattern](https://refactoring.guru/design-patterns/strategy) would determine which functor should be created and operated on (`RandomStrategy::Functor`), as well as how random events should be handled (`RandomStrategy::fmap_rand`). Decoupling `fmap_rand` from individual `Functor` implementations was a major breakthrough for me in the design. This allowed the use of the same containers (like `Vec`) for multiple strategies, such as listing all possible outcomes or a fixed sample size of possible outcomes.

In the end, this is the interface I ended up with:
```rust
pub trait RandomStrategy {
    type Functor<I: Inner>: Functor<I>;

    // Required methods
    fn fmap<A: Inner, B: Inner, F: Fn(A) -> B>(
        f: Self::Functor<A>,
        func: F
    ) -> Self::Functor<B>;
    fn fmap_rand<A: Inner, B: Inner, R: RandomVariable, F: Fn(A, R) -> B>(
        f: Self::Functor<A>,
        rng: &mut impl Rng,
        func: F
    ) -> Self::Functor<B>
       where Standard: Distribution<R>;
}

pub trait Functor<I: Inner> {
    // Required method
    fn pure(i: I) -> Self;
}

pub trait Inner: Clone + Eq + Hash + PartialEq { }

pub trait RandomVariable: Sized
where
    Standard: Distribution<Self>,
{
    // Required method
    fn sample_space() -> impl Iterator<Item = Self>;
}
```
This looks like a lot, but it's fairly easy to break down. A `RandomStrategy` abstracts over different approaches to handling a point in a process where randomness plays a role. Associated with a `RandomStrategy` is a `Functor`, which acts as a container for an `Inner`, which is just the type that the original random process was operating on. `Functors` have the `pure`  method we alluded to earlier to begin computations. Also associated with a `RandomStrategy` are `fmap` and `fmap_rand` functions, which take a `Functor` and apply a function operating on the inner of a `Functor` and (in the case of `fmap_rand`) an arbitrary `RandomVariable`. A `RandomVariable` is just a type that supports sampling from the `Standard` distribution and enumerating all possible outcomes using its `sample_space` associated function.

The definition of `fmap` as an associated function of a `RandomStrategy`, rather than a method of a `Functor`, is due to a limitation of Rust's type system. Ideally, we'd want something like this:
```rust
pub trait Functor<I: Inner> {
    // Required methods
    fn pure(i: I) -> Self;

    fn fmap<A: Inner, B: Inner, F: Fn(A) -> B>(
        f: Self::Functor<A>,
        func: F
    ) -> Self<B>;
}
```
Unfortunately, `Self` is equivalent to `Functor<I>`, not just `Functor`. This makes defining `fmap` as a method on a `Functor` impossible at the moment. This was another issue I struggled with before arriving at the above design. I only got here in [v0.3.0](https://github.com/ADSteele916/rand-functors/releases/tag/v0.3.0); prior versions used a different hack, borrowed from the [`higher`](https://docs.rs/higher/latest/higher/) crate, which proved to be unsound.

Here are the `Functor` implementations for both "raw" `Inner` implementors and vectors of some `Inner` implementor:
```rust
impl<I: Inner> Functor<I> for I {
    #[inline]
    fn pure(i: I) -> I {
        i
    }
}

impl<I: Inner> Functor<I> for Vec<I> {
    #[inline]
    fn pure(i: I) -> Self {
        vec![i]
    }
}
```
And here are `RandomStrategy` implementations that use those `Functor` implementors:
```rust
pub struct Sampler;

impl RandomStrategy for Sampler {
    type Functor<I: Inner> = I;

    #[inline]
    fn fmap<A: Inner, B: Inner, F: Fn(A) -> B>(f: Self::Functor<A>, func: F) -> Self::Functor<B> {
        func(f)
    }

    #[inline]
    fn fmap_rand<A: Inner, B: Inner, R: RandomVariable, F: FnOnce(A, R) -> B>(
        f: Self::Functor<A>,
        rng: &mut impl Rng,
        func: F,
    ) -> Self::Functor<B>
    where
        Standard: Distribution<R>,
    {
        func(f, rng.gen())
    }
}

pub struct Enumerator;

impl RandomStrategy for Enumerator {
    type Functor<I: Inner> = Vec<I>;

    #[inline]
    fn fmap<A: Inner, B: Inner, F: Fn(A) -> B>(f: Self::Functor<A>, func: F) -> Self::Functor<B> {
        f.into_iter().map(func).collect()
    }

    #[inline]
    fn fmap_rand<A: Inner, B: Inner, R: RandomVariable, F: Fn(A, R) -> B>(
        f: Self::Functor<A>,
        _: &mut impl Rng,
        func: F,
    ) -> Self::Functor<B>
    where
        Standard: Distribution<R>,
    {
        f.into_iter()
            .flat_map(|a| R::sample_space().map(move |r| (a.clone(), r)))
            .map(|(a, r)| func(a, r))
            .collect()
    }
}
```
The most complicated parts of these implementations are the signatures. Generic programming's power is only matched by its verbosity. However, if one looks at the actual code that's getting run, there's nothing all that surprising. If one looks at the implementations of `Functor<I> for I` or `Sampler`, the behaviour is exactly what would be expected---they just apply the function, passing a randomly-generated `RandomVariable` when needed. `Functor<I> for Vec<I>` and `Enumerator` are a bit more complicated, but can be parsed with minimal trouble by anyone with experience using Rust's iterators. They just map over the Cartesian product of the existing `Inner` elements and, in the case of `fmap_rand`, all possible values of the `RandomVariable` implementor being operated on by `func`.

With this abstraction, our old `next_state` and `next_states` functions can just become:
```rust
use rand::prelude::*;
use rand_functors::{Functor, RandomStrategy};

fn next_state<S: RandomStrategy>(state: u8) -> S::Functor<u8> {
    let mut out = Functor::pure(state);
    out = S::fmap_rand(out, &mut thread_rng(), |s, r| {
        s.wrapping_add(r)
    });
    out = S::fmap_rand(out, &mut thread_rng(), |s, r| if r { s % 3 } else { s });
    out
}
```
This new `next_state` function is able to encode the same process as the two old functions, with some added syntactic overhead. Its true strength lies in its callers' ability to switch between the behaviour of either of the old functions using Rust's ["Turbofish"](https://blog.rust-lang.org/2021/09/09/Rust-1.55.0.html#dedication) syntax. The old `next_state(s)` is equivalent to `next_state::<Sampler>(s)` and the old `next_states(s)` is equivalent to `next_state::<Enumerator>(s)`. The crucial detail which makes all this worthwhile is that Rust's generics are monomorphized at compile time. All instances of `S` will become `Sampler` and calls to its associated functions can be [aggressively inlined](https://twitter.com/ManishEarth/status/936084757212946432). In the end, an optimizing compiler can often output the exact same machine code as it did for the original functions, making `RandomStrategy` a zero-cost abstraction.

One could easily argue that this is just adding an unnecessary and somewhat leaky abstraction. When using `rand-functors`, developers need to wrap most computations in calls to `fmap` and `fmap_rand`. They will also likely need to modify many of their method signatures to consume and produce functors instead of inners. I agree that in our trivial example of `next_state` and `next_states`, the use of `rand_functors` is probably unnecessary. But in larger, more complex random processes, the abstraction demonstrates its usefulness.

This crate's true value, in my experience using it, is its ability to localize changes. I originally planned on using `Sampler` and `Enumerator` for my model-based search implementation. However, I realized that my random processes typically resulted in duplicate states, causing the vectors produced by the `Enumerator` strategies to grow unnecessarily long. This led me to believe a `HashMap` might be better instead. By designing a new `Counter` strategy, with a `HashMap<I, usize>` as its functor, I was able to test this theory extremely quickly. I swapped `Enumerator` for `Counter` and ran my benchmarks, confident that each strategy was being run as efficiently as possible. That's the true power of a zero-cost abstraction.

This entire process reinforced my belief in the importance of maintaining knowledge in a broad range of CS fields. Far too often, I see peers of mine in the CS department thumbing their noses at certain courses. I know someone who has taken Artificial Intelligence, Machine Learning and Data Mining, Intelligent Systems, Computer Vision, Natural Language Processing, and Advanced Machine Learning (CPSC 322, 340, 422, 425, 436N, and 440, for any UBC students). However, he categorically ruled out ever taking Programming Languages Theory because he "didn't see \[himself\] ever using it." To each his own, I suppose, but he and I work on quite similar problems and something in that course was rather useful to me.
