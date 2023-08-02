---
description: >-
  Gyroflow was designed with modularity in mind. The core is a library that can
  be used for video editor plugins, main program GUI and potentially in other
  places where video is processed.
---

# 🛠 Gyroflow core

[Gyroflow core](https://github.com/gyroflow/gyroflow/tree/master/src/core) is a library that handles all stabilization algorithms and processing of video pixels. It is designed with a few assumptions in mind:

### **Portability**

Gyroflow core doesn't make any assumptions about video data source, motion data source, user interface, video decoding, encoding or playback.

It is designed to be free of any large external dependencies and this makes it possible to compile  for any system, any environment and any purpose.

It doesn't need Qt, ffmpeg, OpenCV or mdk-sdk, and yet it still handles everything needed for video stabilization.

### Comprehensiveness

It handles as much as it can, so the outer layers (e.g. the GUI) can be as thin as possible. This includes parsing of all gyro sources, GPU processing on all graphic APIs, drawing on the pixels, running the optical flow and synchronization algorithms, keyframes, lens profiles and of course the main stabilization algorithms.

### Performance

All hardware accelerated processing is handled inside the core so the outer layers don't need to know anything about it. Calculations of the motion data, stabilization and zooming are also multithreaded inside the core.

## Design

The library is designed to take the following inputs:

* Gyro data file path (or the contents in memory)
* Video information like resolution, frame rate, duration
*   Either&#x20;

    * `.gyroflow` project file, to load all parameters in one go

    or

    * parameters available in the GUI as function calls (eg. `set_smoothing_param, set_horizon_lock, set_imu_orientation` etc
* Video pixels with a timestamp

And the output is stabilized video pixels.

Video pixels can be passed as:&#x20;

* Byte array in memory `&[u8]`
* OpenCL `cl_mem`
* DirectX11 `ID3D11Texture2D *`
* Vulkan `VkImage`
* Metal texture `MTLTexture`
* Metal buffer `MTLBuffer`
* CUDA buffer `Cudeviceptr`

It's also able to handle hybrid solutions, like providing a Metal texture and returning `&[u8]` array in memory

Currently there's no documentation of the API, but the entire code is available so you should able to figure out all needed calls by looking at the [current implementations](gyroflow-core.md#examples). Feel free to [contact me](mailto:adrian@gyroflow.xyz) if you have questions about it.&#x20;

The main app implements GUI in QML on top of the core, and uses ffmpeg to decode and encode the video files.

OpenFX plugin uses pixels provided by the host (e.g. DaVinci Resolve), and settings loaded from `.gyroflow` project file

Final Cut Pro X plugin uses `MTLTexture` provided by the FCPX, and settings loaded from `.gyroflow` project file



Optional OpenCV is used in core for one of the optical flow algorithms (DISOpticalFlow) and the main app uses it as the default. If you're just loading `.gyroflow` project file, then the optical flow is not needed, as it's only used to determine the offsets and not for the actual stabilization process.

Main app also uses OpenCV in the lens calibrator.

## Examples

* [Main app and GUI](https://github.com/gyroflow/gyroflow/blob/master/src/controller.rs)
* [The OpenFX plugin ](https://github.com/gyroflow/gyroflow-ofx/blob/main/src/gyroflow.rs)
* [The FCPX plugin](https://github.com/latenitefilms/GyroflowToolbox/blob/main/Source/Frameworks/gyroflow/src/lib.rs)
