+++
description = "Building and Flashing Firmware"
title = "Building and Flashing Firmware"
weight = 3
url = "docs/edge-ai-tokay-lite/firmware"
+++

This page describes how to setup a process to build and flash custom
firmware for Tokay Lite cameras.

## Necessary Preparations {#preparations}

First and foremost, you need to clone the Tokay Lite firmare repository.
The repositry can be found on GitHub: https://github.com/maxlab-io/tokay-lite-sdk
and be cloned as follows:

```bash
git clone https://github.com/maxlab-io/tokay-lite-sdk.git
```

The firmware requires a couple of submodules to be updated, so you
need to proceed with cloning them as well:

```bash
cd tokay-lite-sdk
git submodule update --init --recursive
```

The firmware is based on [ESP-IDF framework](https://www.espressif.com/en/products/sdks/esp-idf),
so in order to proceed with builds, the ESP-IDF framework must be either
locally installed or used directly from Docker images.

It is recommended to use Docker on Linux, since it reduces the dependency mess.
However, if you still want to install ESP-IDF locally or you have troubles
installing Docker on MacOS, skip the Docker installation section and jump right
to [building firmware locally guide](#esp-idf-native)

## Build and Flash Using Local ESP-IDF Installation {#esp-idf-native}

### Setup ESP-IDF

{{< tip info >}}

The Tokay Lite firmware is compatible only with v5.0 of ESP-IDF.
Make sure you install correct version!

{{< /tip >}}

In order to isntall ESP-IDF, please follow [the official Espressif guide](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/#installation).

You also need to be familiar about building and developing projects using ESP-IDF
tools, so it is stronly advised to complete [the Build Your First Project Guide](https://docs.espressif.com/projects/esp-idf/en/latest/esp32/get-started/index.html#build-your-first-project)

After ESP-IDF installation is complete, return back and proceed with
following steps.

### Building And Flashing Firmware

1. Make sure the repository is cloned and submodules are updated.
   See [Necessary Preparation section](#preparations) for details.

   1. Build the firmware directly using `idf.py`:

    ```bash
    idf.py build -C webserver
    ```

### Flashing Firmware

1. Before flashing Tokay Lite, assure that USB-C is connected.

1. Hold the PWR button to wakeup the device. The firmware will open the USB port
   using the built-in USB-CDC interface of ESP32-S3.

1. If you're on Linux, check that `/dev/ttyACM*` USB port is showing up in the
   system:

    ```bash

    ls -l /dev/ttyACM*

    crw-rw----+ 1 root root 166, 0 May  4 20:57 /dev/ttyACM0

    ```
1. Run the following command, replacing `<TTY-USB-PATH>` with the tty path
   discovered during previous step.

    ```bash
    idf.py -p <TTY-USB-PATH> flash -C webserver

    ```

   The flashing should succeed now.

## Build and Flash Via Docker

   {{< tip "warning" >}}

   Docker installation may not work on all MacOS systems. If you're using Macbook
   as a development platform, it's recommended to install ESP-IDF and
   [build Tokay Lite firmware natively](#esp-idf-native).

   {{< /tip >}}

### Installing Docker

   Before building the firmware, you need to install Docker on your host.
   Please refer to official Docker docs to figure out how to do it:

   * [MacOS guide](https://docs.docker.com/desktop/install/mac-install/)
   * [Linux guide](https://docs.docker.com/desktop/install/linux-install/)
   * [Windows guide](https://docs.docker.com/desktop/install/windows-install/)

   ### Building Firmware

   1. Make sure the repository is cloned and submodules are updated.
      See [Necessary Preparation section](#preparations) for details.

   1. Proceed with building the firmware using Docker image:

       ```bash
       docker run -it --rm -v $PWD:/project -w /project espressif/idf:v5.0 idf.py build -C webserver
       ```

### Flashing Firmware

   1. Before flashing Tokay Lite, assure that USB-C is connected.

   1. Hold the PWR button to wakeup the device. The firmware will open the USB port
      using the built-in USB-CDC interface of ESP32-S3.

   1. If you're on Linux, check that `/dev/ttyACM*` USB port is showing up in the
      system:

       ```bash

       ls -l /dev/ttyACM*

       crw-rw----+ 1 root root 166, 0 May  4 20:57 /dev/ttyACM0

       ```

   1. **Hold the PWR button again. Do not release the button until the flashing process
      is complete**

   1. Run the following command, replacing `<TTY-USB-PATH>` with the tty path
      discovered during previous step.

       ```bash
       docker run -it --device=<TTY-USB-PATH>:/dev/ttyACM0 --rm -v $PWD:/project -w /project espressif/idf:v5.0 idf.py flash -C webserver
       ```

      The flashing should succeed now, you can release the PWR button

## Updating Firmware from Binaries

## Next Steps

1. Once flashed, press and hold the PWR button for 3 seconds and notice
   the `tokay-lite` WiFi access point showing up.

1. Connect to the `tokay-lite` WiFi access point and point your browser to
   `http://tokay-lite.local` page to .

{{< tip info >}}

The hostname `tokay-lite.local` is resolvable only if mDNS discovery
is enabled on your device. Usually that's the case on mainstream systems,
but if your host system lacks mDNS support, use following address:

`http://192.168.4.1`

{{< /tip >}}

3. Supply your WiFi network credentials and press the `Submit` button.
   The camera will connect to your local network.

   Assuming your host and the camera are on the same network, you can access the Tokay web dashboard through the same `http://tokay-lite.local` link.

## Troubleshooting

{{< tip warning >}}

This section is in progress

{{< /tip >}}
