---
description: Newer Sony mirrorless cameras are supported out of the box.
---

# Sony

Most newer Sony mirrorless cameras record the motion data internally.

## Supported models

| Model                                                | Gyro data     | Lens profile                                               | Synchronization     |
| ---------------------------------------------------- | ------------- | ---------------------------------------------------------- | ------------------- |
| Sony α1                                              | ✅             | <mark style="color:yellow;">⚠️</mark> By users             | ❗Needed             |
| Sony α7 IV                                           | ✅             | <mark style="color:yellow;">⚠️</mark> By users             | ❗Needed             |
| Sony α7C                                             | ✅             | <mark style="color:yellow;">⚠️</mark> By users             | ❗Needed             |
| <p>Sony α7R IV<br>Sony α7R V</p>                     | ✅             | <mark style="color:yellow;">⚠️</mark> By users             | ❗Needed             |
| Sony α7S III                                         | ✅             | <mark style="color:yellow;">⚠️</mark> By users             | ❗Needed             |
| Sony α9 II                                           | ✅             | <mark style="color:yellow;">⚠️</mark> By users             | ❗Needed             |
| <p>Sony FX3<br>Sony FX6<br>Sony FX9<br>Sony FX30</p> | ✅             | <mark style="color:yellow;">⚠️</mark> By users             | ❗Needed             |
| <p>Sony RX0<br>Sony RX0 II</p>                       | <p>❌<br>✅</p> | <p>❌<br><mark style="color:yellow;">⚠️</mark> By users</p> | <p>❌<br>❗Needed</p> |
| <p>Sony RX100 VI<br>Sony RX100 VII</p>               | <p>❌<br>✅</p> | <p>❌<br><mark style="color:yellow;">⚠️</mark> By users</p> | <p>❌<br>❗Needed</p> |
| Sony ZV-1                                            | ✅             | <mark style="color:yellow;">⚠️</mark> By users             | ❗Needed             |
| Sony ZV-E10                                          | ✅             | <mark style="color:yellow;">⚠️</mark> By users             | ❗Needed             |
| Sony α6xxx                                           | ❌             | ❌                                                          | ❌                   |

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
