---
description: Newer Sony mirrorless cameras are supported out of the box.
---

# Sony

Most newer Sony mirrorless cameras record the motion data internally.

## Supported models

<table><thead><tr><th width="289">Model</th><th width="174">Gyro data</th><th width="320">Lens profile</th></tr></thead><tbody><tr><td>Sony α1</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony α7 IV</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony α7C<br>Sony α7C II<br>Sony α7CR</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony α7R IV<br>Sony α7R V</td><td>❌<br>✅</td><td>❌<br>✅ Automatic</td></tr><tr><td>Sony α7S III</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony α9 II<br>Sony α9 III</td><td>❌<br>✅</td><td>❌<br>✅ Automatic</td></tr><tr><td>Sony FX3<br>Sony FX6<br>Sony FX9<br>Sony FX30</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony RX0<br>Sony RX0 II</td><td>❌<br>✅</td><td>❌<br>✅ Automatic</td></tr><tr><td>Sony RX100 VI<br>Sony RX100 VII</td><td>❌<br>✅</td><td>❌<br>✅ Automatic</td></tr><tr><td>Sony ZV-1<br>Sony ZV-1 II<br>Sony ZV-1F<br>Sony ZV-E1<br>Sony ZV-E10<br>Sony ZV-E10 II</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony α6500<br>Sony α6600<br>Sony a6700</td><td>❌<br>❌<br>✅</td><td>❌<br>❌<br>✅ Automatic</td></tr><tr><td>Sony HXR-NX800<br>Sony PXW-Z200</td><td>✅<br>❓</td><td>✅ Automatic<br>❓Probable, not confirmed</td></tr></tbody></table>

That list might not be exhaustive as Sony will probably add the motion data to all their new cameras. In general if the stabilization is available in Sony's _Catalyst Browse_ software, Gyroflow should support it too.

## Supported modes

Some recording modes will not store the motion data in files, for example Sony α7S III in 4k/120fps mode doesn't include the motion data.

Gyroflow reads all of the stabilization metadata recorded by the camera, which includes IBIS and OIS data. Therefore, you **can record with IBIS and OIS enabled.** The **Active** and **Dynamic Active** modes are supported as well (Dynamic Active since Gyroflow 1.7.0).

The best option is to use **Standard** stabilization in camera (if your model has IBIS) and add Gyroflow on top of that, because Active and Dynamic Active also crop the image in camera.

All known issues with Sony metadata are fixed since Gyroflow 1.7.0. If your file still doesn't work properly, but does work in Catalyst Browse, submit it in [this GitHub issue](https://github.com/gyroflow/gyroflow/issues/849).

### Supported OSS Lenses

The list of supported lenses with OSS enabled is available on the [Sony website](https://support.d-imaging.sony.co.jp/www/cscs/function/compatibility.php?fnc=1002\&area=gb\&lang=en\&ref=1). If the link ever stops working, here's the current list as of Oct 2025:

<figure><img src="../../.gitbook/assets/sony lenses.jpg" alt=""><figcaption></figcaption></figure>

## Zoom lenses

Zoom lenses are supported automatically, and you can zoom in and out throughout the video.

* If you want to smooth out fast zooms, enable **Stabilize focal length** in the Stabilization panel.
* Some lenses report the focal length a few frames late (e.g. some Tamron zooms). If the picture jumps or wobbles during zooms, set **Lens metadata delay** in `Lens profile -> Advanced`, or click **Analyze** next to it to measure it from the zooms in the clip.

Learn more in [🔭 **Advanced usage -> Lens profiles**](../../advanced-usage/lens-profiles.md#zoom-lenses).

## Lens breathing compensation

Many lenses change the field of view slightly when focusing (this is called focus breathing). Since Gyroflow 1.7.0, Gyroflow can keep the field of view constant while focusing, using the lens breathing data recorded by the camera. That's the case for supported Sony lenses on cameras like the α7 IV, α7C II, α7R V, α6700 and ZV-E1.

When the file contains this data, **Lens breathing compensation** shows up in the Lens profile panel, and it's enabled by default.

## Shutter speed, ND filters and motion blur

You should avoid motion blur when recording, read more why in the [📸 **Common filming tips and issues**](../common-filming-tips-and-issues.md).
