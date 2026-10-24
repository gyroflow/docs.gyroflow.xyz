---
description: Z CAM Mark II cameras record motion data and lens metadata internally.
---

# Z CAM

Since Gyroflow 1.7.0, Z CAM Mark II cameras are supported. They record the motion data, the lens focal length and the frame readout time in the video file.

## Supported models

<table><thead><tr><th width="208">Model</th><th width="110">Gyro data</th><th width="160">Lens profile</th><th width="166">Synchronization</th></tr></thead><tbody><tr><td>E2-M4 Mark II<br>E2-S6 Mark II<br>E2-F6 Mark II<br>E2-F8 Mark II<br>E2-M4G Mark II<br>E2-S6G Mark II<br>E2N Mark II</td><td>✅</td><td><mark style="color:yellow;">⚠️</mark> <a href="zcam.md#lens-profile">From the focal length</a></td><td>✅ Not needed</td></tr></tbody></table>

## Supported formats

* **MP4** and **MOV** files play in Gyroflow, and the motion data is read from the same file.
* **ZRAW** (.zraw) - Gyroflow can't play these files, but it can read their motion data. Open a playable copy of the clip (e.g. a transcode or a proxy) as the video, and then load the original **.zraw** file in **Motion data -> Open file**.

## Lens profile

Gyroflow creates the lens profile from the focal length and the sensor area recorded by the camera. That lens profile has no lens distortion, so it works well for rectilinear lenses with little distortion. For wide or fisheye lenses, load a calibrated lens profile instead, or calibrate your lens by following [🏁 this guide](../lens-calibration.md).

If your lens doesn't report its focal length to the camera (e.g. a manual lens), you also have to provide a lens profile.

## Zoom lenses

The camera records the focal length of every frame, so Gyroflow follows the zoom automatically. If you want to smooth out fast zooms, enable **Stabilize focal length** in the Stabilization panel. Learn more in [🔭 **Advanced usage -> Lens profiles**](../../advanced-usage/lens-profiles.md#zoom-lenses).

## Rolling shutter correction

The frame readout time is read from the file, so you don't need to set it.

## Shutter speed, ND filters and motion blur

You should avoid motion blur when recording, read more why in the [📸 **Common filming tips and issues**](../common-filming-tips-and-issues.md).
