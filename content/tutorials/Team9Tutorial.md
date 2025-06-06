---  
title: A Comprehensive Guide to Performing Computer Vision Tasks with ESP32-CAM Module  
date: 05-19-2025  
authors:  
  - name: Purab Balani  
tags: [ESP32-CAM, Computer Vision, IoT, YOLO, Object Detection, Python, OpenCV]  
---  

![ESP32-CAM](https://github.com/user-attachments/assets/b54308e5-dd03-415c-a266-4db8675e2418)

## Introduction

The ESP32-CAM is a powerful yet compact module combining a microcontroller, Wi-Fi, and a camera, making it ideal for IoT and computer vision tasks. In this tutorial, we will stream video from the ESP32-CAM to a host computer and run object detection using a YOLO model.

### Learning Objectives

- Configure the ESP32-CAM module  
- Stream video over Wi-Fi to a host machine  
- Use OpenCV in Python to capture and display frames  
- Run object detection with a YOLO model using Roboflow Inference  
- Trigger audio alerts with text-to-speech (TTS)  

### Background Information

Computer vision enables machines to understand images and video. The ESP32-CAM cannot run large models directly, but it can stream footage that a host device processes using powerful libraries like OpenCV and YOLO.

## Getting Started

### Required Downloads and Installations

| Software          | Description                          | Installation                                                                 |
|-------------------|--------------------------------------|------------------------------------------------------------------------------|
| Arduino IDE       | Upload firmware to ESP32-CAM         | [Download](https://www.arduino.cc/en/software)                              |
| ESP32 Board Support | Adds ESP32 support to Arduino IDE    | [Guide](https://randomnerdtutorials.com/installing-the-esp32-board-in-arduino-ide-windows-instructions/) |
| Python 3.x        | Required for running detection script | [Download](https://www.python.org/)                                         |
| OpenCV            | Image capture & processing            | `pip install opencv-python`                                                 |
| pyttsx3           | Text-to-speech engine                 | `pip install pyttsx3`                                                       |
| inference         | Roboflow's inference SDK              | `pip install inference`                                                     |

### Required Components

| Component Name           | Quantity |  
|--------------------------|----------|  
| ESP32-S3 CAM             | 1        |  
| USB Cable (for flashing) | 1        |  
| Power Supply             | 1        |  

### Required Tools and Equipment

- Host PC (Linux/Windows/Mac)  
- Wi-Fi network  
- Breadboard (optional for peripherals)  

---

## Part 01: Streaming Video from ESP32-CAM

### Objective  
Flash ESP32-CAM and stream live video via Wi-Fi.

### Instructional Steps

1. Open Arduino IDE.  
2. Install the ESP32 board support and select `XIAO_ESP32S3`.  
3. Use the provided ESP32-CAM code to flash the board.  
4. Connect to the printed IP address and confirm video stream.  

### ESP32-CAM Firmware Code  
<details>  
<summary>Click to view</summary>  

```cpp
// #include "esp_camera.h"
// #include <WiFi.h>
// #include "esp_system.h"
// #include "esp_chip_info.h"

// #define CAMERA_MODEL_XIAO_ESP32S3 // Has PSRAM
// #include "camera_pins.h"

// const char *ssid = "RESNET-GUEST-DEVICE";
// const char *password = "ResnetConnect";

// void startCameraServer();
// void setupLedFlash(int pin);

// void setup() {
//   Serial.begin(115200);
//   Serial.setDebugOutput(true);
//   delay(2000); // Give time for serial monitor to connect
  
//   Serial.println("\n\n=== ESP32 Camera WiFi Debug ===");
  
//   // Basic chip info using ESP32 Arduino methods
//   Serial.printf("Chip Model: ESP32 variant\n");
//   Serial.printf("Chip Revision: %d\n", ESP.getChipRevision());
//   Serial.printf("CPU Frequency: %d MHz\n", ESP.getCpuFreqMHz());
//   Serial.printf("Flash Size: %d bytes\n", ESP.getFlashChipSize());
//   Serial.printf("Free Heap: %d bytes\n", ESP.getFreeHeap());
  
//   // Check PSRAM
//   if (psramFound()) {
//     Serial.printf("PSRAM: Found, size: %d bytes\n", ESP.getPsramSize());
//   } else {
//     Serial.println("PSRAM: Not found");
//   }
  
//   // Get MAC using WiFi library method
//   Serial.println("\nInitializing WiFi...");
//   WiFi.mode(WIFI_STA);
//   delay(100);
  
//   Serial.print("WiFi MAC address: ");
//   Serial.println(WiFi.macAddress());
  
//   // If MAC is still zeros, WiFi hardware has issues
//   if (WiFi.macAddress() == "00:00:00:00:00:00") {
//     Serial.println("CRITICAL ERROR: WiFi hardware not responding!");
//     Serial.println("Check:");
//     Serial.println("1. Board selection in Arduino IDE");
//     Serial.println("2. Flash settings");
//     Serial.println("3. Hardware may be defective");
//     Serial.println("Stopping here - fix WiFi hardware first");
//     while(1) delay(1000); // Stop execution
//   }
  
//   // Camera initialization
//   Serial.println("\nInitializing camera...");
//   camera_config_t config;
//   config.ledc_channel = LEDC_CHANNEL_0;
//   config.ledc_timer = LEDC_TIMER_0;
//   config.pin_d0 = Y2_GPIO_NUM;
//   config.pin_d1 = Y3_GPIO_NUM;
//   config.pin_d2 = Y4_GPIO_NUM;
//   config.pin_d3 = Y5_GPIO_NUM;
//   config.pin_d4 = Y6_GPIO_NUM;
//   config.pin_d5 = Y7_GPIO_NUM;
//   config.pin_d6 = Y8_GPIO_NUM;
//   config.pin_d7 = Y9_GPIO_NUM;
//   config.pin_xclk = XCLK_GPIO_NUM;
//   config.pin_pclk = PCLK_GPIO_NUM;
//   config.pin_vsync = VSYNC_GPIO_NUM;
//   config.pin_href = HREF_GPIO_NUM;
//   config.pin_sccb_sda = SIOD_GPIO_NUM;
//   config.pin_sccb_scl = SIOC_GPIO_NUM;
//   config.pin_pwdn = PWDN_GPIO_NUM;
//   config.pin_reset = RESET_GPIO_NUM;
//   config.xclk_freq_hz = 20000000;
//   config.frame_size = FRAMESIZE_UXGA;
//   config.pixel_format = PIXFORMAT_JPEG;
//   config.grab_mode = CAMERA_GRAB_WHEN_EMPTY;
//   config.fb_location = CAMERA_FB_IN_PSRAM;
//   config.jpeg_quality = 12;
//   config.fb_count = 1;

//   if (config.pixel_format == PIXFORMAT_JPEG) {
//     if (psramFound()) {
//       config.jpeg_quality = 10;
//       config.fb_count = 2;
//       config.grab_mode = CAMERA_GRAB_LATEST;
//     } else {
//       config.frame_size = FRAMESIZE_SVGA;
//       config.fb_location = CAMERA_FB_IN_DRAM;
//     }
//   } else {
//     config.frame_size = FRAMESIZE_240X240;
// #if CONFIG_IDF_TARGET_ESP32S3
//     config.fb_count = 2;
// #endif
//   }

//   esp_err_t err = esp_camera_init(&config);
//   if (err != ESP_OK) {
//     Serial.printf("Camera init failed with error 0x%x\n", err);
//     return;
//   }
//   Serial.println("Camera initialized successfully!");

//   sensor_t *s = esp_camera_sensor_get();
//   if (s->id.PID == OV3660_PID) {
//     s->set_vflip(s, 1);
//     s->set_brightness(s, 1);
//     s->set_saturation(s, -2);
//   }
//   if (config.pixel_format == PIXFORMAT_JPEG) {
//     s->set_framesize(s, FRAMESIZE_QVGA);
//   }

// #if defined(CAMERA_MODEL_ESP32S3_EYE)
//   s->set_vflip(s, 1);
// #endif

// #if defined(LED_GPIO_NUM)
//   setupLedFlash(LED_GPIO_NUM);
// #endif

//   // Network scanning and connection
//   Serial.println("\n=== WiFi Connection Process ===");
//   WiFi.disconnect();
//   delay(100);
  
//   Serial.println("Scanning for networks...");
//   int n = WiFi.scanNetworks();
//   Serial.printf("Found %d networks:\n", n);
  
//   bool targetFound = false;
//   for (int i = 0; i < n; ++i) {
//     Serial.printf("%d: %s (%d dBm) %s\n", 
//                   i + 1, 
//                   WiFi.SSID(i).c_str(), 
//                   WiFi.RSSI(i),
//                   (WiFi.encryptionType(i) == WIFI_AUTH_OPEN) ? "[OPEN]" : "[SECURED]");
//     if (WiFi.SSID(i) == ssid) {
//       targetFound = true;
//       Serial.printf("*** Target network '%s' found with signal %d dBm\n", ssid, WiFi.RSSI(i));
//     }
//   }
  
//   if (!targetFound) {
//     Serial.printf("ERROR: Target network '%s' not found in scan!\n", ssid);
//     Serial.println("Available networks listed above. Check network name exactly.");
//     return;
//   }
  
//   Serial.printf("Connecting to '%s'...\n", ssid);
//   WiFi.begin(ssid, password);
  
//   int attempts = 0;
//   while (WiFi.status() != WL_CONNECTED && attempts < 30) {
//     delay(1000);
//     attempts++;
    
//     wl_status_t status = WiFi.status();
//     Serial.printf("Attempt %d: ", attempts);
    
//     switch(status) {
//       case WL_IDLE_STATUS: Serial.println("IDLE"); break;
//       case WL_NO_SSID_AVAIL: Serial.println("NO_SSID - Network not found"); break;
//       case WL_SCAN_COMPLETED: Serial.println("SCAN_COMPLETED"); break;
//       case WL_CONNECTED: Serial.println("CONNECTED!"); break;
//       case WL_CONNECT_FAILED: Serial.println("CONNECT_FAILED - Wrong password?"); break;
//       case WL_CONNECTION_LOST: Serial.println("CONNECTION_LOST"); break;
//       case WL_DISCONNECTED: Serial.println("DISCONNECTED"); break;
//       default: Serial.printf("UNKNOWN_STATUS_%d\n", status); break;
//     }
    
//     // Try reconnecting every 10 attempts
//     if (attempts % 10 == 0 && status != WL_CONNECTED) {
//       Serial.println("Retrying connection...");
//       WiFi.disconnect();
//       delay(1000);
//       WiFi.begin(ssid, password);
//     }
//   }
  
//   if (WiFi.status() != WL_CONNECTED) {
//     Serial.println("\n*** FAILED TO CONNECT ***");
//     Serial.println("Possible issues:");
//     Serial.println("1. Wrong password");
//     Serial.println("2. Network requires additional authentication");
//     Serial.println("3. MAC address filtering");
//     Serial.println("4. Network is 5GHz only");
//     return;
//   }
  
//   Serial.println("\n*** WiFi Connected Successfully! ***");
//   Serial.print("IP address: ");
//   Serial.println(WiFi.localIP());
//   Serial.print("Gateway: ");
//   Serial.println(WiFi.gatewayIP());
//   Serial.print("DNS: ");
//   Serial.println(WiFi.dnsIP());
//   Serial.print("Signal strength: ");
//   Serial.print(WiFi.RSSI());
//   Serial.println(" dBm");

//   startCameraServer();

//   Serial.print("\nCamera server ready! Go to: http://");
//   Serial.println(WiFi.localIP());
// }

// void loop() {
//   // Print connection status every 30 seconds
//   static unsigned long lastCheck = 0;
//   if (millis() - lastCheck > 30000) {
//     lastCheck = millis();
//     if (WiFi.status() == WL_CONNECTED) {
//       Serial.printf("WiFi OK - IP: %s, Signal: %d dBm\n", 
//                     WiFi.localIP().toString().c_str(), WiFi.RSSI());
//     } else {
//       Serial.println("WiFi connection lost!");
//     }
//   }
//   delay(1000);
// }
















#include "esp_camera.h"
#include <WiFi.h>

#define CAMERA_MODEL_XIAO_ESP32S3
#include "camera_pins.h"

const char *ssid = "RESNET-GUEST-DEVICE";
const char *password = "ResnetConnect";

void startCameraServer();
void setupLedFlash(int pin);

void setup() {
  Serial.begin(115200);
  Serial.setDebugOutput(false); // Disable debug output for better performance
  
  // Optimize CPU frequency
  setCpuFrequencyMhz(240); // Max frequency for ESP32S3
  
  Serial.println("Starting optimized camera setup...");

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
  
  // Optimized camera settings for performance
  config.xclk_freq_hz = 20000000;
  config.pixel_format = PIXFORMAT_JPEG;
  config.grab_mode = CAMERA_GRAB_LATEST; // Always get latest frame
  config.fb_location = CAMERA_FB_IN_PSRAM;
  
  // Performance optimized settings
  if (psramFound()) {
    Serial.println("PSRAM found - using optimized settings");
    config.frame_size = FRAMESIZE_QQVGA;    // 800x600 - good balance
    config.jpeg_quality = 12;              // Lower quality = faster
    config.fb_count = 2;                   // Double buffering
  } else {
    Serial.println("No PSRAM - using conservative settings");
    config.frame_size = FRAMESIZE_QQVGA;     // 640x480
    config.jpeg_quality = 15;
    config.fb_count = 1;
    config.fb_location = CAMERA_FB_IN_DRAM;
  }

  // Initialize camera
  esp_err_t err = esp_camera_init(&config);
  if (err != ESP_OK) {
    Serial.printf("Camera init failed with error 0x%x", err);
    return;
  }

  // Get camera sensor for optimization
  sensor_t *s = esp_camera_sensor_get();
  
  // Optimize sensor settings for speed
  s->set_framesize(s, FRAMESIZE_QQVGA);     // Start with VGA for speed
  s->set_quality(s, 12);                  // JPEG quality (lower = faster)
  
  // Image enhancement settings
  s->set_brightness(s, 0);     // -2 to 2
  s->set_contrast(s, 0);       // -2 to 2
  s->set_saturation(s, 0);     // -2 to 2
  s->set_special_effect(s, 0); // 0 to 6 (0=No Effect)
  s->set_whitebal(s, 1);       // 0 = disable , 1 = enable
  s->set_awb_gain(s, 1);       // 0 = disable , 1 = enable
  s->set_wb_mode(s, 0);        // 0 to 4 - if awb_gain enabled
  s->set_exposure_ctrl(s, 1);  // 0 = disable , 1 = enable
  s->set_aec2(s, 0);           // 0 = disable , 1 = enable
  s->set_ae_level(s, 0);       // -2 to 2
  s->set_aec_value(s, 300);    // 0 to 1200
  s->set_gain_ctrl(s, 1);      // 0 = disable , 1 = enable
  s->set_agc_gain(s, 0);       // 0 to 30
  s->set_gainceiling(s, (gainceiling_t)0); // 0 to 6
  s->set_bpc(s, 0);            // 0 = disable , 1 = enable
  s->set_wpc(s, 1);            // 0 = disable , 1 = enable
  s->set_raw_gma(s, 1);        // 0 = disable , 1 = enable
  s->set_lenc(s, 1);           // 0 = disable , 1 = enable
  s->set_hmirror(s, 0);        // 0 = disable , 1 = enable
  s->set_vflip(s, 0);          // 0 = disable , 1 = enable
  s->set_dcw(s, 1);            // 0 = disable , 1 = enable
  s->set_colorbar(s, 0);       // 0 = disable , 1 = enable

  // Camera model specific optimizations
#if defined(CAMERA_MODEL_XIAO_ESP32S3)
  // No specific flips needed for XIAO ESP32S3
#endif

#if defined(LED_GPIO_NUM)
  setupLedFlash(LED_GPIO_NUM);
#endif

  // WiFi setup with optimizations
  WiFi.mode(WIFI_STA);
  WiFi.setSleep(false); // Disable WiFi sleep for consistent performance
  WiFi.setTxPower(WIFI_POWER_19_5dBm); // Max WiFi power
  
  Serial.printf("Connecting to %s", ssid);
  WiFi.begin(ssid, password);
  
  while (WiFi.status() != WL_CONNECTED) {
    delay(500);
    Serial.print(".");
  }
  Serial.println("");
  Serial.println("WiFi connected");

  startCameraServer();

  Serial.print("Camera Ready! Use 'http://");
  Serial.print(WiFi.localIP());
  Serial.println("' to connect");
  
  // Print optimization info
  Serial.println("\nOptimization Settings Applied:");
  Serial.printf("CPU Frequency: %d MHz\n", getCpuFrequencyMhz());
  Serial.printf("PSRAM Available: %s\n", psramFound() ? "Yes" : "No");
  Serial.printf("Frame Size: %s\n", psramFound() ? "QQVGA" : "QQVGA");
  Serial.printf("JPEG Quality: %d\n", psramFound() ? 12 : 15);
  Serial.printf("Frame Buffers: %d\n", psramFound() ? 2 : 1);
}

void loop() {
  // Keep loop minimal for best performance
  delay(1);
}

```  

</details>  

---

## Part 02: Capturing and Displaying Frames on Host

### Objective  
Connect to ESP32 stream and show real-time video using OpenCV.

### Instructional Steps

1. Use the ESP32 IP (e.g., `http://<IP>:81/stream`) in your Python code.  
2. Use `cv2.VideoCapture()` to connect and read frames.  

---

## Part 03: Performing Object Detection

### Objective  
Use Roboflow’s YOLO model to detect hazards in real-time.

### Code Walkthrough

- **Model Setup**: Using `get_model()` from Roboflow SDK  
- **Detection**: `model.infer(frame)` returns bounding boxes and classes  
- **Annotation**: Use `supervision` library to draw boxes and labels  
- **TTS**: pyttsx3 announces hazards with cooldown logic  

### Full Detection Script  
<details>  
<summary>Click to view Python code</summary>  

```python
import cv2
from inference import get_model
import supervision as sv
import pyttsx3
import time

STREAM_URL = "http://100.117.8.43:81/stream"

def grab_latest_frame(cap, flush_count=4):
    frame = None
    for _ in range(flush_count):
        ret, f = cap.read()
        if not ret:
            break
        frame = f
    return frame

def open_stream():
    cap = cv2.VideoCapture(STREAM_URL)
    if not cap.isOpened():
        print("Failed to open stream!")
        return None
    return cap

cap = open_stream()
if cap is None:
    raise RuntimeError("Could not open ESP32 stream.")

model = get_model(model_id="196v2/1")
box_annotator = sv.BoxAnnotator()
label_annotator = sv.LabelAnnotator()
class_names = model.class_names
engine = pyttsx3.init()
engine.setProperty('rate', 175)

hazard_classes = {
    "wet_floor_sign": "wet floor sign",
    "Safety-cone": "safety cone",
    "Safety-bollard": "safety bollard",
    "barrier tape": "barrier tape"
}

last_spoken_time = 0
SPEECH_COOLDOWN = 5
fail_count = 0
MAX_FAILS = 10

while True:
    frame = grab_latest_frame(cap, flush_count=4)
    if frame is None:
        fail_count += 1
        cap.release()
        time.sleep(1)
        cap = open_stream()
        if cap is None or fail_count > MAX_FAILS:
            break
        continue
    fail_count = 0
    results = model.infer(frame)[0]
    detections = sv.Detections.from_inference(results)
    labels = [class_names[cid] for cid in detections.class_id]
    current_time = time.time()
    detected_hazards = [hazard_classes[label] for label in labels if label in hazard_classes]
    detected_hazards = list(dict.fromkeys(detected_hazards))
    if detected_hazards and (current_time - last_spoken_time >= SPEECH_COOLDOWN):
        if len(detected_hazards) == 1:
            message = f"Warning: {detected_hazards[0]} detected ahead."
        else:
            combined = ", ".join(detected_hazards[:-1]) + " and " + detected_hazards[-1]
            message = f"Warning: {combined} detected ahead."
        engine.say(message)
        engine.runAndWait()
        last_spoken_time = current_time
    annotated = box_annotator.annotate(scene=frame, detections=detections)
    annotated = label_annotator.annotate(scene=annotated, detections=detections)
    cv2.imshow("Live Detection", annotated)
    if cv2.waitKey(1) & 0xFF == 27:
        break
cap.release()
cv2.destroyAllWindows()
```
</details>

---

## Part 04: Putting It All Together

### Objective  
Create a low-cost, vision-based alert system using ESP32 and YOLO.

### Final Integration Steps  
- Power up ESP32-CAM  
- Run Python detection script on host  
- Observe detection overlay + spoken alerts  

### Possible Use Cases  
- Indoor navigation aid for visually impaired  
- Smart home obstacle detection  
- Security monitoring  

---

## Additional Resources  

- [ESP32-CAM Documentation](https://randomnerdtutorials.com/projects-esp32-cam/)  
- [Roboflow Inference Docs](https://docs.roboflow.com/inference)  
- [OpenCV Tutorials](https://docs.opencv.org/4.x/d6/d00/tutorial_py_root.html)  
- [Supervision Library](https://github.com/roboflow/supervision)  

---

*Created by Purab Balani*
