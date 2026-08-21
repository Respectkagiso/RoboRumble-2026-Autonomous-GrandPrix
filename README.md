# Robo Rumble 2026: Autonomous Grand Prix Entry

**Student Name:** Respect Kagiso Morupane  
**Category:** Robo Grand Prix (Autonomous Racing)  
**Institution/Affiliation:** WorldSkills / Mechatronics Entry  

---

## High-Level Summary

This repository contains the complete engineering design, firmware source code, electronic schematics, and technical documentation for an autonomous scale race car built for the **Robo Rumble 2026: Autonomous Grand Prix** challenge.

Designed on a 1:10 scale RC frame featuring Ackermann front-wheel steering geometry, the vehicle operates strictly within the mandatory competition limits—measuring under the **50 cm × 50 cm** maximum footprint and weighing under the **5 kg** weight threshold.

### Autonomous Navigation & Architecture
* **Primary Processor:** ESP32 Dual-Core Microcontroller operating at 240 MHz. Task allocation isolates LiDAR point-cloud acquisition on Core 0 and PID motor/steering execution math on Core 1.
* **LiDAR Sensing:** RPLIDAR A1M8 360° Laser Range Scanner providing real-time obstacle identification and track boundary mapping at 10 Hz.
* **Inertial Tracking:** MPU6050 6-axis Gyroscope & Accelerometer for real-time heading corrections and turn angle stabilization.
* **Motor Drive:** BTS7960 43A High-Power MOSFET H-Bridge driver powering a 540 brushed DC propulsion motor with high-frequency 20 kHz PWM for instant torque.
* **Steering:** MG996R high-torque metal-gear servo delivering rapid steering response.

### Safety & Compliance
The power system is driven by a 2S 7.4V LiPo battery. Strict adherence to competition safety regulations is maintained through a dual-isolation circuit consisting of a heavy-duty **Main Power ON/OFF Toggle Switch** and a dedicated, physical **Latching Emergency Stop (E-Stop) Push-Button** wired directly into the primary positive DC line to cut power instantly in emergency scenarios. All wireless capabilities (Wi-Fi/Bluetooth) are disabled to guarantee 100% onboard autonomous operation.

---

## Directory & Sub-folder Map

Below is the complete breakdown of files and artifacts organized inside the repository:

### 📁 `Folder A: Source Code/`
* **`main_esp32_firmware.ino`** – Main production C++ code utilizing Dual-Core FreeRTOS tasks for simultaneous sensor reading and motor execution.
* **`lidar_parser.h`** – Hardware UART buffer parser for filtering RPLIDAR A1M8 distance arrays.
* **`pid_steering.cpp`** – Real-time PID control logic calculating front-wheel steering angle based on wall distance offsets.
* **`motor_control_pwm.cpp`** – High-frequency (20 kHz) PWM drive logic for the BTS7960 MOSFET H-Bridge.
* **`README.md`** – Code architecture walkthrough, compiling instructions, and execution flowchart.

### 📁 `Folder B: Designs/`
* **`Circuit_Schematics_Diagram.pdf`** – Complete wiring schematic detailing power rails, ESP32 pinouts, BTS7960 connections, and annotated E-Stop/Power switch integration.
* **`Chassis_CAD_Assembly.png`** – Mechanical layout showing sensor placement, battery location, and 50 cm × 50 cm dimensional compliance.
* **`Power_Distribution_Flow.png`** – Visual diagram showing voltage step-downs (LiPo 7.4V to LM2596 Buck Converter 5V output).
* **`README.md`** – Detailed hardware pinout table and circuit explanation.

### 📁 `Folder C: Documentation/`
* **`Pitch_Deck_RoboRumble_2026.pdf`** – Mandatory 7-slide project presentation deck.
* **`Bill_of_Materials_BOM.xlsx`** – Complete itemized component list with purchase links, quantities, and highlighted total cost.
* **`Holistic_Build_Report.pdf`** – Comprehensive engineering report covering vehicle design, test results, PID tuning, and safety features.
* **`FQA_Attendance_Log.md`** – Attendance logbook detailing project milestones, testing sessions, and screenshot proof.
* **`README.md`** – Overview of documentation artifacts and verification steps.
