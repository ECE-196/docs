---  
title: A Basic Guide to Performing Computer Vision Tasks with ESP32-CAM Module  
date: 05-19-2025  
authors:  
  - name: Purab Balani  
tags: [ESP32-CAM, Computer Vision, IoT, YOLO, Object Detection, Python, OpenCV]  
---  

![image](https://github.com/user-attachments/assets/3ea1b21b-82df-472a-acaa-73c45b6e4162)


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

## Part 03: Choosing and Training Your Model with Roboflow

### Objective
Learn how to choose the best model type and train your own dataset using Roboflow.

### Choosing a Model Type
Roboflow supports many model architectures. For use with an ESP32-CAM that streams to a host machine (which does the heavy computation), you’ll want to select a lightweight model optimized for speed and acceptable accuracy. Good options include:

- **YOLOv5n** – “Nano” version of YOLOv5, very fast but less accurate.
- **YOLOv8n** – Latest nano version of YOLOv8, offering better trade-offs.
- **YOLOv8s** – Slightly larger, better accuracy, still usable on laptops.
- **MobileNet-SSD** – Great for low-latency mobile applications.

> ✅ **Tip:** Start with YOLOv8n and scale up if needed.

---

### Training a Model in Roboflow

1. Go to [https://roboflow.com](https://roboflow.com) and create a free account.
2. Click **"Create Project"** and set your object detection parameters.
3. Upload your images and annotate them using Roboflow’s labeling interface.
4. After labeling, click **"Generate Dataset"** to resize and augment your images.
5. Click **"Train Model"** and choose a suitable model type (e.g. YOLOv8n).
6. When training is done, you'll receive a `model_id` for use with the Inference SDK.

---

### Using Your Trained Model

To use your trained model with the Roboflow Inference SDK:

```python
from inference import get_model

model = get_model(model_id="your_model_id")
```

If you're deploying with a `.pt` file locally (instead of the Roboflow-hosted model):

- Download the YOLO weights (`weights.pt`) from Roboflow.
- Use [Ultralytics YOLOv8](https://github.com/ultralytics/ultralytics) locally:

```bash
pip install ultralytics
```

Then you can run:

```python
from ultralytics import YOLO

model = YOLO("your_model.pt")
results = model.predict("image.jpg")
```

---

### Summary

| Scenario | Recommended Model |
|----------|--------------------|
| Streaming from ESP32-CAM to PC | YOLOv8n or YOLOv5n |
| Deployment on Jetson Nano or similar | YOLOv8s |
| Mobile deployment (e.g., Android app) | MobileNet-SSD |
| Need best accuracy | YOLOv8m or YOLOv5m |

---

```markdown
💡 Consider uploading diverse real-world images for best results during training.
```
---

## Part 04: Performing Object Detection

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

# Replace with your ESP32-CAM stream URL
STREAM_URL = "http://<IP>:81/stream" #change this

# Open video stream
cap = cv2.VideoCapture(STREAM_URL)
if not cap.isOpened():
    raise RuntimeError("Could not open ESP32 stream.")

# Load Roboflow model
model = get_model(model_id="YOUR MODEL ID") #change this
box_annotator = sv.BoxAnnotator()
label_annotator = sv.LabelAnnotator()
class_names = model.class_names

# Main loop
while True:
    ret, frame = cap.read()
    if not ret:
        print("Failed to read frame.")
        break

    # Run inference
    results = model.infer(frame)[0]
    detections = sv.Detections.from_inference(results)

    # Annotate frame
    annotated = box_annotator.annotate(scene=frame, detections=detections)
    annotated = label_annotator.annotate(scene=annotated, detections=detections)

    # Show result
    cv2.imshow("YOLO Detection", annotated)

    # Press ESC to quit
    if cv2.waitKey(1) & 0xFF == 27:
        break

cap.release()
cv2.destroyAllWindows()
```
</details>

---

## Part 05: Putting It All Together

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
