Here’s a clean, professional README description you can drop into your GitHub repo.

---

# 🪖 Real-Time Helmet Detection & Face Recognition System

A real-time multi-camera helmet compliance monitoring system built using **YOLO**, **DeepFace**, and **OpenCV**.

This system detects whether a person is wearing a helmet, identifies the individual using facial recognition, and logs violations with snapshots and timestamps into a SQLite database.

Designed for industrial plants, construction sites, chemical factories, and high-risk safety zones.

---

## 🚀 Features

### ✅ Helmet Detection (YOLO)

* Custom-trained YOLO model (`final.pt`)
* Detects:

  * `helmet`
  * `head`
* Uses IoU logic to avoid duplicate or conflicting bounding boxes

### 👤 Face Recognition (DeepFace)

* Matches detected faces against a local face database
* Uses:

  * `VGG-Face` model
  * Cosine distance metric
  * RetinaFace backend
* Threshold-based identity verification
* Automatically labels unknown individuals

### 🌙 Low-Light Enhancement

Improves detection in poor lighting conditions using:

* CLAHE (Contrast Limited Adaptive Histogram Equalization)
* Gamma correction

This acts as a real-time preprocessing pipeline for better YOLO performance.

### 📸 Automatic Violation Logging

When a person is detected without a helmet:

* Face snapshot is captured
* Identity is resolved (or marked `unknown`)
* Image saved inside `/violations`
* Entry stored in SQLite database with:

  * Name
  * Timestamp
  * Camera ID
  * Image path

### 🧠 Smart Cooldown System

Prevents duplicate logging of the same person:

* Frame-based cooldown tracker
* Avoids repeated entries for the same face within N frames

### 📡 Multi-Camera Support

* Uses Python multiprocessing
* Supports:

  * Local webcam (`0`)
  * RTSP streams
  * Multiple camera IDs

---

## 🏗 Tech Stack

* **OpenCV** – Video processing
* **Ultralytics YOLO** – Helmet detection
* **DeepFace** – Face recognition
* **SQLite3** – Violation logging
* **Multiprocessing** – Multi-camera support
* **NumPy** – Image transformation

---

## 📂 Project Structure

```
Helmet_Project/
│
├── final.pt                # Trained YOLO model
├── faces/                  # Face database
├── violations/             # Saved violation snapshots
├── violations.db           # SQLite database
└── main.py                 # Main application script
```

---

## 🗄 Database Schema

Table: `violations`

| Field      | Type    | Description              |
| ---------- | ------- | ------------------------ |
| id         | INTEGER | Primary Key              |
| name       | TEXT    | Person name or "unknown" |
| timestamp  | TEXT    | Date & time of violation |
| image_path | TEXT    | Saved snapshot path      |
| cam_id     | TEXT    | Camera identifier        |

---

## 🧪 How It Works

1. Capture frame from camera
2. Enhance low-light image
3. Run YOLO detection
4. Separate:

   * Helmet boxes
   * Head boxes
5. Compute IoU between head & helmet boxes
6. If no helmet:

   * Extract face ROI
   * Match against face database
   * Save violation snapshot
   * Log into database

---

## ⚙️ Configuration Parameters

```python
threshold_iou = 0.3
deepface_threshold = 0.7
cooldown_frames = 100
```

These control:

* Helmet overlap validation
* Face matching strictness
* Duplicate logging prevention

---

## 🎯 Ideal Use Cases

* Industrial safety monitoring
* Construction site compliance
* Chemical plant PPE enforcement
* Campus or restricted area monitoring

---

## 🔮 Future Improvements

* Train YOLO with dedicated low-light dataset instead of preprocessing hack
* Replace frame-based cooldown with tracking ID system (DeepSORT)
* Web dashboard for live violation monitoring
* Email/SMS alert integration
* Cloud database support
* Edge deployment optimization (Jetson Nano / ESP32-CAM integration)

---

## ⚠️ Limitations

* Face ID based on bounding box position (not tracking-based)
* Performance depends on lighting quality
* DeepFace can be slow on CPU
* Requires properly structured face database

---

## 💡 Why This Project Matters

Most helmet detection systems stop at detection.

This system:

* Detects
* Identifies
* Logs
* Stores evidence
* Supports multiple cameras
* Handles low light

It’s built closer to a real-world deployable safety compliance solution rather than just a demo model.

---


