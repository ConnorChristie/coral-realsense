# Google Coral with Intel RealSense

## Flashing the Coral Dev Board
### Linux and Mac
* Follow https://coral.withgoogle.com/docs/dev-board/get-started/

### Windows
#### Prerequisites
* Install the CP210x USB to UART drivers: https://www.silabs.com/products/development-tools/software/usb-to-uart-bridge-vcp-drivers

* Use the Android Platform Tools distribution for fastboot - https://developer.android.com/studio/releases/platform-tools.html#download and set your PATH to point at the location of this (unzipped) folder - e.g. in cmd via `setx path "%path%;%userprofile%/Downloads/platform-tools`

* A serial console utility: [PuTTY](https://www.putty.org/) is my go-to on Windows.

* Ensure you have the right cables: a USB-C power cable, a micro-USB cable (for the serial console), and a USB-C data cable.

#### Connecting to the Serial Console
1. **Connect to the dev board's micro-USB port**, and identify the COM port the device is attached to in the Device Manager by looking under "Ports (COM & LPT)" for the "CP2105 USB to UART Bridge" device. In my case, it was COM8.

2. **Power on the board** by connecting the USB-C power cable to the power port (furthest from the HDMI port).

3. **Open PuTTY**, select "Serial" as the connection option, set the COM port to the one you identified above, and the data rate to 115200bps.

You should now be at the dev board's uboot prompt, and ready to flash the bootloader & disk image. If not, check that the board is powered on, that the COM port is correct, and that the Device Manager lists the device.

#### Flashing the Board
1. In the u-boot serial console (e.g. in PuTTY), put the dev board into fastboot mode:
    ```
    fastboot 0
    ```

2. In the Device Manager, you'll see a "USB Download Gadget" appear with a warning symbol. Right click, choose "Update Driver", select "Browse my computer for driver software" and then "Let me pick from a list of available drivers from my computer". In the driver browser, choose "WinUsb Device" from the left side, and "ADB Device" (Android Debugger) from the right. Click "Next" and accept the warning. The Device Manager will refresh, and show the device under "Universal Serial Bus devices".

3. To confirm it's configured correctly and visible to the OS, head back to your command prompt and enter:
    ```
    $ fastboot devices
    122041d6ef944da7        fastboot
    ```

4. From here, you’ll need to download and unzip the bootloader image and the disk image (identical to _Part 6_ of the [official instructions](https://coral.withgoogle.com/docs/dev-board/get-started/))

5. Run `flash.sh` inside of a bash console, such as Git bash.

6. You should now have a Coral dev board running the latest version of Mendel Linux and should be able to login with the following credentials:
    ```
    Login is mendel
    Password is mendel
    ```

7. Follow the official documentation on how to [set up network connectivity](https://coral.withgoogle.com/tutorials/devboard/#connect-to-the-internet).

## Patching the Kernel for use with Intel RealSense

The Mendel Linux kernel can _easily_ be patched to work with the Intel RealSense cameras. This patch strategy was tested with a Google Coral Dev Board using kernel `4.9.51-imx` and a [D415 realsense camera](https://store.intelrealsense.com/buy-intel-realsense-depth-camera-d415.html). However, this should work with any realsense camera but unfortunately I have not tested the accelerometer and gyro patches.

The instructions for patching are in [4.9.51-PATCH](./4.9.51-PATCH.md)

## Setup RealSense udev rules
1. Run the following command to setup the rules:
    ```
    sudo ./scripts/setup_udev_rules.sh
    ```

## Installing Intel® RealSense™ SDK 2.0

TODO: I will soon be adding instructions on how to use Docker to compile the realsense SDK and copy it over to the board itself.