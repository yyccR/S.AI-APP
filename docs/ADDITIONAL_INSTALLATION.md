# Additional Installation and Setup

For developing and running S.AI on specific platforms, you might need to install additional dependencies beyond the Flutter SDK. This guide provides an overview of the necessary components.

## Android Development

To build and run S.AI for Android, you will need the following:

### Android SDK

The Android SDK is required for building Android applications. It is typically bundled with Android Studio.

1.  **Install Android Studio:** Download and install it from the [official website](https://developer.android.com/studio).
2.  **SDK Setup:** Follow the setup wizard in Android Studio to install the latest Android SDK, command-line tools, and build tools.

### Android NDK

For AI models that rely on native C++ code (such as those using `ncnn` or other custom libraries), the Android Native Development Kit (NDK) is essential.

*   **Installation:** You can install the NDK via the SDK Manager in Android Studio.
    1.  Go to `Tools > SDK Manager`.
    2.  Select the `SDK Tools` tab.
    3.  Check `NDK (Side by side)` and click `Apply` to install.

## iOS Development

To build and run S.AI for iOS, you will need a macOS machine with the following:

### Xcode

Xcode is the integrated development environment (IDE) for iOS development.

1.  **Install Xcode:** Download and install it from the Mac App Store.
2.  **Command-Line Tools:** After installing Xcode, open a terminal and run `xcode-select --install` to install the command-line tools.
3.  **Simulator/Device Setup:** Use Xcode to set up an iOS Simulator or connect a physical Apple device for testing.

## Further Information

For more detailed setup instructions, please refer to the official documentation for [Flutter](https://flutter.dev/docs/get-started/install), [Android](https://developer.android.com/docs), and [iOS](https://developer.apple.com/documentation). 