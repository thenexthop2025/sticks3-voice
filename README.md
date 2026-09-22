# StickS3 Voice

[English](README.md) | [简体中文](README_zh-CN.md)

ESPHome voice-assistant firmware made specifically for the **M5Stack StickS3 K150 / StickS3**. This is not a generic ESP32-S3 firmware image and should only be installed on the matching StickS3 hardware.

![M5Stack StickS3 K150 hardware overview](docs/sticks3-hardware.png)

## Features

- Home Assistant voice satellite
- On-device `hey_jarvis` wake word
- Microphone and speaker support through the StickS3 ES8311 audio codec
- 135 × 240 color LCD with an animated candy-colored robot
- Listening, thinking, and speaking status screens
- Speech transcript and assistant response text on the display
- Battery status icon and Wi-Fi signal display
- Battery percentage, battery voltage, and Wi-Fi signal sensors in Home Assistant
- Voice activation using the front button
- Automatic return to the robot screen after a completed response
- Wi-Fi provisioning through Improv Serial or the fallback captive portal
- OTA updates after the device joins the network
- Unique device names using a MAC-address suffix

## Supported hardware

This project targets the **M5Stack StickS3 K150 / StickS3**, including:

- ESP32-S3-PICO-1-N8R8
- 8 MB flash
- 8 MB PSRAM
- 1.14-inch 135 × 240 ST7789P3 color LCD
- ES8311 audio codec
- Built-in microphone
- Built-in speaker
- Front button
- 250 mAh battery

The StickS3 also includes infrared hardware, an IMU, and an expansion port, but this firmware does not currently enable those features. Bluetooth Proxy is intentionally disabled to reduce its possible impact on Wi-Fi and voice-assistant performance.

## Privacy and public sharing

This repository does not contain:

- Personal Wi-Fi credentials
- A fixed Home Assistant API encryption key
- Home Assistant access tokens
- Other private credentials belonging to the project author

Each user configures their own Wi-Fi after installation and pairs the device with their own Home Assistant instance.

If you modify or fork this project, do not commit the following information to a public repository:

- `secrets.yaml`
- Wi-Fi passwords
- Home Assistant access tokens
- API keys
- Other personal or home-network information

## Install from the web

Regular users do not need to install ESPHome, download the source code, or create a GitHub repository.

1. Open the [StickS3 Voice web installer](https://thenexthop2025.github.io/sticks3-voice/) using desktop Google Chrome or Microsoft Edge.
2. Connect the StickS3 to the computer using a USB data cable.
3. Select **Connect**.
4. Choose the USB serial port belonging to the StickS3.
5. Follow the instructions shown by the installer.
6. Keep the device connected until flashing and verification are complete.
7. Restart the device and configure Wi-Fi after installation.

> Web installation replaces the firmware currently installed on the device. Do not select “Erase device” unless you intentionally want to remove all existing device data.

## Download the compiled firmware

Users who prefer another flashing tool can download the compiled firmware directly:

[Download firmware.factory.bin](https://thenexthop2025.github.io/sticks3-voice/firmware.factory.bin)

This is the factory image intended for a complete first-time installation.

## Browser requirements

Web installation uses Web Serial. Use a current desktop Chromium-based browser, such as:

- Google Chrome
- Microsoft Edge

The following browsers and devices generally cannot use this installation flow:

- Safari
- Firefox
- Most mobile and tablet browsers

The installer must be served over HTTPS. GitHub Pages provides HTTPS automatically.

If the StickS3 does not appear in the serial-port list:

1. Confirm that the USB cable supports data transfer and is not a charging-only cable.
2. Connect directly to the computer when possible instead of using a hub.
3. Close other applications that may be using the serial port.
4. Reconnect the device and reload the installer.
5. If download mode is required, connect USB and hold the side button for approximately two seconds. Release it when the internal green LED begins flashing.

Do not select the built-in Bluetooth, debug-console, or wlan-debug ports shown by macOS. The StickS3 normally appears as a new USB, Espressif, or `cu.usbmodem` port.

## First-time Wi-Fi setup

After installation, configure Wi-Fi using either of these methods:

- Use an Improv Serial-compatible client while the device is connected over USB.
- Connect to the temporary **StickS3 Voice Setup** Wi-Fi network and enter your Wi-Fi name and password through its captive portal.

The setup access point is used only for provisioning. After the StickS3 successfully joins your Wi-Fi, it uses the configured network for normal operation.

If the captive portal does not open automatically, connect to **StickS3 Voice Setup** and try:

```text
http://192.168.4.1/
```

## Add to Home Assistant

1. Make sure the StickS3 and Home Assistant can reach each other on the same network.
2. In Home Assistant, open **Settings → Devices & services**.
3. Accept the discovered ESPHome device.
4. If it is not discovered automatically, select **Add integration → ESPHome**.
5. Enter the StickS3 hostname or IP address.
6. Complete the ESPHome pairing process.
7. Select the Home Assistant Assist pipeline and language that the device should use.

The device hostname normally begins with `sticks3-voice`. A MAC-address suffix is enabled so that multiple devices do not receive conflicting names.

No Home Assistant API key belonging to the project author is embedded in the firmware.

## Wake and use the assistant

After the device is connected to Home Assistant, start the voice assistant in either of these ways:

- Say the wake phrase: `Hey Jarvis`
- Press the blue front button

The display indicates the current state:

- Listening
- Thinking
- Speaking

After the response is complete, the device automatically returns to the robot screen.

## Build and flash locally

Advanced users can install ESPHome 2026.9.x and run the following command from the project directory:

```bash
esphome run sticks3-voice.yaml
```

For a compile-only build:

```bash
esphome compile sticks3-voice.yaml
```

Local build files are written to the `.esphome` build directory.

The first installation can be performed over USB. After the device joins the network, it can be adopted into a personal ESPHome Dashboard and maintained using OTA updates.

## Automated GitHub Pages deployment

The included GitHub Actions workflow automatically performs the following steps after each push
