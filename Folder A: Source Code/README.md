# Folder A: Embedded Firmware & System Architecture Documentation

This repository folder contains the primary embedded control firmware, system logic flowcharts, Input-Process-Output (IPO) charts, and architectural documentation for the **Robo Grand Prix / RoboRumble 2026** autonomous race car.

---

## 🚗 System Architecture Overview

The autonomous race car operates on an **ESP32-WROOM-32U** microcontroller executing a high-speed, deterministic **"See-Think-Act"** control loop in native C++ using the Arduino framework.

### Primary Hardware Pin Mapping

| Component | Hardware Specification | ESP32 GPIO | Interface / Protocol | Function |
| :--- | :--- | :--- | :--- | :--- |
| **Microcontroller** | ESP32-WROOM-32U | — | Dual-Core 240MHz | Main Vehicle CPU (250 Hz Loop) |
| **Steering Servo** | DS3218MG (20kg Metal Gear) | `GPIO 18` | PWM (50 Hz) | Front Steering Angle (55° to 125°) |
| **Motor ESC** | 60A Brushless ESC | `GPIO 19` | PWM (50 Hz) | Throttle Control (1500µs to 1580µs) |
| **Safety E-Stop** | Physical Latching Relay/Switch | `GPIO 23` | Digital Input (`INPUT_PULLUP`) | Hardware Safety Disarm Interrupt |
| **Left Wall Sensor** | Sharp GP2Y0A21YK0F Analog IR | `GPIO 34` | Analog (12-bit ADC) | Left Wall Distance Measurement |
| **Right Wall Sensor** | Sharp GP2Y0A21YK0F Analog IR | `GPIO 35` | Analog (12-bit ADC) | Right Wall Distance Measurement |
| **Front LiDAR** | TF-Luna Micro LiDAR | `GPIO 16` (RX2)<br>`GPIO 17` (TX2) | Serial UART (115,200 baud) | High-Speed Forward Obstacle Scan |

---

## 🧠 Software Pipeline & Execution Logic

The firmware runs in a non-blocking loop strictly rate-limited to **250 Hz (4 ms per cycle)** to ensure deterministic response times at racing speeds.

1. **Safety Interrupt (Top Priority):** Reads `GPIO 23` at the beginning of every single cycle. If `HIGH`, motor output is cut instantly (1500µs Neutral) and steering centers (90°).
2. **See (Perception):** Reads analog voltage levels from left/right IR sensors (GPIO 34/35) and parses UART distance frames from the front TF-Luna LiDAR.
3. **Think (Control & Braking):** 
   * Checks for immediate front collision hazards (< 30 cm).
   * Calculates lateral deviation error ($e = \text{Left Distance} - \text{Right Distance}$).
   * Passes error through closed-loop PID math ($K_p=1.8, K_i=0.05, K_d=0.4$) with anti-windup clamping to compute steering adjustments.
4. **Act (Actuation):** Sends clamped PWM signals to the DS3218MG servo (55° to 125°) and forward pulse commands to the 60A ESC (1580µs).

---

## 🔄 Logic Flowchart

```text
+------------------------------------+
|               START                |
+------------------------------------+
                  |
                  v
+------------------------------------+
|           INITIALIZATION           |
| • Set GPIO Pins (18, 19, 23, 34,35)|
| • Start UART1 @ 115200 (TF-Luna)   |
| • Calibrate ESC to Neutral (1500us)|
| • Center Servo Angle to 90°        |
| • Clear PID State Variables        |
+------------------------------------+
                  |
                  v
+------------------------------------+
|       READ SAFETY INTERRUPT        |
|      Check E-Stop Relay Pin 23     |
+------------------------------------+
                  |
         /-----------------\
        /  Is E-Stop Tripped? \
        \    (Pin == HIGH)    /
         \-----------------/
            /           \
     YES   /             \   NO
          v               v
+-----------------------+ +------------------------------------+
|  EMERGENCY STOP LOOP  | |        STEP 1: SEE (PERCEPTION)    |
| • Send 1500us to ESC  | | • Read Left IR Sensor (Pin 34)     |
| • Center Servo (90°)  | | • Read Right IR Sensor (Pin 35)    |
| • Halt All Operations | | • Read Front TF-Luna LiDAR (UART)  |
+-----------------------+ +------------------------------------+
                                            |
                                            v
                                 /---------------------\
                                /  Front Distance < 30cm?\
                                \   (Obstacle Alert)    /
                                 \---------------------/
                                    /               \
                             YES   /                 \   NO
                                  v                   v
                        +-------------------+ +------------------------------------+
                        |  EMERGENCY BRAKE  | |       STEP 2: THINK (CONTROL)      |
                        | • Send 1500us ESC | | • Error = Left Dist - Right Dist   |
                        | • Steering = 90°  | | • Steering = 90° + PID(Error)      |
                        | • Pause 100ms     | | • Clamp Angle: 55° to 125°         |
                        +-------------------+ +------------------------------------+
                                  |                             |
                                  |                             v
                                  |           +------------------------------------+
                                  |           |       STEP 3: ACT (ACTUATION)      |
                                  |           | • Send PWM Angle to DS3218 Servo   |
                                  |           | • Send Drive PWM (1580us) to ESC   |
                                  |           +------------------------------------+
                                  |                             |
                                  |                             v
                                  |           +------------------------------------+
                                  |           |       DELAY 4ms (250Hz Loop)       |
                                  |           +------------------------------------+
                                  |                             |
                                  +-----------------------------+---> [Repeat Loop]
