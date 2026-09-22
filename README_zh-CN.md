# StickS3 Voice

[English](README.md) | [简体中文](README_zh-CN.md)

这是专门为 **M5Stack StickS3 K150 / StickS3** 制作的 ESPHome 语音助手固件。它不是通用的 ESP32-S3 固件，请只在对应的 StickS3 硬件上使用。

![M5Stack StickS3 K150 硬件概览](docs/sticks3-hardware.png)

## 功能

- Home Assistant 语音卫星
- 使用设备端 `hey_jarvis` 唤醒词
- 通过 StickS3 的 ES8311 音频编解码器使用麦克风和扬声器
- 135 × 240 彩色 LCD，显示糖果色动态机器人
- 显示正在聆听、思考和播报等语音状态
- 在屏幕上显示语音识别文本和助手回复
- 显示电池状态图标和 Wi-Fi 信号
- 在 Home Assistant 中提供电池百分比、电池电压和 Wi-Fi 信号传感器
- 支持正面按键启动语音助手
- 回答结束后自动返回机器人待机画面
- 可通过 Improv Serial 或备用配网页面完成 Wi-Fi 设置
- 设备联网后支持 OTA 更新
- 自动在设备名后加入 MAC 地址后缀，避免多台设备重名

## 支持的硬件

本项目适用于 **M5Stack StickS3 K150 / StickS3**，包括：

- ESP32-S3-PICO-1-N8R8
- 8 MB Flash
- 8 MB PSRAM
- 1.14 英寸 135 × 240 ST7789P3 彩色 LCD
- ES8311 音频编解码器
- 内置麦克风
- 内置扬声器
- 正面按键
- 250 mAh 电池

StickS3 还带有红外收发、IMU 和扩展接口，但当前固件没有启用这些功能。Bluetooth Proxy 也有意保持关闭，以减少它对 Wi-Fi 和语音功能的影响。

## 隐私与公开分享

本仓库不包含：

- 个人 Wi-Fi 名称或密码
- 固定的 Home Assistant API 加密密钥
- Home Assistant 访问令牌
- 其他属于项目作者的私人凭据

每位用户都应在刷机后配置自己的 Wi-Fi，并将设备接入自己的 Home Assistant。

如果你修改或派生本项目，请勿把以下内容提交到公开仓库：

- `secrets.yaml`
- Wi-Fi 密码
- Home Assistant 访问令牌
- API 密钥
- 其他个人或家庭网络信息

## 网页一键安装

普通用户不需要安装 ESPHome、下载源代码或创建 GitHub 仓库。

1. 使用桌面版 Google Chrome 或 Microsoft Edge 打开：
   [StickS3 Voice 网页安装器](https://thenexthop2025.github.io/sticks3-voice/)
2. 使用支持数据传输的 USB 线把 StickS3 连接到电脑。
3. 点击 **Connect**。
4. 选择 StickS3 对应的 USB 串口。
5. 按照页面提示安装固件。
6. 在刷写和校验完成前保持设备连接。
7. 安装完成后重新启动设备并配置 Wi-Fi。

> 网页安装会覆盖设备当前的固件。请勿选择“擦除设备”，除非确实需要完全清空设备。

## 直接下载固件

需要使用其他烧录工具的用户，可以直接下载已经编译好的固件：

[下载 firmware.factory.bin](https://thenexthop2025.github.io/sticks3-voice/firmware.factory.bin)

这个文件是适用于首次完整刷机的 factory 固件。

## 浏览器要求

网页刷机依赖 Web Serial，请使用最新版桌面端 Chromium 浏览器，例如：

- Google Chrome
- Microsoft Edge

以下浏览器或设备通常不能使用这个网页刷机流程：

- Safari
- Firefox
- 大多数手机和平板浏览器

安装页面必须通过 HTTPS 提供，GitHub Pages 会自动满足这个要求。

如果串口列表中没有出现 StickS3：

1. 确认 USB 线支持数据传输，而不只是充电。
2. 尽量直接连接电脑，不要经过扩展坞。
3. 关闭可能正在占用串口的程序。
4. 重新插拔设备并刷新网页。
5. 如需进入下载模式，连接 USB 后长按设备侧键约 2 秒，看到内部绿色指示灯闪烁后松开。

不要选择 Mac 自带的 Bluetooth、debug-console 或 wlan-debug 端口。StickS3 通常会显示为新的 USB、Espressif 或 `cu.usbmodem` 端口。

## 首次配网

刷机完成后，可以通过以下方式配置 Wi-Fi：

- 使用支持 Improv Serial 的客户端通过 USB 配网。
- 连接设备临时创建的 **StickS3 Voice Setup** Wi-Fi，然后在弹出的配网页面中输入自己的 Wi-Fi 名称和密码。

这个热点只用于初次配网。设备成功连接你的 Wi-Fi 后，会使用你配置的网络正常运行。

如果配网页面没有自动弹出，可以在连接 **StickS3 Voice Setup** 后尝试访问：

```text
http://192.168.4.1/
```

## 接入 Home Assistant

1. 确保 StickS3 与 Home Assistant 位于可以互相访问的网络中。
2. 在 Home Assistant 中打开 **设置 → 设备与服务**。
3. 接受自动发现的 ESPHome 设备。
4. 如果没有自动发现，选择 **添加集成 → ESPHome**。
5. 输入 StickS3 的主机名或 IP 地址。
6. 按照界面完成 ESPHome 配对。
7. 为设备设置要使用的 Home Assistant Assist 管线和语言。

设备主机名通常以 `sticks3-voice` 开头。由于固件启用了 MAC 地址后缀，不同用户和不同设备不会轻易发生名称冲突。

固件中没有内置属于项目作者的 Home Assistant API 密钥。

## 唤醒与使用

设备正常连接 Home Assistant 后，可以通过以下方式启动语音助手：

- 说出唤醒词：`Hey Jarvis`
- 按下设备正面的蓝色按键

屏幕会根据状态显示：

- Listening：正在聆听
- Thinking：正在处理
- Speaking：正在播报

回答结束后，设备会自动返回机器人待机画面。

## 本地编译和刷机

高级用户可以安装 ESPHome 2026.9.x，并在项目目录运行：

```bash
esphome run sticks3-voice.yaml
```

如果只需要编译：

```bash
esphome compile sticks3-voice.yaml
```

本地编译生成的文件位于 `.esphome` 构建目录中。

首次刷机可以使用 USB。设备联网后，也可以把它接管到自己的 ESPHome Dashboard，并使用 OTA 方式维护后续配置。

## GitHub Pages 自动构建

仓库中的 GitHub Actions 工作流会在每次向 `main` 分支提交修改后自动：

1. 安装 ESPHome 2026.9.x。
2. 编译 `sticks3-voice.yaml`。
3. 查找生成的 `firmware.factory.bin`。
4. 生成网页安装器。
5. 把安装页面和固件发布到 GitHub Pages。

也可以在 GitHub 仓库的 **Actions** 页面手动运行工作流。

每次成功部署后，安装地址保持不变：

```text
https://thenexthop2025.github.io/sticks3-voice/
```

## 项目文件说明

| 路径 | 用途 |
| --- | --- |
| `sticks3-voice.yaml` | ESPHome 固件配置 |
| `docs/sticks3-hardware.png` | 两个 README 共用的 StickS3 硬件参考图 |
| `site/index.html` | 浏览器一键安装页面 |
| `site/manifest.json` | ESP Web Tools 固件清单 |
| `.github/workflows/pages.yml` | 自动编译固件并发布 GitHub Pages |
| `.gitignore` | 排除本地构建结果、私密配置和生成的固件 |
| `README.md` | 英文说明 |
| `README_zh-CN.md` | 简体中文说明 |

## 注意事项

- GitHub Actions 工作流固定使用 ESPHome 2026.9.x，以便公开构建结果保持一致。
- 刷机会替换设备当前的固件，请先备份需要保留的配置。
- 进入下载模式或异常重启后，如果电池图标暂时消失，可以拔掉 USB、双击侧键完全关机，等待约 10 秒后再单击开机。
- 电池百分比根据电压估算，不是精密的电量计读数。
- 这是针对上图所示硬件的社区项目
