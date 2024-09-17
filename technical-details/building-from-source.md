# 🧪 Building from Source

## Dev environment

`Visual Studio Code` with `rust-analyzer` extension.

For working with QML I recommend to use Qt Creator and load all QML files there, as it has auto-complete and syntax highlighting. The project also supports UI live reload, it's a super quick way of working with the UI. Just change `live_reload = true` in `gyroflow.rs` and it should work right away. Now every time you change any QML file, the app should reload it immediately.

## Building steps

{% tabs %}
{% tab title="Windows" %}
1. Prerequisites: `git`, `7z` and working `powershell`. If you never ran powershell scripts before, run `set-executionpolicy remotesigned` in powershell as admin
2. Get latest stable Rust language from: https://rustup.rs/
   * Please make sure to check the English language pack option when installing the C++ build tools from Visual Studio Installer
3. Install `Just` by running `cargo install --force just`
4. Clone the repo: `git clone https://github.com/gyroflow/gyroflow.git`
5. Enter the project directory and:
   * Install dependencies: `just install-deps`
   * Compile and run: `just run`
{% endtab %}

{% tab title="macOS" %}
1. Prerequisites: `git`, `brew`
2. Get latest stable Rust language from: https://rustup.rs/
3. Install `Just` by running `cargo install --force just`
4. Clone the repo: `git clone https://github.com/gyroflow/gyroflow.git`
5. Enter the project directory and:
   * Install dependencies: `just install-deps`
   * Compile and run: `just run`
   * The first time you run it won't work, run `just deploy` once and then `just run` will work
{% endtab %}

{% tab title="Linux" %}
1. Prerequisites: `git`, `7z`, `python`, `apt` package manager (or adjust commands inside scripts if on different distro)
2. Get latest stable Rust language from: https://rustup.rs/
3. Install `Just` by running `cargo install --force just`
4. Clone the repo: `git clone https://github.com/gyroflow/gyroflow.git`
5. Enter the project directory and:
   * Install dependencies: `just install-deps`
   * Compile and run: `just run`
{% endtab %}

{% tab title="Android" %}
1. Prerequisites: `git`, `7z`, working `powershell`, Android SDK and NDK. Building is supported only on Windows
2. Get latest stable Rust language from: https://rustup.rs/
3. Install `Just` by running `cargo install --force just`
4. Clone the repo: `git clone https://github.com/gyroflow/gyroflow.git`
5. Install Android SDK and NDK r23c and update paths in `_scripts/android.just`
6. Enter the project directory and:
   * Install dependencies: `just android install-deps`
   * Compile the apk and install on device: `just android deploy`
{% endtab %}

{% tab title="iOS" %}
1. Prerequisites: `git`, `brew`
2. Get latest stable Rust language from: https://rustup.rs/
3. Install `Just` by running `cargo install --force just`
4. Clone the repo: `git clone https://github.com/gyroflow/gyroflow.git`
5. Enter the project directory and:
   * Install dependencies: `just ios install-deps`
   * Update Team ID, signing keys and provisioning profiles in `_scripts/ios.just`
   * Compile and run on device: `just ios run`
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
