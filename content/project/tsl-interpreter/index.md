+++
title = "TSL Interpreter"
summary = "Interpreter for a toy sublanguage of Racket."
authors = []
tags = ["Programming Languages", "Python"]
categories = []
date = "2020-08-04"

external_link = ""

url_code = "https://github.com/ADSteele916/tsl-interpreter"
url_pdf = ""
url_slides = ""
url_video = ""

slides = ""

[image]
  caption = "Demonstration of higher-order function evaluation."
  focal_point = "Smart"
  preview_only = false
+++

This is just a fun little project I made over the August long weekend. It's a parser and interpreter for the "Tiny Student Language", a sublanguage of [Racket](https://racket-lang.org/). I made it as an accessible, high-level demonstration of how programming languages work. As such, it has very few built-in types and functions. However, it is still capable of correctly evaluating recursive and higher-order functions, and, theoretically, can traverse trees and graphs using mutually-recursive functions (this would be rather ugly and inefficient due to the absence of define-struct and local though). I don't plan on adding new features to TSL in the future, so as to keep it simple, but I would like to expand its error handling, so imperfect code doesn't cause an instant crash.
