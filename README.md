# ESP32-CAM-Intrusion-Snapshot-System

## Project Overview

The purpose of this project was to demonstrate how IoT camera devices can be used for automated monitoring and event logging within a cybersecurity environment. Using the ESP32-CAM, the system automatically captures image snapshots at scheduled intervals and records activity through the Arduino Serial Monitor. The project simulates a lightweight intrusion monitoring system capable of documenting activity occurring inside a monitored environment.

From a cybersecurity perspective, this project demonstrates automated surveillance, IoT device deployment, event logging, and embedded monitoring systems. The ESP32-CAM functions as a simple security monitoring device that continuously captures snapshots and logs system activity automatically.

The project also provided hands-on experience with:

* Arduino IDE configuration
* ESP32 board setup
* Camera initialization
* Serial monitoring
* Hardware/software troubleshooting
* IoT security device implementation

---
# Equipment and Resources

## Hardware Used

* ESP32-CAM (AI Thinker model)
* USB data cable
* Laptop/computer

## Software Used

* Arduino IDE
* ESP32 Board Package by Espressif Systems

## Resources and Documentation
* ChatGPT for Documentation
* ESP32-CAM CameraWebServer Example
* Arduino IDE ESP32 Board Manager
* ESP32-CAM documentation and troubleshooting guides

---
# Step-by-Step Installation and Configuration

## Step 1 — Install ESP32 Board Support

Inside Arduino IDE:

1. Go to:

```text
File → Preferences
```

2. In “Additional Boards Manager URLs,” add:
https://dl.espressif.com/dl/package_esp32_index.json

3. Click OK.

4. Go to:
Tools → Board → Boards Manager

5. Search:
ESP32

7. Install:
esp32 by Espressif Systems
`
## Step 2 — Connect the ESP32-CAM

1. Connect the ESP32-CAM to the laptop using a USB data cable.
2. Wait for the device to power on.
3. Verify that the COM port appears in Arduino IDE.

If the board is not detected:

* try another USB cable
* install CH340 drivers
* reconnect the device


## Step 3 — Configure Arduino IDE

Inside Arduino IDE:

1. Go to:

Tools → Board

2. Select:
AI Thinker ESP32-CAM

3. Set:
Upload Speed → 115200

4. Select the correct COM port under:
Tools → Port


# ESP32-CAM Snapshot Capture Code

#include "esp_camera.h"

// AI Thinker ESP32-CAM pin definitions
#define PWDN_GPIO_NUM     32
#define RESET_GPIO_NUM    -1
#define XCLK_GPIO_NUM      0
#define SIOD_GPIO_NUM     26
#define SIOC_GPIO_NUM     27

#define Y9_GPIO_NUM       35
#define Y8_GPIO_NUM       34
#define Y7_GPIO_NUM       39
#define Y6_GPIO_NUM       36
#define Y5_GPIO_NUM       21
#define Y4_GPIO_NUM       19
#define Y3_GPIO_NUM       18
#define Y2_GPIO_NUM        5

#define VSYNC_GPIO_NUM    25
#define HREF_GPIO_NUM     23
#define PCLK_GPIO_NUM     22

int snapshotNumber = 1;

void setup() {

  Serial.begin(115200);
  Serial.println();
  Serial.println("ESP32-CAM Automated Intrusion Snapshot System Starting...");

  camera_config_t config;

  config.ledc_channel = LEDC_CHANNEL_0;
  config.ledc_timer = LEDC_TIMER_0;

  config.pin_d0 = Y2_GPIO_NUM;
  config.pin_d1 = Y3_GPIO_NUM;
  config.pin_d2 = Y4_GPIO_NUM;
  config.pin_d3 = Y5_GPIO_NUM;
  config.pin_d4 = Y6_GPIO_NUM;
  config.pin_d5 = Y7_GPIO_NUM;
  config.pin_d6 = Y8_GPIO_NUM;
  config.pin_d7 = Y9_GPIO_NUM;

  config.pin_xclk = XCLK_GPIO_NUM;
  config.pin_pclk = PCLK_GPIO_NUM;
  config.pin_vsync = VSYNC_GPIO_NUM;
  config.pin_href = HREF_GPIO_NUM;

  config.pin_sccb_sda = SIOD_GPIO_NUM;
  config.pin_sccb_scl = SIOC_GPIO_NUM;

  config.pin_pwdn = PWDN_GPIO_NUM;
  config.pin_reset = RESET_GPIO_NUM;

  config.xclk_freq_hz = 20000000;
  config.pixel_format = PIXFORMAT_JPEG;

  config.frame_size = FRAMESIZE_QVGA;
  config.jpeg_quality = 12;
  config.fb_count = 1;

  config.fb_location = CAMERA_FB_IN_DRAM;
  config.grab_mode = CAMERA_GRAB_WHEN_EMPTY;

  esp_err_t err = esp_camera_init(&config);

  if (err != ESP_OK) {
    Serial.printf("Camera initialization failed with error 0x%x\n", err);
    Serial.println("Check camera ribbon cable, board settings, and power.");
    return;
  }

  Serial.println("Camera initialized successfully.");
  Serial.println("System monitoring started.");
}

void loop() {

  camera_fb_t *fb = esp_camera_fb_get();

  if (!fb) {
    Serial.println("Photo capture failed.");
    delay(5000);
    return;
  }

  Serial.print("Snapshot ");
  Serial.print(snapshotNumber);
  Serial.println(" captured successfully.");

  esp_camera_fb_return(fb);

  snapshotNumber++;

  delay(5000);
}
```

---

# Explanation of the Code

The program begins by importing the ESP32 camera library and defining the camera pin layout for the AI Thinker ESP32-CAM board. Inside the setup function, the Serial Monitor is initialized so the system can log activity and debugging messages.

The camera configuration settings define:

* image resolution
* JPEG quality
* camera communication pins
* memory usage settings

The `esp_camera_init()` function initializes the camera hardware. If initialization fails, the Serial Monitor displays an error message to help troubleshoot hardware or configuration problems.

Inside the loop function, the ESP32-CAM automatically captures snapshots every five seconds using:

camera_fb_t *fb = esp_camera_fb_get();


Each successful snapshot is logged to the Serial Monitor, simulating event logging and automated surveillance activity within a cybersecurity monitoring environment.

# Uploading the Program

1. Click Upload in Arduino IDE.
2. If the upload hangs on:
Connecting...
```

press the RESET button on the ESP32-CAM once.

3. Wait for upload completion.


# Opening the Serial Monitor

1. Go to:
Tools → Serial Monitor

2. Set baud rate:
115200

3. Verify successful output such as:
Camera initialized successfully
Snapshot 1 captured successfully
Snapshot 2 captured successfully


---

# Troubleshooting Notes and Challenges Encountered

## Problem 1 — Upload Stuck on “Connecting...”

### Issue

The ESP32-CAM failed to upload code and remained stuck on:
Connecting...


### Solution

Pressed the RESET button while the upload process was attempting to connect. This forced the board into upload mode and allowed the code to upload successfully.

---

## Problem 2 — Black Screen During Streaming

### Issue

The camera webpage loaded correctly, but the live stream displayed only a black image.

### Solution

The camera ribbon cable was checked and reseated to ensure proper connection. The correct AI Thinker camera model configuration was also verified in the code.

---

## Problem 3 — Missing `camera_pins.h` File

### Issue

Arduino IDE displayed:

fatal error: camera_pins.h: No such file or directory

### Solution

The issue occurred because the sketch was missing required camera files. The AI Thinker camera pin definitions were directly embedded into the sketch itself, eliminating the dependency on the missing file.



# Results

After troubleshooting and configuring the hardware and software correctly, the ESP32-CAM successfully initialized and captured snapshots automatically every five seconds. The Serial Monitor continuously displayed successful snapshot capture logs, confirming that the monitoring system was functioning properly.

Example output:

Camera initialized successfully
Snapshot 1 captured successfully
Snapshot 2 captured successfully

The project demonstrated how embedded IoT devices can support automated monitoring, event logging, and basic surveillance operations within a cybersecurity-focused environment.


# Conclusion

This project successfully demonstrated how a low-cost IoT camera device can be used for automated monitoring and event logging in a cybersecurity environment. The ESP32-CAM was configured to initialize the camera, capture snapshots automatically, and log monitoring activity through the Serial Monitor. Throughout the project, several hardware and software challenges were encountered, including upload issues, camera streaming problems, and missing file errors. Troubleshooting these problems improved understanding of embedded systems, IoT device deployment, and hardware/software integration. In summary, the project showed how embedded devices can support cybersecurity monitoring and surveillance operations through automated image capture and logging functionality.
