# objectdetection-esp32

Real-time object detection over an ESP32-CAM stream. The ESP32-CAM serves JPEG
frames over WiFi; a Python client pulls them and runs object detection, drawing
labelled bounding boxes on a live window.

## How it works

- **`esp32-cam.ino`** — flashed to an ESP32-CAM (AI-Thinker board). Connects to
  WiFi and runs a small web server exposing JPEG snapshots at three resolutions:
  `/cam-lo.jpg` (320×240), `/cam-mid.jpg` (350×530), `/cam-hi.jpg` (800×600).
  Built on the [esp32cam](https://github.com/yoursunny/esp32cam) library.
- **`model-code.py`** — fetches frames from the camera's HTTP endpoint and runs
  [cvlib](https://github.com/arunponnusamy/cvlib)'s `detect_common_objects`
  (a pretrained YOLO model over the 80 COCO classes), drawing boxes with
  `draw_bbox`. Two windows run in parallel: the raw live transmission and the
  detection overlay.

## Hardware

- ESP32-CAM module (AI-Thinker pin config) + an FTDI / USB-serial adapter to flash it.

## Setup

1. In `esp32-cam.ino`, set `WIFI_SSID` / `WIFI_PASS`, flash the board, and note
   the IP it prints over serial (115200 baud).
2. Put that IP into the `url` variable in `model-code.py`.
3. Run the detector (cvlib downloads the YOLO weights on first run):

       pip install opencv-python cvlib numpy matplotlib
       python model-code.py

Press **q** to close a window.

> **Note:** never hardcode real WiFi credentials in a public repo — the
> placeholders above are intentional.

![ESP32-CAM object detection demo](https://github.com/abdelrhmanidk/objectdetection-esp32/assets/145793607/8ff00897-1e96-488f-857d-1c13d62e9433)
