+++
title = "Project Fëanor"
summary = "MSP430-based Quadcopter"
authors = []
tags = ["C", "Electronics"]
categories = []
date = "2022-04-05"

external_link = ""

url_code = "https://github.com/ADSteele916/feanor"
url_pdf = ""
url_slides = ""
url_video = ""

slides = ""

[image]
  caption = ""
  focal_point = "BottomRight"
  preview_only = false
+++

Project Fëanor was my term project for PHYS 319: Electronics Laboratory. I designed a quadcopter using mostly 3D-printed parts and used the MSP430 microcontroller as a flight controller, while working within its limited 512 bytes of RAM and 16 kilobytes of memory. To control the four motors, I wrote C code that interfaced with an accelerometer over I2C and accepted commands from a laptop over UART.
