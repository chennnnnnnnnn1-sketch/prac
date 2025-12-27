# AI-Based Autonomous Fire Detection Robot
An intelligent mobile robot that autonomously patrols, detects fire using YOLOv8 deep learning, and navigates toward fire sources while avoiding obstacles.

## I. Project Purpose

This project focuses on the development of an **AI-based mobile robot** capable of **autonomously detecting and locating fire** within an indoor environment. The system integrates **computer vision**, **artificial intelligence**, and **autonomous navigation** to identify fire sources and move toward them without human intervention.

A Raspberry Pi 4 Model B serves as the main controller, processing real-time video captured by a Raspberry Pi Camera Module. Fire detection is performed using a **YOLOv8 deep learning model**, enabling the robot to visually recognize flames with high accuracy. Upon detecting fire, the robot navigates toward the source, stops at a safe distance, and activates an audible and visual alert system.

While the current prototype focuses on **fire detection and alerting**, the platform is designed for future expansion, such as fire suppression modules or IoT-based emergency notifications.

---

## II. Technologies Used

### Hardware Components

| Component | Model/Specification | Purpose |
|-----------|-------------------|---------|
| **Main Controller** | Raspberry Pi 4 Model B (4GB RAM) | AI processing, motor control, sensor integration |
| **Camera** | Raspberry Pi Camera Module V2 (8MP) | Real-time video capture for fire detection |
| **Motor Driver** | L298N Dual H-Bridge | Controls DC motor direction and speed |
| **Motors** | 2× DC Geared Motors (6V, 200 RPM) | Robot locomotion |
| **Obstacle Sensors** | 3× HC-SR04 Ultrasonic Sensors | Front, left, and right obstacle detection |
| **Alert System** | Active Buzzer Module + LED | Audio-visual fire alert |
| **Power Supply** | 2× 18650 Li-ion Battery Pack (7.4V, 5200mAh) | Powers motors and electronics |
| **Chassis** | 4-Wheel Robot Chassis with Caster Wheel | Structural platform |


### Software & AI Technologies

| Category | Technology | Version/Details | Purpose |
|----------|------------|-----------------|---------|
| **AI Framework** | YOLOv8 (Ultralytics) | YOLOv8n (Nano) | Real-time fire detection with neural network |
| **Training Data** | Roboflow Fire Dataset | 90,000 images | Trained on fire, smoke, and negative samples |
| **Computer Vision** | OpenCV | 4.x | Image processing and frame handling |
| **Camera Library** | Picamera2 | Latest | Raspberry Pi Camera Module interface |
| **Programming** | Python | 3.11 | Core development language |
| **GPIO Control** | RPi.GPIO | Latest | Hardware pin control |
| **Remote Access** | RealVNC Viewer | Latest | Live monitoring and debugging |
| **AI Backend** | PyTorch | 2.x | Neural network inference engine |

### AI Model Training 
The YOLOv8 fire detection model used in this project was custom-trained on a large-scale dataset sourced and augmented using Roboflow. The training process involved approximately **90,000 labeled images**, ensuring detection performance across different fire scenarios.

---


## III. Proponents

**Project Leader:**  
- Obedencia, Phyq L.

**Project Members:**  
- Catian, Christine Ann P.
- Penoy, Arlyn C.

---


## IV.  Block Diagram and System Architecture

### System Overview

![System Block Diagram](images/block_diagram.png)


### Decision Flow
```
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
```



