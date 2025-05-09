# RFID-Controlled Line Follower Robot with WebSocket Communication (ESP32)

## Overview
This project implements an advanced **ESP32-based line following robot** that:
- **Starts** following a line upon scanning an RFID tag,
- **Pauses** when the same RFID tag is scanned again,
- **Sends RFID scan data** live via **WebSocket** to a remote server for real-time monitoring,
- **Handles motor control** and **sensor processing** separately on both CPU cores for maximum performance.

## Features
- **Dual-Core Tasking**:
  - Core 0: Handles **WiFi connection**, **WebSocket client**, and **RFID scanning**.
  - Core 1: Handles **sensor reading** and **PID-based motor control** for line following.
- **RFID Triggered Operation**:
  - First scan: Starts line following.
  - Same scan later: Pauses the robot for a few seconds.
- **WebSocket Communication**:
  - Sends the scanned RFID UID as JSON to a remote WebSocket server.
- **Robust Line Following**:
  - Uses **8 analog QTR sensors** for detecting the line.
  - **PID control** for smooth and stable movement.
  - Automatic recovery if the line is lost (spins to search for the line).

## Materials Used
- **ESP32 Development Board**  
- **QTR-8RA Reflectance Sensor Array**  
- **MFRC522 RFID Reader**  
- **L298N Motor Driver**  
- **Basic DC Motors** (x2)  
- **Wheels** (x2)  
- **Power Supply (Battery Pack)**  
- **Chassis or Robot Frame**  
- **Jumper Wires and Breadboard** (optional for easier wiring)

## Software Requirements
- **Arduino IDE**
- ESP32 Board Package installed
- Libraries:
  - `WiFi.h`
  - `WebSocketsClient.h`
  - `SPI.h`
  - `MFRC522.h`
  - `QTRSensors.h`

## How it Works
1. **WiFi Connect**: ESP32 connects to the specified WiFi network.
2. **WebSocket Connect**: Establishes a live WebSocket connection with a remote server.
3. **Wait for RFID**: The robot waits for an RFID tag scan to begin moving.
4. **Line Following**: After a successful scan, the robot starts following the line using PID control.
5. **Pause on Second Scan**: If the same RFID tag is scanned again, the robot pauses for a few seconds and then resumes.
6. **Data Sending**: Every scanned RFID UID is sent to the server through WebSocket.

## Project Structure
- `TaskWebSocketRFID()`: Connects to WiFi, maintains WebSocket communication, and reads RFID.
- `TaskLineFollow()`: Controls motors using PID logic based on QTR sensor readings.
- `setup()`: Initializes both tasks on different cores.
- `loop()`: Remains empty, as tasks are handled independently.

