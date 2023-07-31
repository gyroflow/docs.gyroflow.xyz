---
description: Newer Sony mirrorless cameras are supported out of the box.
---

# Sony

Most newer Sony mirrorless cameras record the motion data internally.

## Supported models

<table><thead><tr><th width="289">Model</th><th width="174">Gyro data</th><th width="320">Lens profile</th></tr></thead><tbody><tr><td>Sony α1</td><td>✅</td><td><mark style="color:yellow;">⚠️</mark> By users</td></tr><tr><td>Sony α7 IV</td><td>✅</td><td><mark style="color:yellow;">⚠️</mark> By users</td></tr><tr><td>Sony α7C</td><td>✅</td><td><mark style="color:yellow;">⚠️</mark> By users</td></tr><tr><td>Sony α7R IV<br>Sony α7R V</td><td>❌<br>✅</td><td>❌<br><mark style="color:yellow;">⚠️</mark> By users</td></tr><tr><td>Sony α7S III</td><td>✅</td><td><mark style="color:yellow;">⚠️</mark> By users</td></tr><tr><td>Sony α9 II</td><td>✅</td><td><mark style="color:yellow;">⚠️</mark> By users</td></tr><tr><td>Sony FX3<br>Sony FX6<br>Sony FX9<br>Sony FX30</td><td>✅</td><td><mark style="color:yellow;">⚠️</mark> By users</td></tr><tr><td>Sony RX0<br>Sony RX0 II</td><td>❌<br>✅</td><td>❌<br><mark style="color:yellow;">⚠️</mark> By users</td></tr><tr><td>Sony RX100 VI<br>Sony RX100 VII</td><td>❌<br>✅</td><td>❌<br><mark style="color:yellow;">⚠️</mark> By users</td></tr><tr><td>Sony ZV-1<br>Sony ZV-E1<br>Sony ZV-E10</td><td>✅</td><td><mark style="color:yellow;">⚠️</mark> By users</td></tr><tr><td>Sony α6500<br>Sony α6600<br>Sony a6700</td><td>❌<br>❌<br>✅</td><td>❌<br>❌<br><mark style="color:yellow;">⚠️</mark> By users</td></tr></tbody></table>

That list might not be exhaustive as Sony will probably add the motion data to all their new cameras. In general if the stabilization is available in Sony's _Catalyst Browse_ software, Gyroflow should support it too.

## Supported modes

Some recording modes will not store the motion data in files, for example Sony α7S III in 4k/120fps mode doesn't include the motion data.&#x20;

Currently Gyroflow supports reading only gyroscope and accelerometer data, so in order to stabilize the footage in Gyroflow, all internal stabilization in camera needs to be **disabled**. Make sure to disable Lens OSS, IBIS and Active stabilization (EIS).

However, there is a lot more information in that metadata, including lens distortions, IBIS, OIS and EIS movements. Sony's own _Catalyst Browse_ software, can use that metadata to stabilize videos recorded with internal stabilizations enabled.&#x20;

In theory, Gyroflow could support all these modes, but that metadata is in a proprietary format and so far we were unable to reverse-engineer and understand these values.

Help in this area is very much needed, more details are available [here](https://github.com/gyroflow/gyroflow/issues/44).

## Zoom lenses

Sony provides a focal length information per each frame of the video and we can use that to support zoom lenses, i.e. zoom position can be changing throughout the video.

Creating such lens profile requires a bit of manual work. For more details, see [here](../../advanced-usage/lens-profiles.md).

If you don't want to create that special zoom lens profile, you can just calibrate your lens with a static focal length (eg. 24mm on a 24-70mm zoom lens), and then don't change the zoom value when recording the video.

## Shutter speed, ND filters and motion blur

You should avoid motion blur when recording, read more why in the [📸 **Common filming tips and issues**](../common-filming-tips-and-issues.md).
