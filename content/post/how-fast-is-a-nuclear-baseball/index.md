+++
title = "How Fast Is a Nuclear Baseball?"
subtitle = "A Brief Dive Into the Process of Designing and Testing a Fermi Estimation Question for UBC’s Physics Olympics"
summary = "A Brief Dive Into the Process of Designing and Testing a Fermi Estimation Question for UBC’s Physics Olympics"
authors = []
tags = ["Fermi Questions", "Fun", "Physics"]
categories = []
date = "2022-03-06"
lastmod = "2022-03-06"
featured = false
draft = false

[image]
caption = ""
focal_point = ""
preview_only = false

projects = []
+++

UBC's annual Physics Olympics just happened yesterday. Every year, high schools from across British Columbia send teams of students to compete in a series of physics-themed events. There are a couple of pre-built events, where students design some contraption; a lab event, where they need to apply what they learned to measure some quantity experimentally; Quizzics, where students answer rapid-fire physics and astronomy-related trivia; and Fermi questions, where they apply their knowledge of physics to give order-of-magnitude estimates for various fun problems.

Fermi questions are named after the Italian-American physicist Enrico Fermi, who was famously able to provide order-of-magnitude estimates for seemingly impossible-to-compute figures, like the number of piano tuners in New York City, the blast yield of the Trinity test, or the number of intelligent civilizations in the galaxy with which we could potentially communicate. The objective is to come up with a series of trivial estimates that can be multiplied together to obtain a decent estimate of a highly non-trivial problem.

The Fermi questions were always my favourites back when I competed in high school. For the past three years, I've helped design and test them. This usually involves me and [Rio Weil](https://rioweil.github.io/) coming up with a bunch of ideas for different quantities to estimate, then trying to determine different ways to arrive at them. Our criteria for good questions is that they must be non-elementary, robust, and doable on a calculator. By non-elementary, we just mean that it's not a question one would see in a high school physics textbook where a student could just use a known formula and constants, together with given quantities, to obtain an answer. The solver needs to make a non-obvious estimate, or apply some sort of university-level knowledge. Robustness is determined by whether or not different approaches converge to the same answer. This usually isn't a problem when questions are specifically-worded enough, but it can still be problematic if the solution is highly sensitive to how one chooses to model the problem. Finally, since students aren't allowed calculators, we try to ensure that the questions don't rely on any transcendental functions (save for the trigonometric functions that everyone has memorized) or complicated square roots.

We usually come up with a set of problems that trend upward in difficulty. The first question is usually a matter of making one or two (possibly slightly unintuitive) estimates and combining them using simple formulas from high school physics. The last question, however, will typically require students to come up with a more complex model or have knowledge of advanced physics (that a particularly keen AP or IB student might be expected to have).

I thought it would be interesting to walk through the process of designing a problem. Since all of the problems from yesterday's event were solved live by Jeorg Rottler at the end of the day, we'll look at a question that Rio and I tried to make work for a while, but ended up putting aside.

The inspiration for this question came from the ["Relativistic Baseball"](https://what-if.xkcd.com/1/) article on xkcd's *What If?* blog. I was curious about how quickly a baseball would need to be thrown to replicate the effects described in the article (namely, a small nuclear blast). To be more concrete, I phrased the question as follows:

> How fast would one need to throw a baseball in order for its impact to release as much energy as the atomic bomb dropped on Hiroshima? Assume no air resistance.

The question, in its current state, requires the estimation of two quantities: the mass of a [baseball](https://en.wikipedia.org/wiki/Baseball_(ball)) and the yield of [Little Boy](https://en.wikipedia.org/wiki/Little_Boy). This alone does not make for an interesting problem. However, I was hoping that, like in xkcd, special relativity would come into play.

In classical mechanics, the kinetic energy of an object is given by the expression $\frac{1}{2} m v^2$, where $m$ is the mass of the object and $v$ is its velocity. Therefore, solving this problem classically is simply a matter of substituting one's estimates into $v = \sqrt{\frac{2E_{\text{blast}}}{m}}$, where $E_{\text{blast}}$ is the yield of the Hiroshima bomb. However, when $v$ is close to the speed of light, this relation breaks down and we must, instead, turn to special relativity.

The total relativistic energy of an object is given by $\gamma m c^2$, where $\gamma = \frac{1}{\sqrt{1 - \frac{v^2}{c^2}}}$ is the so-called Lorentz factor, which is equal to $1$ for a stationary object and diverges to infinity as $v$ approaches $c$, the speed of light. To obtain the object's relativistic kinetic energy, we simply subtract off its rest mass energy $E_0 = mc^2$ (this is obtained from the total relativistic energy formula by setting $v = 0$) and obtain $T = E - E_0 = (\gamma - 1) m c^2$ (as a fun exercise, one can derive the classical $T = \frac{1}{2} m v^2$ by taking a first-order Taylor approximation of this quantity). Note that we are using relativistic kinetic energy, not total energy. The rest mass energy of a baseball is enough for several hundred Hiroshima bombs, and the question never states that the baseball will be disintegrated down to its last atom. Rearranging to solve for $v$, we find:

$$\begin{align}
T = E_{\text{blast}} &= (\gamma - 1) m c^2 \newline
\frac{E_{\text{blast}}}{m c^2} + 1 &= \gamma \newline
\frac{E_{\text{blast}}}{m c^2} + 1 &= \frac{1}{\sqrt{1 - \frac{v^2}{c^2}}} \newline
\frac{1}{\frac{E_{\text{blast}}}{m c^2} + 1} &= \sqrt{1 - \frac{v^2}{c^2}} \newline
\frac{1}{\left(\frac{E_{\text{blast}}}{m c^2} + 1\right)^2} &= 1 - \frac{v^2}{c^2} \newline
1 - \frac{1}{\left(\frac{E_{\text{blast}}}{m c^2} + 1\right)^2} &= \frac{v^2}{c^2} \newline
\frac{\left(\frac{E_{\text{blast}}}{m c^2} + 1\right)^2 - 1}{\left(\frac{E_{\text{blast}}}{m c^2} + 1\right)^2} &= \frac{v^2}{c^2} \newline
c \cdot \frac{\sqrt{\left(\frac{E_{\text{blast}}}{m c^2} + 1\right)^2 - 1}}{\frac{E_{\text{blast}}}{m c^2} + 1} &= v
\end{align}$$

Needless to say, the use of special relativity propels this problem from being too easy to be the first question to being a potential final problem, just by virtue of it requiring knowledge of relativistic energy.

However, we still have not determined whether the desired appreciable distinction between the two approaches actually exists. Solving classically, we find a velocity of $2.8927 \times 10^{7} \text{ m/s}$. Using special relativity, we instead obtain $2.8826 \times 10^{7} \text{ m/s}$. While one could fairly easily use this question in a setting where calculators are allowed, three digits of precision is too much to ask for on a Fermi question. The differences between speeds calculated using slightly different bomb yields would be far greater than the difference between the classical and relativistic approaches.

I resolved this problem by changing the energy we were comparing against. Instead of Little Boy, I substituted the energies released by the [Tsar Bomba](https://en.wikipedia.org/wiki/Tsar_Bomba) (50 megatonnes), the [Krakatoa eruption](https://en.wikipedia.org/wiki/1883_eruption_of_Krakatoa) (200 megatonnes), and the [asteroid impact](https://en.wikipedia.org/wiki/Chicxulub_crater) that is hypothesized to have caused the dinosaurs' extinction (100 teratonnes). All of these energy releases result in massive differences between the classical and relativistic results. The classical results are impossibly large (i.e. faster than the speed of light), while the relativistic results converge to values slightly lower than the speed of light.

However, this brings us to another issue of precision, as the correct values would be virtually indistinguishable from the speed of light with only one or two digits of precision. A student could just calculate a speed of $1.6701 \times 10^{9} \text{ m/s}$ (for equivalence to the Tsar Bomba) classically, note that his or her result is much greater than the speed of light, and then simply guess $2.998 \times 10^{8} \text{ m/s}$ without doing any of the relativistic calculations that we just covered.

For this question to work, two criteria had to be met. First, there must be a classical error significant enough to notice at one digit of precision, such that taking the relativistic approach is rewarded. Second, the relativistic speed must be distinguishable from the speed of light. Also, it would be nice if the classical speed was less than the speed of light, to make the need for using relativity non-obvious. After [some messing around on Desmos](https://www.desmos.com/calculator/d1b5wlk9uj), I determined that 5.581 megatonnes was about ideal, with a factor of two separating the relativistic and classical results. This still gave us a relativistic result of $0.931c$, with a classical result well over the speed of light, however.

In theory, this question could still have been a candidate. It would have a pretty glaring weakness in that simply guessing the speed of light would likely give at least part marks (for first digit accuracy), but that isn't highly likely to occur to a student with a time limit of five minutes. However, the last nail in its coffin was that there just wasn't anything all that interesting that released 5.581 megatonnes of energy. Anything we could come up with that wasn't incredibly obscure required several more layers of estimations that the students probably wouldn't have time to make. For this reason, we ended up scrapping the nuclear baseball question in favour of others.

That was a brief overview of the design and testing process that goes into writing a Fermi question, along with some hopefully interesting physics. If you happen to be a UBC student in the physics department (or even one outside it with an interest in physics) and you found this compelling, consider volunteering to help organize the Physics Olympics next year. More ideas (and hands) are always welcome.
