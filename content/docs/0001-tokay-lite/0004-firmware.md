+++
description = "Know Your Tokay Camera Better"
title = "Building and Flashing Firmware"
weight = 3
url = "docs/edge-ai-tokay-lite/firmware"
+++

## Cloning Firmware Repository

```bash
git clone https://github.com/maxlab-io/tokay-lite-sdk.git
```

## Setup ESP-IDF

The firmware is based on ESP-IDF v5.0, you can either install it locally, or use a pre-built docker image
by Espressif as your build environment (preferred, see below).

## Building Firmware

To build the firmware with docker image, run this from the root directory of the repository:

```bash
docker run -it --rm -v $PWD:/project -w /project espressif/idf:v5.0 idf.py build -C webserver
```

## Flashing Firmware

The firmware is flashed via a USB using the built-in USB-CDC interface of ESP32-S3.
To flash the device, just plug the USB cable, and hold the PWR button to keep the device powered on.
Notice the /dev/ttyACMx device showing up in the system, and plug it into the command below instead of `<USB-device-path>`.

```bash
docker run -it --device=<USB-device-path>:/dev/ttyACM0 --rm -v $PWD:/project -w /project espressif/idf:v5.0 idf.py flash -C webserver
```

## Next Steps

Once flashed, press and hold the PWR button for 3 seconds and notice the `tokay-lite` WiFi access point showing up.
Connect to that AP and point your browser to http://tokay-lite.local page to supply your WiFi network credentials.
After pressing the `Submit` button, the camera will connect to your local network and you can access the web dashboard
through the same http://tokay-lite.local link.
NOTE: this require MDNS support on your PC (this is usually enabled by default on most mainstream systems).
