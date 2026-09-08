# Folder B: Designs (Mechanical, Electronic & Schematics)

This directory contains the physical blueprints, 3D CAD renders, Fritzing circuit schematics, hardware component justifications, safety interlock annotations, and Wokwi simulation testing for the **Robo Grand Prix** autonomous race car[cite: 1].

---

## 🛠️ Compliance & Universal Design Constraints

* **Maximum Footprint:** $25.7\text{ cm} \times 19.0\text{ cm}$ (Strictly within the maximum $50\text{ cm} \times 50\text{ cm}$ limit)[cite: 1].
* **Total Vehicle Weight:** ~ $1.15\text{ kg}$ (Strictly below the mandatory $5.0\text{ kg}$ limit)[cite: 1].
* **Safety Integration:** Features a dedicated physical ON/OFF power switch and an instant hardware Emergency Stop (E-Stop) mechanism[cite: 1].

---

## 🔌 Electronic Design & Fritzing Schematics

All circuit schematics, power routing, and signal netlists were engineered and verified using **Fritzing (EDA)**[cite: 1].

* **Fritzing Source File:** `docs/racecar_circuit.fzz` (Editable Fritzing project)[cite: 1].
* **Exported Schematic Diagram:** `docs/wiring_schematic.png` (High-resolution schematic showing $90^\circ$ orthogonal routing)[cite: 1].

### 🚨 Mandatory Safety Interlock Annotations (ON/OFF & E-Stop)
1. **Master ON/OFF Switch:** Placed directly on the primary positive lead between the $7.4\text{V}$ LiPo battery pack and the power bus bar to isolate the system completely[cite: 1].
2. **Hardware Emergency Stop (E-Stop):** A physical latching kill switch connected to `GPIO 23` (configured with internal `INPUT_PULLUP`) that instantly triggers a software and hardware interrupt to cut power to the 60A ESC and steering servo[cite: 1].

---

## 📌 Master Hardware Component Netlist & Justifications

| Component | Hardware Specification | ESP32 GPIO / Bus | Power Rail | Selection Justification |
| :--- | :--- | :--- | :--- | :--- |
| **Microcontroller** | ESP32-WROOM-32U | — | $5\text{V}$ Buck | Dual-core $240\text{ MHz}$ processing required for $250\text{ Hz}$ PID steering loops[cite: 1]. |
| **Steering Servo** | DS3218MG ($20\text{kg}$ Metal Gear) | `GPIO 18` | $5\text{V}$ Buck | $20\text{kg}\cdot\text{cm}$ torque prevents gear stripping under high-speed cornering[cite: 1]. |
| **Motor Drive** | 60A Brushless ESC | `GPIO 19` | Main Battery | High-current capability to handle rapid acceleration bursts without thermal throttling[cite: 1]. |
| **Safety E-Stop** | Physical Latching Switch | `GPIO 23` | Logic GND | Direct hardware interrupt to instantly disarm actuation[cite: 1]. |
| **Left Wall Sensor** | Sharp GP2Y0A21YK0F Analog IR | `GPIO 34` | $5\text{V}$ Buck | High-frequency analog proximity detection ($10\text{ cm}\text{--}80\text{ cm}$)[cite: 1]. |
| **Right Wall Sensor** | Sharp GP2Y0A21YK0F Analog IR | `GPIO 35` | $5\text{V}$ Buck | High-frequency analog proximity detection ($10\text{ cm}\text{--}80\text{ cm}$)[cite: 1]. |
| **Front LiDAR** | TF-Luna Micro LiDAR | `GPIO 16` (RX2)<br>`GPIO 17` (TX2) | $5\text{V}$ Buck | Serial UART ($115.2\text{k}$ baud) scanning up to $8\text{ meters}$ ahead for forward obstacles[cite: 1]. |

---

## 🚗 Mechanical Design, Fabrication & Kinematics

* **Chassis Architecture:** 1:10 scale RWD platform ($257\text{ mm}$ wheelbase, $190\text{ mm}$ track width) utilizing composite plate construction[cite: 1].
* **Mass Distribution:** Optimized **45% Front / 55% Rear** mass balance with a low center of gravity ($15\text{ mm}$ off deck) to maintain rear wheel traction during throttle bursts[cite: 1].
* **Steering Kinematics:** Ackerman steering geometry driven by direct M3 aluminum tie-rods to prevent tire scrub during cornering[cite: 1].
* **3D Printed Mounts (`cad/sensor_mounts/`):**
  * **LiDAR Tower (`lidar_mount.stl`):** Elevates TF-Luna $100\text{ mm}$ above track level at $0^\circ$ pitch to eliminate ground reflections[cite: 1].
  * **IR Brackets (`ir_brackets.stl`):** Positions side IR sensors at a **$15^\circ$ toe-out angle** for early wall detection in turns[cite: 1].


## 📁 Folder B Directory Structure

```text
Folder_B/
├── README.md                      <-- Master Designs Overview (This File)[cite: 1]
├── cad/
│   ├── chassis_assembly.step     <-- Master 3D CAD Assembly File[cite: 1]
│   └── sensor_mounts/
│       ├── lidar_tower_mount.stl <-- 100mm Elevated LiDAR Tower Mount[cite: 1]
│       ├── left_ir_bracket.stl   <-- 15° Toe-out Left IR Mount[cite: 1]
│       └── right_ir_bracket.stl  <-- 15° Toe-out Right IR Mount[cite: 1]
└── docs/
    ├── mechanical_design.md       <-- Detailed Fabrication & Kinematics Report[cite: 1]
    ├── electronic_design.md       <-- Component Justifications & Power Distribution[cite: 1]
    ├── simulation.md              <-- Wokwi Virtual Test Results & Logs[cite: 1]
    ├── racecar_circuit.fzz        <-- Editable Fritzing Source CAD File[cite: 1]
    └── wiring_schematic.png       <-- ANNOTATED Fritzing Diagram (ON/OFF & E-Stop)[cite: 1]
