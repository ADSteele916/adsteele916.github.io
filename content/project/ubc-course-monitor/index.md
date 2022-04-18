+++
title = "UBC Course Monitor"
summary = "Web application that monitors courses at UBC for seat openings."
authors = []
tags = ["Django", "Python", "Web Development"]
categories = []
date = "2020-07-16"

external_link = ""

url_code = "https://github.com/ADSteele916/ubccoursemonitor"
url_pdf = ""
url_slides = ""
url_video = ""

slides = ""

[image]
  caption = "Webpage for adding a course section to one's monitor list."
  focal_point = "Smart"
  preview_only = false
+++

UBC Course Monitor is a web application that I built to help fellow UBC students find open seats in courses they want to take. Users can sign up to monitor any course section publically listed on the University of British Columbia's Student Services Centre. Once they add the course to their list, they will receive an email notification when a seat opens up. The backend of the site was built using the Django framework and uses a Celery queue to do background monitoring. The application is deployed on Heroku and uses a PostgreSQL database to manage users and courses.
