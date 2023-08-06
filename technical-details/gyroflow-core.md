---
description: >-
  Gyroflow was designed with modularity in mind. Gyroflow Core is a library that
  can be used for video editor plugins, main program GUI and potentially in
  other places where video is processed.
---

# 🛠 Gyroflow Core

Gyroflow Core is a library that handles all stabilization algorithms and processing of video pixels.&#x20;

You can find the **source code** on GitHub [here](https://github.com/gyroflow/gyroflow/tree/master/src/core).

Gyroflow Core purposely excludes **ffmpeg** and thus the decoding and encoding of data. This also includes render queue, as render queue without a decoder and encoder doesn't make sense.

Whilst the [Command Line Interface (CLI)](../advanced-usage/command-line-cli.md) uses Gyroflow Core, it also includes **Qt** and **ffmpeg.** The Command Line Interface is useful when you need to automate Gyroflow, or do things in a programatic or batch way.&#x20;

Coding using Gyroflow Core is useful if you're building things like:

* Video Editor Plugins (i.e. OpenFX, FxPlug4)
* OBS Plugin for Real-time Stabilisation
* A web version of Gyroflow (compiled to WASM + processing with browser-provided video/canvas functions)
* A fork of Gyroflow, with the intention to use a different GUI than **Qt**, or different video processing API than **ffmpeg** (for example C# + MFT for a pure Windows use case).

`StabilizationManager` is the core struct of Gyroflow Core - it's the main interface. Whilst there isn't yet any official documentation for this, by referring to the existing OpenFX and FxPlug4 plugins, it should be easy enough to understand - especially if you're familiar with Rust.

Gyroflow Core has been designed with a few assumptions in mind:

### **Portability**

Gyroflow core doesn't make any assumptions about video data source, motion data source, user interface, video decoding, encoding or playback.

It is designed to be free of any large external dependencies and this makes it possible to compile for any system, any environment and any purpose.

It doesn't need **Qt**, **ffmpeg**, **OpenCV** or **mdk-sdk**, and yet it still handles everything needed for video stabilization.

### Comprehensiveness

It handles as much as it can, so the outer layers (e.g. the GUI) can be as thin as possible. This includes parsing of all gyro sources, GPU processing on all graphic APIs, drawing on the pixels, running the optical flow and synchronization algorithms, keyframes, lens profiles and of course the main stabilization algorithms.

### Performance

All hardware accelerated processing is handled inside the core so the outer layers don't need to know anything about it. Calculations of the motion data, stabilization and zooming are also multithreaded inside the core.

## Design

The library is designed to take the following **inputs**:

* Gyro data file path (or the contents in memory)
* Video information like resolution, frame rate, duration
*   Either:

    * `.gyroflow` project file, to load all parameters in one go

    or

    * Parameters available in the GUI as function calls (eg. `set_smoothing_param, set_horizon_lock, set_imu_orientation` etc
* Video pixels with a timestamp

And the **output** is stabilized video pixels.

Video pixels can be passed as:

* Byte array in memory `&[u8]`
* OpenCL `cl_mem`
* DirectX11 `ID3D11Texture2D *`
* Vulkan `VkImage`
* Metal texture `MTLTexture`
* Metal buffer `MTLBuffer`
* CUDA buffer `Cudeviceptr`

It's also able to handle hybrid solutions, like providing a Metal texture and returning `&[u8]` array in memory.

Currently there's no documentation of the API, but the entire code is available so you should able to figure out all needed calls by looking at the [current implementations](gyroflow-core.md#examples). Feel free to [contact Adrian](mailto:adrian@gyroflow.xyz) if you have questions about it, or join the [Discord](https://discord.com/invite/BBJ2UVAr2D).

The main app implements GUI in **QML** on top of Gyroflow Core, and uses **ffmpeg** to decode and encode the video files.

The **OpenFX** plugin uses pixels provided by the host (e.g. DaVinci Resolve), and settings loaded from `.gyroflow` project file

**Gyroflow Toolbox**, the **Final Cut Pro X** plugin, uses a `MTLTexture` provided by Final Cut Pro via the [FxPlug4 API](https://developer.apple.com/documentation/professional\_video\_applications/fxplug). It has the ability to import a `.gyroflow` project file, or it can programatically create a new internal Gyroflow Project if you're using a camera format that doesn't require synchronisation within the main Gyroflow application. Gyroflow Toolbox is primarily written in Objective-C, with a C interface to some Rust functions.

Optionally, **OpenCV** is used in Gyroflow Core for one of the optical flow algorithms (**DISOpticalFlow**) and the main app uses it as the default. However, if you're just loading a `.gyroflow` project file, then the optical flow is not needed, as it's only used to determine the offsets and not for the actual stabilization process.

Main app also uses **OpenCV** in the lens calibrator.

## Examples

* [The hero Gyroflow Application & GUI (Rust & QML)](https://github.com/gyroflow/gyroflow/blob/master/src/controller.rs)
* [The OpenFX Plugin for DaVinci Resolve (Rust)](https://github.com/gyroflow/gyroflow-ofx/blob/main/src/gyroflow.rs)
* [Gyroflow Toolbox - FxPlug4 Plugin for Final Cut Pro (Rust & Objective-C)](https://github.com/latenitefilms/GyroflowToolbox/blob/main/Source/Frameworks/gyroflow/src/lib.rs)
