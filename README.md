# StickS3 Voice

[English](README.md) | [简体中文](README_zh-CN.md)

ESPHome voice-assistant firmware made specifically for the **M5Stack StickS3 K150 / StickS3**. It is not a generic ESP32-S3 firmware image; use it only with the matching StickS3 hardware.

![M5Stack StickS3 K150 hardware overview](docs/sticks3-hardware.png)

## Features

- Home Assistant voice satellite with the `hey_jarvis` on-device wake word
- Microphone and speaker support through the StickS3 ES8311 audio codec
- 135 × 240 color LCD with an animated candy-colored robot
- Listening, thinking, and speaking status screens
- Speech transcript and assistant reply text on the display
- Battery percentage, battery voltage, and Wi-Fi signal display
- Front-button voice activation and automatic return to the idle screen
- Wi-Fi setup through Improv Serial or the fallback captive portal
- OTA updates after the device has joined the network
- Unique device names through the MAC-address suffix

## Supported hardware

This project targets the **M5Stack StickS3 K150 / StickS3** with an ESP32-S3-PICO-1-N8R8, 8 MB flash, 8 MB PSRAM, 1.14-inch ST7789P3 LCD, ES8311 codec, microphone, speaker, buttons, and 250 mAh battery.

The board also contains infrared hardware, an IMU, and an expansion port, but this firmware does not currently enable those features. Bluetooth Proxy is also intentionally not enabled.

## Privacy and public sharing

This repository contains no personal Wi-Fi credentials and no fixed Home Assistant API encryption key. Each user configures their own network after flashing and pairs the device with their own Home Assistant instance.

Before publishing changes, do not add `secrets.yaml`, Wi-Fi passwords, Home Assistant tokens, API keys, or other personal data to the repository.

## Install from GitHub Pages

After this repository is pushed to GitHub and Pages is enabled with **GitHub Actions** as its source:

1. Open `https://<github-user>.github.io/sticks3-voice/`.
2. Connect the StickS3 to the computer with a USB data cable.
3. Select **Install**, choose the serial device, and confirm the installation.
4. Keep the device connected until flashing and verification finish.

The included workflow compiles `sticks3-voice.yaml`, locates `firmware.factory.bin`, builds the installer site, and deploys it to GitHub Pages on every push to `main`. The same workflow can also be started manually from the GitHub **Actions** tab.

## Browser requirements

Web installation requires Web Serial. Use a current desktop Chromium-based browser such as Google Chrome or Microsoft Edge. Safari, Firefox, and most mobile browsers do not support this installation flow. The installer must be served over HTTPS (GitHub Pages does this automatically). If no serial device appears, check that the cable carries data, close other programs using the port, and reconnect the device.

## First-time Wi-Fi setup

After flashing, configure Wi-Fi in either of these ways:

- Complete the Improv Serial setup offered by the installer/browser while the device is connected over USB.
- Connect to the temporary **StickS3 Voice Setup** Wi-Fi network and use its captive portal to enter your network name and password.

The setup access point is for provisioning only. It stops being the normal connection after the device successfully joins your Wi-Fi.

## Add to Home Assistant

1. Make sure the StickS3 and Home Assistant can reach each other on the same network.
2. In Home Assistant, open **Settings → Devices & services**.
3. Accept the discovered ESPHome device, or select **Add integration → ESPHome** and enter the device hostname or IP address.
4. Finish the ESPHome pairing flow for your Home Assistant installation.
5. Configure the Home Assistant Assist pipeline and language you want the voice assistant to use.

No API key belonging to the project author is embedded in the firmware.

## Build and flash locally

With ESPHome 2026.9.x installed:

```bash
esphome run sticks3-voice.yaml
```

For a compile-only build:

```bash
esphome compile sticks3-voice.yaml
```

On first use, ESPHome can provision the device over USB. You can also adopt the device into your own ESPHome Dashboard and maintain your private settings there.

## Project files

| Path | Purpose |
| --- | --- |
| `sticks3-voice.yaml` | ESPHome firmware configuration |
| `docs/sticks3-hardware.png` | StickS3 hardware reference image used by both READMEs |
| `site/index.html` | Browser-based installer page |
| `site/manifest.json` | ESP Web Tools firmware manifest |
| `.github/workflows/pages.yml` | Automatic firmware build and GitHub Pages deployment |
| `.gitignore` | Excludes local ESPHome builds, secrets, and generated firmware |
| `README.md` | English documentation |
| `README_zh-CN.md` | Simplified Chinese documentation |

## Notes

- The GitHub Actions workflow is pinned to ESPHome 2026.9.x to keep public builds reproducible.
- Flashing replaces the firmware currently on the device. Back up any configuration you need first.
- This is a community project for the specific hardware shown above; it is not a universal firmware image for other ESP32-S3 boards.
