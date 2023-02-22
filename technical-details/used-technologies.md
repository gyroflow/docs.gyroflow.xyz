# 🦀 Used technologies

## Used languages and technologies

_Gyroflow_ is written in [Rust](https://www.rust-lang.org/), with UI written in [QML](https://doc.qt.io/qt-6/qmlfirststeps.html). It uses _Qt_, _ffmpeg_, _OpenCV_ and _mdk-sdk_ external dependencies for the main program, but the core library is written in pure Rust without any external dependencies.

OpenCV usage is kept to a minimum, used only for lens calibration and optical flow (`src/core/calibration/mod.rs` and `src/core/synchronization/opencv.rs`). Core algorithms and undistortion don't use OpenCV.

GPU stuff supports _DirectX_, _OpenGL_, _Metal_ and _Vulkan_ thanks to _Qt RHI_ and _wgpu_. For GPU processing we use _OpenCL_ or _wgpu_, with highly parallelized CPU implementation as a fallback.

Zero-copy preview in the UI is implemented using Qt RHI and everything from video decoding, through processing to UI display is done on the GPU without the image data ever reaching CPU.

Zero-copy processing for video editor plugins supports: OpenCL buffers, CUDA buffers, Metal buffers and textures, Vulkan textures and DirectX11 textures. The OpenCL and wgpu interop is implemented [here](https://github.com/gyroflow/gyroflow/tree/master/src/core/gpu), was very challenging to do and resulted in multiple contributions to other Rust packages ([#1](https://github.com/cogciprocate/ocl/pull/212), [#2](https://github.com/gfx-rs/wgpu/pull/3338), [#3](https://github.com/gfx-rs/wgpu/pull/3355))&#x20;

## Why Rust?

Rust is an amazing and performant systems language. Gyroflow's nature requires a language that allows to achieve the most possible performance and use CPU cores and GPU devices to the maximum.\
Rust's _fearless concurrency_, _memory safety_ and extremely convenient package system allowed Gyroflow to grow very fast and avoid any memory issues and most crashes. This resulted in Gyroflow being extremely stable while using a lot of threading and supporting all possible graphics APIs.\
Rust's [`wgpu` ](https://github.com/gfx-rs/wgpu)project fits perfectly for Gyroflow's GPU processing requirements.

## Why QML?

Unfortunately most native Rust GUI frameworks are still not mature enough to implement a fairly complex UI like Gyroflow's, while allowing low-level control for zero-copy rendering on various graphics APIs.

QML on the other hand is a very robust and proven UI framework, which provides all necessary features while being implemented in a very efficient way. This ticks all the boxes for Gyroflow's UI requirements and was very easy to work with. It runs natively on DirectX, Vulkan, Metal and OpenGL through Qt RHI.

Rust <-> QML interop is also quite easy through the [`qmetaobject-rs`](https://github.com/woboq/qmetaobject-rs) crate. It required to write some C++ code for some low-level Qt interaction, but even in a complex app like Gyroflow, the amount of C++ needed was very low and acceptable.

QML supports hot-reloading so the UI development is very fast and easy, it has great support for animations and the UI rendering performance is great. The resulting UIs can run everywhere, not only desktop (Windows/Linux/macOS), but also mobile (Android/iOS) and even web browsers with WASM and WebGL.



Other UIs could be written on top of **gyroflow\_core,** but I don't see an use case for that other than for fun.



