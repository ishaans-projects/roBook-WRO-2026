# roBook - Autonomous Manuscript Sorting System


An innovative, automated robotic solution designed to safely sort and preserve ancient manuscripts by date and geographical origin, preventing damage and human error in museums. Created for the **World Robot Olympiad (WRO) 2026 - Future Innovators (Elementary)**, where it won **3rd Place in Canada**.

## 🚀 Project Overview
* **The Problem:** Up to 90% of ancient manuscripts have been lost or damaged due to improper organization and handling.
* **Our Solution:** A custom-built robot utilizing Computer Vision and AI to scan manuscript labels, determine their age/origin, and automatically place them onto designated shelves.
* **Target Environment:** Designed using affordable, accessible components (\$1500 CAD total build budget) for museums of all sizes.

---

## 🛠️ System Architecture


```text
[ Manuscript Input ] 
       │
       ▼
 ┌──────────┐      Frame Stream      ┌──────────────────────────┐
 │  Camera  │───────────────────────>│   OpenCV Pre-Processing  │
 └──────────┘                        └──────────────────────────┘
                                                   │
                                                   ▼
 ┌──────────┐     Telemetry Stream   ┌──────────────────────────┐
 │  BME280  │───────────────────────>│  Tesseract OCR (Worker)  │
 └──────────┘                        └──────────────────────────┘
                                                   │
                                                   ▼
 ┌──────────┐       GPIO Control     ┌──────────────────────────┐
 │  L298N   │<───────────────────────│  Python Logic / Regex    │
 └──────────┘                        └──────────────────────────┘
       │                                           ▲
       ▼                                           │ Live Updates
 ┌──────────┐                                ┌─────────────┐
 │  Motors  │                                │ Pygame GUI  │
 └──────────┘                                └─────────────┘
```




### 1. Mechanical Subsystems
* **Drivetrain:** A heavy-duty tank drive built using aluminum C-channels and powered by four VEX EDR 2-wire motors with Omni wheels for zero-radius turning.
* **Book Dispensing Lifting Rack:** Perpendicular double rack-and-pinion system driven by dual VEX motors to raise the platform to multiple bookshelf heights.
* **Horizontal Pusher:** Dual 3D-printed rack-and-pinion rails powered by two DC motors to smoothly push manuscripts out of the robot onto shelves.

### 2. Electrical Core
* **Main Microcontroller:** Raspberry Pi 5 running Raspberry Pi OS with a 128GB high-speed SD card.
* **Motor Control:** Dual L298N H-Bridge motor drivers wired with inverted polarity pairs to prevent physical jams.
* **Power Delivery:** Swapped a lightweight 6.5V AA battery housing for a robust 12V/9V/5V discrete multi-output battery pack to stop core brownouts and Raspberry Pi crashes under motor loads.

---

## Python Libaries
**Pygame**>>Gui(Graphical User Interface) with buttons and screen
**OpenCV**>>Camera and talking to pytesseract
**AdafruitBME280**>>Sensor libary for finding humidity and temp
**Pytesseract**>>ocr based text detecting libary

---

## 💻 Software & Pipeline

The system is programmed entirely in **Python** and optimized using background multiprocessing to keep the Pygame graphical interface running smoothly at 30 FPS.

### Core Software Tasks
1. **Graphical User Interface:** Built with Pygame to showcase live video feeds, real-time climate telemetry, and instant emergency kill switches.
2. **Computer Vision & OCR Pipe:** Captures video frames using OpenCV, transforms them via adaptive thresholding, and processes them through `pytesseract`.
3. **Data Classification (Regex):** Parses extracted strings using a targeted regular expression pattern to isolate the year and geographic continent code digit:
   ```python
   matches = re.findall(r"(17[0-9]{2}|18[0-9]{2}|19[0-9]{2}|20[0-9]{2})([1-7])", text)
   ```
4. **Environmental Monitoring:** Continuously samples live museum data using a DHT11 temperature and humidity sensor to guarantee safe document storage environments.

---

## 🔧 Engineering Challenges & Solutions

* **The Left-Side Lift Sag:** Our early single-motor rack lift caused the left frame rail to bind and jam under load. We re-engineered the lift to use balanced, dual-sided synchronized VEX motors.
* **The Montreal Hardware Breakdown:** When 3 Raspberry Pis fried the night before the national finals, we successfully rebuilt the operating system, compiled libraries, and deployed our backup scripts on a replacement board overnight to keep our team in the competition.

---
*Created by Team Future Robo Labs (Mohit, Ishaan, Teddy) @ Zebra Robotics*.
