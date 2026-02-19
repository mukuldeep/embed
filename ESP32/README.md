# Embedded & IoT Projects Collection

This repository contains multiple embedded systems and IoT projects built using microcontrollers such as **ESP32**, sensors, displays, and networking modules. Each project is organized into its own directory with source code, documentation, and required resources.

---

## Repository Structure
    ├── sketch_esp32s3_netSyncTime/
    ├── files/
    └── README.md

# ESP32-S3 Internet Synced Clock

<video src="./files/netSyncClock.mp4" controls></video>

An internet-connected digital clock built using the **ESP32-S3** microcontroller.  
The device automatically synchronizes time from an NTP (Network Time Protocol) server over WiFi.

### Features

- 📡 WiFi connectivity  
- 🌍 Automatic NTP time synchronization  
- ⏱ Real-time clock display  
- 🔄 Auto re-sync at configurable intervals  
- ⚡ Fast boot and low power capable  

### Hardware Requirements

- ESP32-S3 development board  
- Display module (iff not on board)  
- USB cable  
- Stable WiFi connection  

### Software Requirements

- Arduino IDE or PlatformIO  
- ESP32 board support package  
- Required libraries

## Setup Instructions

1. Clone the repository:

    `git clone https://github.com/mukuldeep/embed.git`


2. Open the `sketch_esp32s3_netSyncTime` project in Arduino IDE or PlatformIO.

3. Update WiFi credentials in the source code:
```cpp
const char* ssid = "YOUR_WIFI_NAME";
const char* password = "YOUR_WIFI_PASSWORD";
```

4. Select the correct ESP32-S3 board.
5. Upload the code to your device.

### Demo

Watch the clock syncing and running here: `./files/netSyncClock.mp4`