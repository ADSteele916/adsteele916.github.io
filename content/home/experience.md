+++
# An instance of the Experience widget.
# Documentation: https://wowchemy.com/docs/page-builder/
widget = "experience"

# This file represents a page section.
headless = true

# Order that this section appears on the page.
weight = 20

title = "Experience"
subtitle = ""

# Date format for experience
#   Refer to https://wowchemy.com/docs/customization/#date-format
date_format = "Jan 2006"

# Experiences.
#   Add/remove as many `experience` items below as you like.
#   Required fields are `title`, `company`, and `date_start`.
#   Leave `date_end` empty if it's your current employer.
[[experience]]
title = "Software Engineer Intern"
company = "Tesla"
company_url = "https://www.tesla.com/"
company_logo = "Tesla_Motors"
location = "Palo Alto, California"
date_start = "2023-05-15"
date_end = ""
description = """
  * Developed **C** firmware for communication over the vehicle's controller area network, enabling full archival of crash data.
  * Independently discovered and patched allocation inefficiency in **Rust** simulation codebase, improving runtimes by 30%.
  * Optimized **Rust** data intake pipeline by hand-writing a parser, resulting in an 80% performance improvement.
  * Wrote **Rust** software-in-the-loop (**SIL**) models and integration tests for inter-chip SPI communication and snooping.
  * Ported internal hardware-in-the-loop (**HIL**) testing library to **Linux** and enabled firmware engineers to remotely execute tests using **Python**, **C**, **Docker**, and **Jenkins**."""
[[experience]]
title = "Software Engineer Intern"
company = "Tesla"
company_url = "https://www.tesla.com/"
company_logo = "Tesla_Motors"
location = "Palo Alto, California"
date_start = "2022-09-06"
date_end = "2022-12-16"
description = """
  * Designed and wrote SPI drivers to control the restraint control module's (RCM) inertial measurement units (IMUs) in **C**.
  * Implemented numerical integral approximations for the RCM crash algorithm's near-deploy calculations in **C**.
  * Created chip-level software-in-the-loop (**SIL**) models for the RCM's onboard IMUs with extensive fault-injection capabilities in **Rust** and **PyO3** and wrote SIL tests for drivers and crash algorithm using **PyTest**.
  * Reduced hardware-in-the-loop (**HIL**) test execution time from 5.5 hours to 2 minutes and enabled the addition of the HIL test suite to continuous integration (CI) by automating test running using **Python** and **C**."""
[[experience]]
title = "Numerical Methods Research Assistant"
company = "UBC Department of Computer Science"
company_url = "https://www.cs.ubc.ca/"
company_logo = "ubc-logo-2018-crest-blue282-pms"
location = "Vancouver, British Columbia"
date_start = "2022-05-09"
date_end = "2022-08-26"
description = """
  * Created novel discretization technique for solving ill-conditioned instances of the Helmholtz equation in **MATLAB**.
  * Developed high-performance finite-element magnetohydrodynamic simulation software using **C++** and **Eigen**.
  * Optimized the performance of simulations with millions of degrees of freedom using knowledge of vector calculus."""
[[experience]]
title = "Software Engineer Intern"
company = "Kepler Communications"
company_url = "https://kepler.space/"
company_logo = "Kepler-Icon-Logo-Transparent-Black"
location = "Toronto, Ontario"
date_start = "2021-01-04"
date_end = "2021-08-27"
description = """
  * Architected and created drivers and a multithreaded **Python** application for the display and keypad on Kepler's next-generation modems.
  * Singlehandedly developed new remote software image deployment system capable of supporting the growing number of models in Kepler's constellation of 19 satellites using **Python** and **SQL**."""

[[experience]]
title = "Lead Undergraduate Teaching Assistant"
company = "University of British Columbia"
company_url = "https://www.cs.ubc.ca/"
company_logo = "ubc-logo-2018-crest-blue282-pms"
location = "Vancouver, British Columbia"
date_start = "2020-01-09"
date_end = ""
description = """
  * Maintains **Racket** autograder server used by over 800 students to submit and receive feedback on over 1500 files daily.
  * Improves students' engagement by providing personalized feedback using applications developed in **Python** and **Bash**.
  * Detected over 200 cases of academic misconduct by designing, implementing, and deploying novel code-similarity algorithm using **Rust**, **Python**, and **TensorFlow**.
  * Supervises three other teaching assistants who contribute to the course infrastructure and teaching materials."""

[design]
columns = "2"
+++
