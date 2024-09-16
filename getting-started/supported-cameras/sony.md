---
description: Newer Sony mirrorless cameras are supported out of the box.
---

# Sony

Most newer Sony mirrorless cameras record the motion data internally.

## Supported models

<table><thead><tr><th width="289">Model</th><th width="174">Gyro data</th><th width="320">Lens profile</th></tr></thead><tbody><tr><td>Sony α1</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony α7 IV</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony α7C<br>Sony α7C II<br>Sony α7CR</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony α7R IV<br>Sony α7R V</td><td>❌<br>✅</td><td>❌<br>✅ Automatic</td></tr><tr><td>Sony α7S III</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony α9 II</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony FX3<br>Sony FX6<br>Sony FX9<br>Sony FX30</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony RX0<br>Sony RX0 II</td><td>❌<br>✅</td><td>❌<br>✅ Automatic</td></tr><tr><td>Sony RX100 VI<br>Sony RX100 VII</td><td>❌<br>✅</td><td>❌<br>✅ Automatic</td></tr><tr><td>Sony ZV-1<br>Sony ZV-1 II<br>Sony ZV-1F<br>Sony ZV-E1<br>Sony ZV-E10<br>Sony ZV-E10 II</td><td>✅</td><td>✅ Automatic</td></tr><tr><td>Sony α6500<br>Sony α6600<br>Sony a6700</td><td>❌<br>❌<br>✅</td><td>❌<br>❌<br>✅ Automatic</td></tr></tbody></table>

That list might not be exhaustive as Sony will probably add the motion data to all their new cameras. In general if the stabilization is available in Sony's _Catalyst Browse_ software, Gyroflow should support it too.

## Supported modes

Some recording modes will not store the motion data in files, for example Sony α7S III in 4k/120fps mode doesn't include the motion data.&#x20;

Currently Gyroflow supports reading most of the recorded metadata, which includes IBIS and OIS data. Therefore, you **can record with IBIS and OIS enabled.** Most newer or high-end lenses with OIS are supported, but there are some lenses where enabling OIS prevents proper stabilization, so check your setup first before enabling OIS. **Dynamic Active** mode is also not supported, but the regular **Active** is.

The best option is to use **Standard** stabilization in camera (if your model has IBIS) and add Gyroflow on top of that.

List of current issues with Sony metadata is [here](https://github.com/gyroflow/gyroflow/issues/849). If your file doesn't work properly, but does work in Catalyst Browse, submit it in that [GitHub issue](https://github.com/gyroflow/gyroflow/issues/849).

## Zoom lenses

Zoom lenses are supported automatically, and you can zoom in and out throughout the video.

## Shutter speed, ND filters and motion blur

You should avoid motion blur when recording, read more why in the [📸 **Common filming tips and issues**](../common-filming-tips-and-issues.md).
