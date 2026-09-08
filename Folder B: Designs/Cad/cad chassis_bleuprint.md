Technical Drawing & Mechanical BlueprintProject: Robo Grand Prix Autonomous Race Car  Drawing Title: 1:10 Scale RWD Chassis Blueprint & Assembly Layout  Units: Millimeters ($\text{mm}$) | Scale: 1:2 | Projection: Third Angle ($\text{ISO}$)  Max Footprint: $257\text{ mm} \times 190\text{ mm} \times 130\text{ mm}$ (Compliant with $50\text{ cm} \times 50\text{ cm}$ rule)  Total Weight: $1.15\text{ kg}$ (Compliant with $< 5.0\text{ kg}$ limit)  

1. Top Plan View & Component Dimensioning
|<──────────────────── Overall Length: 257 mm ───────────────────>|

       +-----------------------------------------------------------------+  ---
       | [Left IR Sensor]       [TF-Luna LiDAR]       [Right IR Sensor]  |   ^
       |   (15° Toe-out)         (100mm Tower)         (15° Toe-out)     |   |
       +-------+-----------------------+-----------------------+---------+   |
               |                       |                       |             |
           +---+-----------------------+-----------------------+---+         |
           | [O]                                               [O] |         |
           |  |                                                 |  |         |
           |  | <============== [Tie Rod Linkage] ============> |  |         |
           |  |                                                 |  |         |
           | [O] (Front Wheel)                         (Front Wheel)[O] |         |
           +---+-----------------------+-----------------------+---+         |
               |              [DS3218MG Servo]                 |             |
               |                 (Centerline)                  |             | Overall
               |                                               |             | Width:
               |             [ESP32 Development Board]         |             | 190 mm
               |              & 5V Buck Converter              |             |
               |                                               |             |
               |             [7.4V 2S LiPo Battery]            |             |
               |              (Low Center Floor)               |             |
               |                                               |             |
           +---+-----------------------+-----------------------+---+         |
           | [O]                                               [O] |         |
           |  |              [60A Brushless ESC]                |  |         |
           |  |                         |                       |  |         |
           |  |                     [Rear Axle]                 |  |         |
           | [O] (Rear Drive)                             (Rear Drive)[O] |         v
           +---+-----------------------------------------------+---+        ---
               |<------------ Wheelbase: 257 mm -------------->|

2. Side Elevation View & Height Dimensioning

|<─────────────────────── 257 mm ────────────────────────>|

       +-----------------------+                                             --- 130 mm (Max Height)
       |  TF-Luna LiDAR Tower  |                                              ^
       +-----------+-----------+                                              |
                   |                                                          | 100 mm Elevation
                   |  [Upper Deck]                                            v
       +-----------+---------------+---------------------------+             ---
       |  Left/Right IR Sensors    |  ESP32 / Buck / ESC       |              ^
       +===========================+===========================+              | 35 mm Elevation
       |                  [7.4V LiPo Battery]                  | (Chassis Floor)
       +-------+---------------------------------------+-------+             ---
         (O)   |                                       |   (O)                ^ 12 mm Ground Clearance
     [Front Wheel]                                  [Rear Wheel]             v
  ===========================================================================--- (Track Surface)
3. Engineering Dimension Summary Table
Dimension ParameterValue (mm)Tolerance (mm)Engineering Justification
Overall Length$257\text{ mm}$$\pm 1.0\text{ mm}$Fits well within the $500\text{ mm}$ competition constraint.
Overall Width$190\text{ mm}$$\pm 1.0\text{ mm}$Standard 1:10 scale track width for high-speed lateral stability.
Total Height$130\text{ mm}$$\pm 2.0\text{ mm}$Measured from track surface to the top of the LiDAR tower.
Ground Clearance$12\text{ mm}$$\pm 0.5\text{ mm}$Lowers center of gravity while clearing carpet or track joints.
LiDAR Elevation$100\text{ mm}$$\pm 1.0\text{ mm}$Eliminates ground surface reflections for clear $8\text{m}$ forward scanning.
IR Sensor Toe-Out$15^\circ$ Angle$\pm 0.5^\circ$Angles side sensors outward to detect approaching walls during corner entry.
