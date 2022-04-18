+++
title = "Project Lance"
summary = "Desktop Application and NEAT Model for Generation I AI Pokemon battling."
authors = []
tags = ["Machine Learning", "Python"]
categories = []
date = "2021-07-17"

external_link = ""

url_code = "https://github.com/ADSteele916/project-lance"
url_pdf = ""
url_slides = ""
url_video = ""

slides = ""

[image]
  caption = "First rival battle in Pokemon Yellow."
  focal_point = "Smart"
  preview_only = false
+++

In order to give myself some more tactile experience working with machine learning, I challenged myself to create a bot capable of competing in Generation I Pokemon battles. However, training the bot would still take forever with the only existing option - running a local Pokemon Showdown server and interfacing with it. To solve this problem, I devised Project Lance: a locally-run Python implementation of Generation I Pokemon battles. With it, I was able to run about 2000 full battles per second on my CPU, as opposed to the couple dozen I could run by interfacing with Showdown in that same time. To further improve training speed, I implemented a multicore evaluator for self-play. This allowed me to train a [NEAT](https://en.wikipedia.org/wiki/Neuroevolution_of_augmenting_topologies) model to play optimally on a simplified version of the full battle engine.
