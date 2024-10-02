# 💡 General Plugin Workflow

Gyroflow plugin for a video editor allows you to stabilize the video directly inside the video editor, where you work with the original video file, instead of rendering a stabilized video in the app and then importing it to the video editor.

If your camera has official lens-profiles and accurate gyro timing (GoPro 8+, Sony, Insta360, DJI), then you should be able to just apply the plugin to your clip. In the Adobe plugin, it should load the gyro data automatically. In Resolve, you should click "Load for current file" or use the "Browse" button to select your video file or the `.gyroflow` project file.

Since Gyroflow supports a lot of different cameras and gyro sources, it's practically impossible to recreate all tools it offers (for example synchronization with optical flow) inside the plugin UI.

If your camera requires synchronization (RED, RunCam, Hawkeye, phone apps, blackbox, etc.), the workflow starts inside the main Gyroflow app, where you load your video, lens profile, gyro data, you do all synchronization and parameters, but **instead of rendering - you export a project file** which includes all your parameters and gyro data. This can be easily done by **Ctrl + S** shortcut, or using **Export -> Export project file (including gyro data)**.

This exported project file is then loaded inside the Gyroflow plugin in your video editor and the plugin will process your pixels directly inside your editor according to the gyro data and all your parameters, but without any transcoding, recompression or additional processing.

This is especially important when working with RAW files (like BRAW or R3D), where you retain all your RAW controls like ISO, White Balance etc.

### Currently supported video editors:

* [DaVinci Resolve (OpenFX)](davinci-resolve-openfx.md)
* [MAGIX Vegas (OpenFX)](davinci-resolve-openfx.md)
* [The Foundry Nuke (OpenFX)](davinci-resolve-openfx.md)
* [ASSIMILATE Scratch (OpenFX)](davinci-resolve-openfx.md)
* [Adobe Premiere](davinci-resolve-openfx-1.md)
* [Adobe After Effects](davinci-resolve-openfx-1.md)
* [Kdenlive (frei0r)](frei0r.md)
* [Shotcut (frei0r)](frei0r.md)
* [Final Cut Pro X](final-cut-pro-x.md)

