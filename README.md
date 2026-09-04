# ESP32-CAM Edge Vision Pipeline

A real-time object-detection prototype that combines an ESP32-CAM video source with host-side YOLO/cvlib inference.

The ESP32 handles image capture and Wi-Fi delivery; a Python client performs detection over the incoming frames. This separation keeps the embedded component lightweight while allowing a larger pretrained model to run on a laptop or edge host.

## Architecture

```mermaid
flowchart LR
    A[ESP32-CAM] -->|JPEG over Wi-Fi| B[Python frame client]
    B --> C[YOLO / cvlib inference]
    C --> D[Live labelled output]
```

## Components

- **`esp32-cam.ino`** — Connects an AI-Thinker ESP32-CAM to Wi-Fi and exposes JPEG snapshots at three resolutions:
  - `/cam-lo.jpg` — 320×240
  - `/cam-mid.jpg` — 350×530
  - `/cam-hi.jpg` — 800×600
- **`model-code.py`** — Pulls frames from the camera endpoint, runs `detect_common_objects` using a pretrained YOLO model over the COCO classes, and renders labelled bounding boxes.

## Hardware

- ESP32-CAM module using the AI-Thinker pin configuration
- FTDI or USB-to-serial adapter for flashing
- A host machine for Python inference

## Run the prototype

1. Replace the Wi-Fi placeholders in `esp32-cam.ino`.
2. Flash the firmware and read the assigned IP address over serial at 115200 baud.
3. Set that address in the Python client's `url` variable.
4. Install the dependencies and run the detector:

```bash
pip install opencv-python cvlib numpy matplotlib
python model-code.py
```

Press `q` to close the live windows. cvlib downloads the pretrained YOLO weights on first use.

## Engineering focus

- Embedded camera streaming over HTTP
- Real-time frame acquisition and inference
- Hardware–software integration
- Resolution and latency trade-offs in a constrained vision pipeline

## Security and limitations

Never commit real Wi-Fi credentials; the repository intentionally uses placeholders. This prototype uses unencrypted local HTTP and host-side inference, so it is intended for controlled-network experimentation rather than production deployment.

![ESP32-CAM object detection demo](https://github.com/abdelrhmanidk/objectdetection-esp32/assets/145793607/8ff00897-1e96-488f-857d-1c13d62e9433)
