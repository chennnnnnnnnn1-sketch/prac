START
  │
  ├── Initialize System
  │       ├── Load YOLOv8 Fire Detection Model
  │       ├── Initialize Pi Camera (640×480, manual exposure)
  │       ├── Initialize Ultrasonic Sensors (Front, Left, Right)
  │       ├── Initialize Motors and Buzzer
  │
  ├── Enter Continuous Control Loop
  │
  ├── Capture Live Video Frame (Real-Time)
  │
  ├── YOLOv8 Fire Detection
  │
  ├── Fire Detected? (Confidence ≥ 0.50)
  │        │
  │    ┌───┴────────┐
  │   YES           NO
  │    │             │
  │    │             ├── Read Ultrasonic Sensor Distances
  │    │             │       ├── Front Distance (cm)
  │    │             │       ├── Left Distance (cm)
  │    │             │       └── Right Distance (cm)
  │    │             │
  │    │             ├── Is Front Distance < 15 cm?
  │    │             │        │
  │    │             │    ┌───┴────────┐
  │    │             │   YES           NO
  │    │             │    │             │
  │    │             │    │             ├── Is Left Distance < 3 cm?
  │    │             │    │             │        │
  │    │             │    │             │    ┌───┴──────┐
  │    │             │    │             │   YES         NO
  │    │             │    │             │    │           │
  │    │             │    │             │    │           ├── Is Right Distance < 3 cm?
  │    │             │    │             │    │           │        │
  │    │             │    │             │    │           │    ┌───┴──────┐
  │    │             │    │             │    │           │   YES         NO
  │    │             │    │             │    │           │    │           │
  │    │             │    │             │    │           │    │           ├── Move Forward
  │    │             │    │             │    │           │    │           │   (Normal Patrol)
  │    │             │    │             │    │           │    │           │
  │    │             │    │             │    │           │    └── Adjust Left Slightly
  │    │             │    │             │    │           │         (Avoid Right Obstacle)
  │    │             │    │             │    │           │
  │    │             │    │             │    └── Adjust Right Slightly
  │    │             │    │             │         (Avoid Left Obstacle)
  │    │             │    │             │
  │    │             │    │             └── Move Forward
  │    │             │    │
  │    │             │    ├── STOP Motors
  │    │             │    ├── Reverse Slightly
  │    │             │    ├── Compare Left vs Right Distance
  │    │             │    └── Turn Toward Larger Distance
  │    │             │         (Clearer Path Selection)
  │    │             │
  │    │             └── Continue Loop
  │    │
  │    ├── STOP Motors Immediately
  │    ├── Activate Buzzer (Continuous Alert)
  │    ├── Lock Movement Permanently
  │    ├── Continue Real-Time Detection Display (VNC Viewer)
  │    │
  │    └── Loop Continues Until Program Exit
