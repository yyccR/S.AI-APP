# S.AI - Your Intelligent Companion

<p align="center">
      <img src="./asserts/images/app_logo/s.ai.256x225.png" alt="logo"/>
</p>

<p align="center">
  <a href="https://github.com/yyccR/S.AI"><img alt="GitHub stars" src="https://img.shields.io/github/stars/yyccR/S.AI?style=social"></a>
  <a href="https://github.com/yyccR/S.AI/issues"><img alt="GitHub issues" src="https://img.shields.io/github/issues/yyccR/S.AI"></a>
  <a href="https://github.com/yyccR/S.AI/blob/main/LICENSE"><img alt="License" src="https://img.shields.io/github/license/yyccR/S.AI"></a>
  <a href="https://github.com/yyccR/S.AI/actions/workflows/android.yml"><img alt="Android Build" src="https://github.com/yyccR/S.AI/actions/workflows/android.yml/badge.svg?branch=develop_flutter_3.27.4"></a>
</p>

<p align="center">
  <a href="./README_CN.md"><img alt="简体中文版自述文件" src="https://img.shields.io/badge/简体中文-d9d9d9"></a>
</p>

## 🌟 Introduction

S.AI is a cutting-edge, cross-platform AI application. Designed for seamless interaction and intelligent solutions, S.AI leverages advanced artificial intelligence models to enhance user experience across various devices. Discover the future of intelligent assistance with S.AI.

## ✨ Key Features

S.AI (**S**maller, **S**wifter, **S**marter AI) is a powerful, open-source platform designed for the seamless deployment, release, and utilization of AI models. It offers a comprehensive suite of features, including:

*   **Support Platform:** Run on Android, iOS, Web, and desktop.
*   **Support Framework:** Integrates with TFLite, NCNN, Paddle Lite, ONNX Runtime, MNN, TNN, GGML, and more.
*   **Support Cloud:** Connects to APIs from major cloud providers like Google, Microsoft, and Alibaba.
*   **Support Workflows:** Build adaptive workflows by combining various AI algorithms.
*   **Support self-defined-Apis:** Fetch, parse, and visualize data from custom HTTP sources.
*   **Support Model Management:** Test, evaluate, and publish AI models for mobile and cloud.
*   **Support Hub:** Publish and share innovative AI models or tools.
*   **Support Knowledge:** Access SOTA papers, books, and educational videos.

## 🚀 Quick Start

Follow these steps to get S.AI up and running on your local machine.

### Prerequisites

*   **Flutter SDK (Recommended: FVM)**
    To ensure consistent Flutter SDK versions across your development team, we highly recommend using [Flutter Version Management (FVM)](https://fvm.app/documentation/getting-started/installation).

    **MacOS/Linux:**
    ```bash
    curl -fsSL https://fvm.app/install.sh | bash
    ```

    **Window:** Install FVM using Chocolatey from the command line or PowerShell:
    ```bash
    choco install fvm
    ```
    For other operating systems or detailed installation instructions, please refer to the [FVM official documentation](https://fvm.app/documentation/getting-started/installation).

*   **Platform-Specific Dependencies**
    For detailed instructions on setting up platform-specific dependencies like the Android SDK, NDK, and Xcode, please refer to our [Additional Installation Guide](./docs/ADDITIONAL_INSTALLATION.md).

### Installation

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/yyccR/S.AI.git
    cd S.AI
    ```

2.  **Install Flutter dependencies (using FVM 3.27.4):**
    Ensure FVM is set up and then use Flutter version 3.27.4:
    ```bash
    fvm install 3.27.4
    fvm use 3.27.4
    flutter pub get
    ```

3.  **Running the Application**
    Connect a device or start an emulator, then run:

    ```bash
    fvm flutter run
    ```

    For specific platforms:

    *   **To get device ID:** Run `fvm flutter devices` to list available devices and their IDs.
    *   **Android:** `fvm flutter run --device-id <your_android_device_id>`
    *   **iOS:** `fvm flutter run --device-id <your_ios_device_id>`
    *   **Web:** `fvm flutter run -d chrome` (or your preferred browser)
    *   **Desktop (Windows/macOS/Linux):** `fvm flutter run -d <platform>` (e.g., `fvm flutter run -d windows`)

## 🖼️ Project Preview

See S.AI in action:

<!-- Add more screenshots or a GIF here if available -->

## ⬇️ Download

S.AI is available on multiple platforms. Click the links below to download:

*   **Android:** [Download APK (Coming Soon)]()
*   **iOS:** [Download on App Store (Coming Soon)]()
*   **Web:** [Launch Web App (Coming Soon)]()
*   **Windows:** [Download Installer (Coming Soon)]()
*   **macOS:** [Download DMG (Coming Soon)]()
*   **Linux:** [Download AppImage/Deb/RPM (Coming Soon)]()

## 🗓️ Release Plan

We are committed to continuously improving S.AI. Here's a glimpse of our future plans:

*   **2025 Q3 (v1.1):** Expand support for more ONNX, TFLite, and cloud models.
*   **2025 Q4 (v1.2):** Introduce workflow functionality with support for logical computation and tool nodes.
*   **2026 Q1 (v1.3):** Launch a Hub space for model downloading and sharing.

Stay tuned for more exciting developments!

## 💡 Supported Frameworks and Models

|                                                                                                                                                                     | ncnn | tflite | paddle lite | onnxruntime | mnn    | tnn    | ggml    | cloud api | ~~tvm~~  | ~~tengine~~ | ~~openvino~~ |
|:-------------------------------------------------------------------------------------------------------------------------------------------------------------------:|:----:|:------:|:--------:|:--------:| :----: | :----: | :----: |:---------:|:--------:|:-----------:|:------------:|
|                                                         [nanodet plus](https://github.com/RangiLyu/nanodet)                                                         |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                           [yolov5](https://github.com/ultralytics/yolov5)                                                           |      |   ✅    |          |          |        |        |        |           |          |              |              |
|                                            [yolov5(v6.0 ~ v7.0)](https://github.com/ultralytics/yolov5/releases/tag/6.0)                                            |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                              [yolov5-segment](https://github.com/ultralytics/yolov5/releases/tag/7.0)                                               |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                         [yolov5-lite](https://github.com/ppogg/YOLOv5-Lite)                                                         |      |        |          |      ✅   |        |         |        |           |          |              |              |
|                                                             [yolov6](https://github.com/meituan/YOLOv6)                                                             |      |        |          |          |        | ✅      |        |           |          |              |              |
|                                                           [yolov7](https://github.com/WongKinYiu/yolov7)                                                            |      |        |      ✅   |          |        |         |        |           |          |              |              |
|                                                       [yolox](https://github.com/Megvii-BaseDetection/YOLOX)                                                        |      |        |          |          |  ✅    |         |        |           |          |              |              |
|                                                     [ultralytics:yolov3](https://github.com/ultralytics/yolov3)                                                     |      |        |          |          |        |         |        |           |          |               |              |
|                                                        [PPOCR-v5](https://github.com/PaddlePaddle/PaddleOCR)                                                        |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                      [yolov8](https://docs.ultralytics.com/zh/models/yolov8/)                                                       |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                    [yolov8-pose](https://docs.ultralytics.com/zh/models/yolov8/)                                                    |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                  [yolov8-segment](https://docs.ultralytics.com/zh/models/yolov8/)                                                   |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                    [yolov8-obb](https://docs.ultralytics.com/zh/models/yolov8/)                                                     |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                    [yolov8-cls](https://docs.ultralytics.com/zh/models/yolov8/)                                                     |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                 [yolov8-worldv2](https://docs.ultralytics.com/zh/models/yolo-world)                                                 |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                      [yolov9](https://docs.ultralytics.com/zh/models/yolov9/)                                                       |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                      [yolov11](https://docs.ultralytics.com/zh/models/yolo11)                                                       |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                   [yolov11-pose](https://docs.ultralytics.com/zh/models/yolo11/)                                                    |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                    [yolov11-seg](https://docs.ultralytics.com/zh/models/yolo11/)                                                    |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                    [yolov11-obb](https://docs.ultralytics.com/zh/models/yolo11/)                                                    |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                        [yolov12](https://github.com/ultralytics/ultralytics)                                                        |      |        |          |          |        |        |        |           |          |              |              |
|                                                     [yolov12-pose](https://github.com/ultralytics/ultralytics)                                                      |      |        |          |          |        |        |        |           |          |              |              |
|                                                      [yolov12-seg](https://github.com/ultralytics/ultralytics)                                                      |      |        |          |          |        |        |        |           |          |              |              |
|                                                      [yolov12-obb](https://github.com/ultralytics/ultralytics)                                                      |      |        |          |          |        |        |        |           |          |              |              |
|                                                       [yolo-nas](https://github.com/Deci-AI/super-gradients)                                                        |      |        |          |      ✅   |        |        |        |           |          |              |              |
|                                                   [sherpa-ncnn<br/>(stt)](https://github.com/k2-fsa/sherpa-ncnn)                                                    |  ✅   |        |          |          |        |        |        |           |          |              |              |
|                                                   [sherpa-onnx<br/>(tts)](https://github.com/k2-fsa/sherpa-onnx)                                                    |      |        |          |      ✅   |        |        |        |           |          |              |              |
|                                                            [whisper](https://github.com/openai/whisper)                                                             |      |   ✅    |          |          |        |         |   ✅    |           |          |              |              |
|                                              [chatgpt](https://platform.openai.com/docs/api-reference/authentication)                                               |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                   [openai-vsion](https://platform.openai.com/docs/guides/vision)                                                    |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                     [moonshot](https://platform.moonshot.cn/docs/intro#文本生成模型)                                                      |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                    [Doubao](https://team.doubao.com/zh/?view_from=homepage_tab)                                                     |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                 [Doubao-vision](https://team.doubao.com/zh/?view_from=homepage_tab)                                                 |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                                [deepseek](https://www.deepseek.com/)                                                                |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                           [gemini](https://ai.google.dev/gemini-api/docs)                                                           |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                        [claude](https://docs.anthropic.com/en/api/overview)                                                         |      |        |          |          |        |         |        |     ✅     |          |              |              |
|  [qwen](https://bailian.console.aliyun.com/console?tab=api#/api/?type=model&url=https%3A%2F%2Fhelp.aliyun.com%2Fdocument_detail%2F2833609.html&renderType=iframe)   |      |        |          |          |        |         |        |     ✅     |          |              |              |
| [wanx2.1](https://bailian.console.aliyun.com/console?tab=api#/api/?type=model&url=https%3A%2F%2Fhelp.aliyun.com%2Fdocument_detail%2F2867393.html&renderType=iframe) |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                        [CosyVoice](https://github.com/FunAudioLLM/CosyVoice)                                                        |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                       [fish-speech](https://github.com/fishaudio/fish-speech)                                                       |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                        [GPT-SoVITS](https://github.com/RVC-Boss/GPT-SoVITS)                                                         |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                   [LTX(ComfyUI)](https://github.com/Lightricks/ComfyUI-LTXVideo)                                                    |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                               [CogVideoX(ComfyUI)](https://github.com/kijai/ComfyUI-CogVideoXWrapper)                                               |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                                   [拍我AI](https://pai.video/home)                                                                    |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                                [PixVerse](https://app.pixverse.ai/)                                                                 |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                  [Hailuo(minimax)](https://www.minimax.io/news/minimax-hailuo-02)                                                   |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                       [paddleocr](https://github.com/PaddlePaddle/PaddleOCR)                                                        |      |        |      ✅   |          |        |         |        |           |          |              |              |
|                                                  [chineseocr_lite](https://github.com/DayBreak-u/chineseocr_lite)                                                   |  ✅   |        |          |          |        |         |        |           |          |              |              |
|                                                          [deep sort](https://github.com/nwojke/deep_sort)                                                           |  ✅   |        |          |          |        |         |        |           |          |              |              |
|                                                    [volcengine stt](https://www.volcengine.com/docs/6561/163032)                                                    |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                               [azure stt](https://learn.microsoft.com/zh-tw/azure/ai-services/speech-service/index-speech-to-text)                                  |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                    [volcengine tts](https://www.volcengine.com/docs/6561/163032)                                                    |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                    [volcengine ocr](https://www.volcengine.com/docs/6790/117730)                                                    |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                 [volcengine translate](https://www.volcengine.com/docs/4640/65067)                                                  |      |        |          |          |        |         |        |     ✅     |          |              |              |
|                                                            [Real-SR](https://github.com/Tencent/Real-SR)                                                            |  ✅   |        |          |          |        |         |        |           |          |              |              |
|                                                          [ESRGAN](https://github.com/xinntao/Real-ESRGAN)                                                           |  ✅   |        |          |          |        |         |        |           |          |              |              |
|                                                 [silero-vad(v5)](https://github.com/snakers4/silero-vad/tree/v5.0)                                                  |      |        |          |     ✅    |        |         |        |           |          |              |              |
|                                                 [silero-vad(v4)](https://github.com/snakers4/silero-vad/tree/v4.0)                                                  |      |        |          |     ✅    |        |         |        |           |          |              |              |

## 💬 Community & Contact

*   **GitHub Discussions:** Best for sharing feedback and asking questions. (Link to your GitHub Discussions)
*   **GitHub Issues:** Best for reporting bugs and proposing features. (Link to your GitHub Issues)
*   **QQ Group:** Join our QQ group for technical discussions and community interaction. (QQ Group Number: 728382168)

## 👋 Contributing

We welcome contributions to S.AI! Please see our [Contribution Guide](LINK_TO_CONTRIBUTING_GUIDE) for more information on how to get started.

## 📜 License

S.AI is licensed under a modified version of the Apache License 2.0, with the following additional conditions:

1.  **Authorization:** If you want to use S.AI in your company's commercial products, regardless of whether there is a profit-making behavior or not, you must obtain written authorization from the producer of S.AI. Otherwise, using the S.AI source code to build and release your products is not allowed.

2.  **LOGO and copyright information:** When integrating the S.AI source code into your products or interfaces, you shall not remove or modify the LOGO and copyright information of the S.AI application.

3.  **As a contributor, you should agree that:**
    a. The producer can adjust the open-source agreement to be either more stringent or more lenient as deemed necessary.
    b. The code you contribute may be used for commercial purposes, including but not limited to cloud business operations.

Except for the specific conditions mentioned above, all other rights and restrictions are in accordance with the Apache License 2.0. Detailed information about the Apache License 2.0 can be found at http://www.apache.org/licenses/LICENSE - 2.0.

The unique interface design and interactive elements of S.AI are protected by relevant patents and intellectual property laws.

© 2025 S.AI


