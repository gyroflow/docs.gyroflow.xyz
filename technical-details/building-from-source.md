# 🧪 Building from source

## Dev environment

`Visual Studio Code` with `rust-analyzer` extension.

For working with QML I recommend to use Qt Creator and load all QML files there, as it has auto-complete and syntax highlighting. The project also supports UI live reload, it's a super quick way of working with the UI. Just change `live_reload = true` in `gyroflow.rs` and it should work right away. Now every time you change any QML file, the app should reload it immediately.

## Building steps

{% tabs %}
{% tab title="Windows" %}
1. Get latest stable Rust language from: [https://rustup.rs/](https://rustup.rs/)
   * Please make sure to check the English language pack option when installing the C++ build tools from Visual Studio Installer
2. Clone the repo: `git clone https://github.com/gyroflow/gyroflow.git`
3. Install dependencies to the `ext` directory: `cd gyroflow/ext`
   * `Qt 6.4.2` or higher: `pip3 install -U pip & pip3 install aqtinstall` then `aqt install-qt windows desktop 6.4.2 win64_msvc2019_64 -m qtshadertools` or use the [official installer](https://www.qt.io/download-qt-installer)
   * `FFmpeg 5.1`: download [ffmpeg 5.1 lite](https://sourceforge.net/projects/avbuild/files/windows-desktop/ffmpeg-master-windows-desktop-vs2022-gpl-lite.7z/download) and unzip it to `ext/ffmpeg-master-windows-desktop-vs2022-gpl-lite`
   * vcpkg: `git clone --depth 1 https://github.com/Microsoft/vcpkg.git & .\vcpkg\bootstrap-vcpkg.bat -disableMetrics`
   * OpenCV: `.\vcpkg\vcpkg install "opencv[core]:x64-windows-release"`
   * OpenCL: `.\vcpkg\vcpkg install "opencl:x64-windows-release"`
   * LLVM: download and install [LLVM](https://github.com/llvm/llvm-project/releases/download/llvmorg-15.0.4/LLVM-15.0.4-win64.exe)
4. Make sure you have `7z` in PATH.
5. Setup the environment in powershell (or set the same variables in cmd): `./__env.ps1` - I do this in VS Code built-in terminal
   * some machines might have issue that scripts are forbidden to run, try to `set-executionpolicy remotesigned` in powershell with admin
6. Compile and run: `cargo run --release`
{% endtab %}

{% tab title="macOS" %}
1. Get latest stable Rust language from: [https://rustup.rs/](https://rustup.rs/)
2. Install Xcode command line tools: `xcode-select --install`
3. Clone the repo: `git clone https://github.com/gyroflow/gyroflow.git`
4. Install dependencies: `cd gyroflow/ext && ./install-deps-mac.sh`
5. Setup the environment in terminal: `source __env-macos.sh` or `. ./__env-macos.sh` - I do this in VS Code built-in terminal
6. Compile and run: `cargo run --release`
7. If it fails to run, do: `./_deployment/deploy-macos.sh` once
{% endtab %}

{% tab title="Linux" %}
1. Get latest stable Rust language from: [https://rustup.rs/](https://rustup.rs/)
2. Clone the repo: `git clone https://github.com/gyroflow/gyroflow.git`
3. Install dependencies: `cd gyroflow/ext && ./install-deps-linux.sh` (Debian based apt)
4. Setup the environment in terminal: `./__env-linux.sh` - I do this in VS Code built-in terminal
5. Compile and run: `cargo run --release`
{% endtab %}

{% tab title="Android" %}
1. Android is not well supported yet, but the app can be built and somewhat works. So far only building on Windows was tested
2. Install Qt for Android: `aqt install-qt windows android 6.4.2 android_arm64_v8a` and `aqt install-qt windows desktop 6.4.2 win64_mingw`
3. Install `cargo-apk`: `cargo install --git https://github.com/rust-windowing/android-ndk-rs.git cargo-apk`
4. Add a Rust target: `rustup target add aarch64-linux-android`
5. Update paths in `_deployment/build-android.ps1`
6. Run `.\_deployment\build-android.ps1` in Powershell
{% endtab %}

{% tab title="iOS" %}
1. iOS is not well supported yet, work in progress
2. Get latest stable Rust language from: [https://rustup.rs/](https://rustup.rs/)
3. Install Xcode command line tools: `xcode-select --install`
4. Clone the repo: `git clone https://github.com/gyroflow/gyroflow.git`
5. Install dependencies: `cd gyroflow/ext && ./install-deps-ios.sh`
6. Setup the environment in terminal: `source __env-ios.sh` or `. ./__env-ios.sh` - I do this in VS Code built-in terminal
7. Compile and run: `cargo build --target aarch64-apple-ios --release`
8. Rest of the steps not figured out yet. Now need to package the app, create .ipa and sign it
{% endtab %}
{% endtabs %}

## Profiling

{% tabs %}
{% tab title="Windows" %}
1. Install and run `Visual Studio Community Edition`
2. Compile and run Gyroflow with the `profile` profile: `cargo run --profile profile`
3. In Visual Studio, go to `Debug -> Performance Profiler...`
   * Under `Target`, open `Change Target` and select `Running Process...`, select the running `gyroflow.exe` process
{% endtab %}

{% tab title="QML" %}
1. Uncomment `config.define("QT_QML_DEBUG", None);` in `build.rs`
2. Comment `cli::run()` in `gyroflow.rs`
3. Run in debug mode with QML debugger args: `cargo run -- "-qmljsdebugger=port:1234,block,services:CanvasFrameRate,EngineControl,DebugMessages"`
4. In Qt Creator go to `Analyze` -> `QML Profiler (Attach to Waiting Application)` and enter port 1234
{% endtab %}
{% endtabs %}
