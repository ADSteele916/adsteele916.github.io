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
company = "Kepler Communications"
company_url = "https://kepler.space/"
company_logo = "Kepler-Icon-Logo-Transparent-Black"
location = "Toronto, Ontario"
date_start = "2021-01-04"
date_end = "2021-08-27"
description = """
  * Created drivers for the LCD and keypad on Kepler's next-generation modems using **Python**.
  * Architected and developed a modular user configuration menu for the modems using my driver.
  * Designed, implemented, and tested multithreaded systems connecting the next-generation modems' configuration interfaces to Kepler's Global Data Service in **Python** and **Bash**.
  * Wrote suites of unit and integration tests using **PyTest** with full coverage for all new features.
  * Refactored and expanded Kepler's **SQL**-based remote deployment system by generalizing software image database to support multiple satellite models with different compatibilities."""

[[experience]]
title = "Undergraduate Teaching Assistant"
company = "University of British Columbia"
company_url = "https://www.cs.ubc.ca/"
location = "Vancouver, British Columbia"
date_start = "2020-01-09"
date_end = ""
description = """
  * TA for CPSC 110: Computation, Programs, and Programming, UBC's major-stream introductory computer science course, for seven academic terms.
  * Lead TA for internal systems, including the autograder and handin server, since September 2020.
  * Summer Course Development Assistant in 2020 and 2021.
  * Coordinates with the professors, course coordinators, and other TAs to ensure that students comprehend course concepts.
  * Supervises multiple weekly laboratory sections of about 30 students each, in which hands-on learning of the course content is facilitated.
  * Develops graders, server scripts, and other tools in **Racket**, **Python**, and **Bash** to streamline instructors' workflows and improve students' engagement with the course.
  * Aided in planning how the course would be taught online during the COVID-19 pandemic."""

[design]
columns = "2"
+++
