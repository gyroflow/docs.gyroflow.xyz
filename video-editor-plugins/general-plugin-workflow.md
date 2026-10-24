# 💡 General Plugin Workflow

Gyroflow plugin for a video editor allows you to stabilize the video directly inside the video editor, where you work with the original video file, instead of rendering a stabilized video in the app and then importing it to the video editor.

If your camera has official lens-profiles and accurate gyro timing (GoPro 8+, Sony, Insta360, DJI, Z CAM), then you should be able to just apply the plugin to your clip. In the Adobe plugin, it should load the gyro data automatically. In Resolve, you should click "Load for current file" or use the "Browse" button to select your video file or the `.gyroflow` project file.

Since Gyroflow supports a lot of different cameras and gyro sources, it's practically impossible to recreate all tools it offers (for example synchronization with optical flow) inside the plugin UI.

If your camera requires synchronization (RunCam, Hawkeye, phone apps, blackbox, etc.) or a lens profile you choose yourself (e.g. Blackmagic or RED), the workflow starts inside the main Gyroflow app, where you load your video, lens profile, gyro data, you do all synchronization and parameters, but **instead of rendering - you export a project file** which includes all your parameters and gyro data. This can be easily done by **Ctrl + S** shortcut, or using **Export -> Export project file (including gyro data)**.

This exported project file is then loaded inside the Gyroflow plugin in your video editor and the plugin will process your pixels directly inside your editor according to the gyro data and all your parameters, but without any transcoding, recompression or additional processing.

This is especially important when working with RAW files (like BRAW or R3D), where you retain all your RAW controls like ISO, White Balance etc.

The plugins use the same stabilization engine as the app, so the improvements in Gyroflow 1.7.0 (Sony support, lens breathing compensation and the new lens math) apply in the plugins as well. The **Output projection** and the **Static zoom** zoom level from projects and presets made in the app are used too.

### Installation

The easiest way to install or update the OpenFX and Adobe plugins is the **Video editor plugins** panel in the Gyroflow app, at the bottom of the right side (on Windows and macOS, when a supported video editor is installed). Manual installation is described on the page of each plugin.

### Default settings

The plugin has a **Save as default** button. It saves the current smoothness, zoom limit, lens correction, horizon lock and FOV as the defaults for every newly loaded clip, so you can change e.g. the default 130% zoom limit.

### Vertical video

Vertical footage works correctly on a vertical timeline, when the footage and the timeline have the same aspect ratio.

### Anamorphic lenses

If your lens profile uses **Input stretch** (e.g. for an anamorphic lens) and your video editor already does the desqueeze, enable **Disable lens stretch (for plugins)** when creating a preset in the app, so the plugin doesn't stretch the video again. See [⚙ Settings presets](../advanced-usage/settings-presets.md).

### Currently supported video editors:

* [DaVinci Resolve (OpenFX)](davinci-resolve-openfx.md)
* [MAGIX Vegas (OpenFX)](davinci-resolve-openfx.md)
* [The Foundry Nuke (OpenFX)](davinci-resolve-openfx.md)
* [ASSIMILATE Scratch (OpenFX)](davinci-resolve-openfx.md)
* [Adobe Premiere](adobe-premiere-after-effects.md)
* [Adobe After Effects](adobe-premiere-after-effects.md)
* [Kdenlive (frei0r)](frei0r.md)
* [Shotcut (frei0r)](frei0r.md#shotcut)
* [Final Cut Pro X](final-cut-pro-x.md)

