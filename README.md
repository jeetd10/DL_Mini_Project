
# SiteGuard AI — Construction Site Safety & PPE Detection Using YOLOv8

![SiteGuard AI](assets/videoconstruc2.gif)

**Computer Vision · Real-Time Camera Monitoring · PPE Detection · Safety Intelligence**

SiteGuard AI is a computer vision application designed to assist construction-site safety monitoring. Powered by a custom-trained YOLOv8 model, it detects personal protective equipment (PPE), identifies potential missing-PPE indicators, and provides visual results through an interactive Streamlit dashboard.

The application supports **image analysis, video analysis, and live camera monitoring**, with inference performed locally on the computer running the application.

> AI-generated detections are intended to support safety reviews. They are not a substitute for professional inspection or a certified workplace safety assessment.

---

## Introduction

Construction sites require consistent monitoring to help ensure that workers use appropriate protective equipment. Manually inspecting large amounts of site footage can be time-consuming.

SiteGuard AI uses object detection to analyze construction-site images and videos. It can identify objects such as hardhats, safety vests, people, vehicles, and machinery, along with classes that indicate potentially missing PPE.

The project uses the Construction Site Safety dataset available through Roboflow Universe:

- [Construction Site Safety — Roboflow Universe](https://universe.roboflow.com/roboflow-universe-projects/construction-site-safety)
- [Construction Site Safety Dataset — Kaggle](https://www.kaggle.com/datasets/snehilsanyal/construction-site-safety-image-dataset-roboflow)

The original dataset contains 2,801 annotated images in YOLO format, divided into 2,605 training images, 114 validation images, and 82 test images.

### Detection classes

The custom model is designed to detect the following 10 classes:

1. Hardhat
2. Mask
3. NO-Hardhat
4. NO-Mask
5. NO-Safety Vest
6. Person
7. Safety Cone
8. Safety Vest
9. machinery
10. vehicle

![PPE Dataset](assets/ppe.webp)

---

## Key Features

### 1. Interactive Streamlit Dashboard

SiteGuard AI provides a dashboard with four main sections:

- **Platform:** Project overview and navigation to the analysis tools.
- **Image Analysis:** Upload and inspect construction-site images.
- **Video Analysis:** Process uploaded videos frame by frame.
- **Live Cameras:** Connect compatible phone-camera streams or IP cameras.

### 2. Image Analysis

Upload a JPG, JPEG, or PNG image to run object detection using the custom YOLOv8 model.

The image analysis page provides:

- Original and annotated image comparison.
- Bounding boxes and predicted class labels.
- Object detection and class counts.
- Inference time.
- Hardhat, safety vest, and missing-PPE indicators.
- Class-wise detection charts.
- Downloadable annotated images.

### 3. Video Analysis

Upload a supported construction-site video in MP4, MOV, or AVI format.

The application processes the video frame by frame and generates an annotated output.

Features include:

- Frame-by-frame YOLO inference.
- Annotated video output.
- Processing progress feedback.
- Detection counts by class across processed frames.
- Potential missing-PPE indicators.
- Downloadable annotated video.

**Important:** Video detection counts represent detections across frames, not unique workers. A worker may be counted multiple times.

### 4. Live Camera Monitoring

Connect a compatible camera stream to monitor a construction site through the dashboard.

Supported stream types depend on the camera application and OpenCV backend, and may include:

- HTTP/MJPEG streams.
- RTSP streams.
- Compatible phone-camera applications.
- Compatible IP cameras.

The phone and computer must be able to communicate over the network. The application uses the provided stream URL to receive video frames and run YOLO inference locally.

The live monitoring page displays:

- Annotated camera frames.
- Current-frame object and class counts.
- Potential missing-PPE indicators.
- A configurable confidence threshold.
- Camera connection and disconnection controls.

**Network note:** A local Wi-Fi stream generally works when the phone and computer can reach each other. Remote access requires an appropriately configured and secured network connection. A camera stream URL is not guaranteed to work outside its local network.

### 5. PPE Indicators and Confidence Settings

The confidence threshold can be adjusted through the dashboard.

- A higher threshold filters out lower-confidence predictions.
- A lower threshold may reveal additional detections but can also increase false positives.

Classes such as `NO-Hardhat` and `NO-Safety Vest` are treated as potential missing-PPE indicators. Model predictions should be checked by a human before taking action.

### 6. Local Inference

The application loads its custom-trained weights from:

`models/best.pt`

Image, video, and live-camera inference runs on the machine hosting the Streamlit application. Performance depends on the computer, video resolution, model configuration, and number of frames processed.

---

## Technology Stack

| Technology | Purpose |
|---|---|
| Python | Application logic |
| Streamlit | Interactive dashboard and user interface |
| Ultralytics YOLOv8 | Object detection |
| OpenCV | Image processing, video processing, and camera streams |
| Pillow (PIL) | Image loading and conversion |
| NumPy | Image array conversion |
| Collections.Counter | Detection counting |
| tempfile and os | Temporary video files and file management |
| Streamlit Autorefresh | Periodic refresh for live monitoring |

---

## Project Setup

### Prerequisites

Install the following before running the application:

- Python compatible with your installed dependencies.
- pip.
- Git (optional, for cloning the repository).
- The custom-trained model file, `models/best.pt`.
- A compatible camera-stream application if you want to use live monitoring.

### 1. Clone or open the repository

If you are cloning the project:

```bash
git clone <YOUR_GITHUB_REPOSITORY_URL>
cd Construction-Site-Safety-PPE-Detection
```

Alternatively, open your existing project folder in VS Code.

### 2. Create and activate a virtual environment

On macOS or Linux:

```bash
python3 -m venv venv
source venv/bin/activate
```

On Windows:

```bash
python -m venv venv
venv\Scripts\activate
```

### 3. Install dependencies

```bash
python -m pip install --upgrade pip
python -m pip install streamlit ultralytics opencv-python pillow numpy streamlit-autorefresh
```

If your project already contains a tested `requirements.txt`, you can instead install the listed dependencies using:

```bash
python -m pip install -r requirements.txt
```

### 4. Add the trained model

Ensure the custom-trained model exists at:

```text
models/best.pt
```

The application expects this file relative to `app.py`. If it is missing, the application will raise a file-not-found error.

### 5. Verify the application

Run a Python syntax check:

```bash
python -m py_compile app.py
```

If the command completes without output, Python has not reported a syntax error.

### 6. Launch the dashboard

```bash
streamlit run app.py --server.port 8503
```

Open the local address displayed by Streamlit. The default address for this command is:

```text
http://localhost:8503
```

If port 8503 is already occupied, try another port:

```bash
streamlit run app.py --server.port 8504
```

To stop the application, press `Ctrl+C` in the terminal running Streamlit.

---

## Using SiteGuard AI

### Image inspection

1. Open the **Image Analysis** page.
2. Upload a construction-site image.
3. Adjust the confidence threshold if necessary.
4. Select **ANALYZE IMAGE**.
5. Review the original and annotated images.
6. Inspect the class counts and potential missing-PPE indicators.
7. Download the annotated image if required.

### Video inspection

1. Open the **Video Analysis** page.
2. Upload an MP4, MOV, or AVI video.
3. Select **Analyze Video**.
4. Wait for frame processing to finish.
5. Review the annotated video and class-wise counts.
6. Download the annotated video.

Processing time depends on the video's duration, frame rate, resolution, and available hardware.

### Live camera monitoring

1. Open a compatible camera-stream application on your phone or start your IP camera.
2. Confirm that the stream is running and accessible from your computer.
3. Copy the stream URL provided by the camera application.
4. Open **Live Cameras** in SiteGuard AI.
5. Enter the stream URL and select **CONNECT CAMERA**.
6. Review the live annotated feed and current-frame detections.
7. Select **STOP CAMERA** when finished.

Example URL format:

```text
http://192.168.1.25:8080/video
```

This is only an example. Replace it with the actual URL provided by your camera application.

---

## Model and Dataset Background

The original project used the Ultralytics YOLOv8 framework and a custom construction-site PPE dataset.

The original training workflow used Kaggle with a P100 GPU. The custom model was trained for 100 epochs, with a reported training duration of approximately 2.719 hours.

The current Streamlit application loads the trained weights from `models/best.pt` for inference. Training the model again is not required to run the dashboard if the trained weights and application dependencies are already available.

For more information about the original dataset exploration, see:

[Check yo'self before you wreck yo'self — CSS EDA on Kaggle](https://www.kaggle.com/code/snehilsanyal/check-yo-self-before-you-wreck-yo-self-css-eda)

---

## Project Structure

The application uses `app.py` as its main entry point and `models/best.pt` as its custom model.

```text
Construction-Site-Safety-PPE-Detection/
│
├── app.py
├── README.md
├── requirements.txt
│
├── models/
│   └── best.pt
│
├── assets/
│   ├── videoconstruc2.gif
│   └── ppe.webp
│
├── data/
│   ├── data.yaml
│   ├── ppe_data.yaml
│   ├── train/
│   │   ├── images/
│   │   └── labels/
│   ├── valid/
│   │   ├── images/
│   │   └── labels/
│   └── test/
│       ├── images/
│       └── labels/
│
├── results/
│   ├── confusion_matrix.png
│   ├── results.png
│   ├── train_batch14672.jpg
│   └── val_batch2_pred.jpg
│
├── output/
│   └── output_yolov8n_100e/
│
└── source_files/
```

This tree describes the expected or original project layout. Some folders and example files are optional for running the dashboard. Keep only the entries that actually exist in your repository.

---

## Model Results

The original training workflow generated visualizations for evaluating the custom-trained YOLOv8n model.

Examples of the training artifacts include:

### Confusion matrix

![Confusion Matrix](results/confusion_matrix.png)

### Training batch

![Training Batch](results/train_batch14672.jpg)

### Validation predictions

![Validation Predictions](results/val_batch2_pred.jpg)

### Training metrics

![Training Results](results/results.png)

These images are available only if the corresponding result files are present in the repository. They describe the original training workflow and should not be interpreted as a guarantee of real-world performance.

---

## Example Outputs

The following examples come from the original project's saved prediction outputs.

![Construction Safety Detection](output/output_yolov8n_100e/construction-safety.jpg)

![PPE Detection Example](output/output_yolov8n_100e/portrait-of-woman-with-mask-and-man-with-safety-glasses-on-a-construction-HX01FH.jpg)

![Construction Workers Detection](output/output_yolov8n_100e/two-young-construction-workers-wearing-555864.jpg)

---

## Limitations

- Detection quality depends on the training dataset, lighting, camera angle, occlusion, image quality, and confidence threshold.
- A detected `NO-Hardhat` or `NO-Safety Vest` class indicates a model prediction, not a verified safety violation.
- A missing detection does not prove that a worker is wearing all required PPE.
- Video detection totals count detections across frames rather than distinct workers.
- Live-stream compatibility depends on the camera application, URL, network, and OpenCV backend.
- Remote camera access requires appropriate network configuration and security.
- The application does not, by itself, provide persistent worker identity tracking, an automated alarm system, or a certified safety-compliance decision.

---

## Future Work

1. Evaluate and improve the model on more varied construction-site footage.
2. Compare YOLOv8 model variants and inference configurations.
3. Add persistent worker tracking and unique-worker counting.
4. Store timestamped detections and missing-PPE events.
5. Add configurable alerts for potential PPE violations.
6. Develop safety reports with event history and exportable summaries.
7. Improve multi-camera management and secure remote camera access.
8. Optimize live inference for lower latency and higher frame rates.
9. Evaluate precision, recall, mAP, and false-positive rates on a held-out test set.

---

## References

- [Ultralytics Documentation](https://docs.ultralytics.com/)
- [Roboflow Construction Site Safety Dataset](https://universe.roboflow.com/roboflow-universe-projects/construction-site-safety)
- [Kaggle Construction Site Safety Dataset](https://www.kaggle.com/datasets/snehilsanyal/construction-site-safety-image-dataset-roboflow)
- [Original CSS EDA Notebook](https://www.kaggle.com/code/snehilsanyal/check-yo-self-before-you-wreck-yo-self-css-eda)

---

## Disclaimer

SiteGuard AI is a computer vision prototype intended to assist construction-site safety monitoring. It does not guarantee detection accuracy or regulatory compliance. All predictions and potential safety incidents should be reviewed by a qualified person before operational decisions are made.