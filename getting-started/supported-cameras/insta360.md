---
description: Most Insta360 action cameras are supported.
---

# Insta360

Insta360 has two types of cameras: action cameras and 360° cameras. Action cameras are supported out of the box in Gyroflow, but 360° cameras [are not](../../advanced-usage/360-cameras.md).

## Supported models

<table><thead><tr><th width="197">Model</th><th width="83">Type</th><th width="174">Support</th><th width="158">Synchronization</th></tr></thead><tbody><tr><td>Insta360 GO</td><td>Action</td><td>✅ Supported</td><td>❗Needed</td></tr><tr><td>Insta360 GO 2<br>Caddx Peanut</td><td>Action</td><td>✅ Supported</td><td>✅ Not needed</td></tr><tr><td>Insta360 GO 3<br>Insta360 GO 3S<br>Insta360 GO Ultra</td><td>Action</td><td>✅ Supported</td><td>✅ Not needed</td></tr><tr><td>Insta360 ONE R<br>Insta360 SMO 4k</td><td>Action</td><td>✅ Supported</td><td>✅ Not needed</td></tr><tr><td>Insta360 ONE RS</td><td>Action</td><td>✅ Supported</td><td>✅ Not needed</td></tr><tr><td>Insta360 Ace<br>Insta360 Ace Pro</td><td>Action</td><td>✅ Supported</td><td>✅ Not needed</td></tr><tr><td>Insta360 ONE RS 1-Inch 360</td><td>360°</td><td>❌ Not supported</td><td>❌</td></tr><tr><td>Insta360 ONE X2</td><td>360°</td><td>❌ Not supported</td><td>❌</td></tr><tr><td>Insta360 X3</td><td>360°</td><td>❌ Not supported</td><td>❌</td></tr><tr><td>Insta360 X5<br>Insta360 Antigravity A1</td><td>360°</td><td><mark style="color:yellow;">⚠️</mark> <a href="insta360.md#motion-data-only">Motion data only</a></td><td>✅ Not needed</td></tr></tbody></table>

Most supported cameras have official lens profiles built-in, except for the older _Insta360 Go_

## Motion data only

Since Gyroflow 1.7.0, Gyroflow reads the motion data of the Insta360 X5 and Antigravity A1, but it can't play their **.insv** files. Open a playable copy of the clip (e.g. a transcode or a proxy) as the video, and then load the original **.insv** file in **Motion data -> Open file**.

The **.lrv** proxy files recorded by the X series contain the same motion data as the **.insv** file, and Gyroflow can open them as well.

Stabilizing stitched 360° video is still not supported, see [🥽 360° cameras](../../advanced-usage/360-cameras.md).

Some of the cameras require use of FreeFrame video mode, for motion data to be saved. 

## Shutter speed, ND filters and motion blur

You should avoid motion blur when recording, read more why in the [📸 **Common filming tips and issues**](../common-filming-tips-and-issues.md).
