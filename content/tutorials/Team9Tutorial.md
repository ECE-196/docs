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
// Code omitted for brevity; use full firmware from project files
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
