---
description: RunCam cameras save the gyro data in the .gcsv format next to the video files
---

# RunCam

{% hint style="warning" %}
**Important!** Make sure you have the latest firmware installed in your camera.

You can download it from [here](https://www.runcam.com/download/actioncameras).
{% endhint %}

## Supported models

<table><thead><tr><th width="167">Model</th><th width="111">Gyro data</th><th width="126">Lens profile</th><th width="159">Synchronization</th><th>Quality</th></tr></thead><tbody><tr><td>5 Orange</td><td>✅ </td><td>✅ Official</td><td>❗Needed</td><td>✅ Decent</td></tr><tr><td>Thumb</td><td>✅ </td><td>✅ Official</td><td>❗Needed</td><td>❌ Bad</td></tr><tr><td>Thumb Pro</td><td>✅ </td><td>✅ Official</td><td>❗Needed</td><td>✅ Decent</td></tr><tr><td>Thumb Pro-W</td><td>✅ </td><td>✅ Official</td><td>❗Needed</td><td>✅ Decent</td></tr></tbody></table>

## Caveats

While RunCam cameras save the gyro information, it can be very tricky to stabilize the footage accurately, especially on an FPV drone.

RunCam cameras have an issue with duplicated or dropped frames, and this causes synchronization issues.

Moreover because they are very lightweight, they are prone to receive a lot of vibrations from the motors. You may have to try different mounts to get acceptable results.

## Synchronization

It may be difficult to correctly sync the RunCam cameras. By default Gyroflow uses a special sync points pattern to try to sync the video when the frames are duplicated or dropped, thus giving you the best chance of correct synchronization.&#x20;

With some videos this may not work by default so you'll need to adjust these sync points, deleting some and adding new ones in different places. Make sure to learn how to use Gyroflow by reading the [🔧 **Basic usage**](../basic-usage/), [📈**Timeline and gyro chart**](../basic-usage/timeline-and-gyro-chart.md) and [⌛ **Synchronization**](runcam.md#synchronization) sections.

Correct synchronization of RunCam cameras can be a pain sometimes so if you want to save yourself trouble and frustration, avoid RunCam cameras and get something else instead.

{% hint style="info" %}
Ignore sync point colors with RunCam cameras, some will be red and some will be green, but because of the duplicated/dropped frames, the color indicator is useless. Make sure your video is stabilized by playing it.
{% endhint %}

## Recommended camera settings

* Video quality: High
* Resolution: Either 4K@30FPS(XV) or 1440p@60fps.
  * The XV resolution is actually the full 4:3 sensor stretched to a 16:9 image. This gives the highest resolution and FOV.
  * Note that the stretching itself looks bad, so if used without additional processing, linearly squeeze the width to 75% in the video editor to get a 4:3 image.
  * The 1440p option is natively 4:3, so if you want 60fps use this one.
  * You can also use one of the 16:9 resolutions, but this significantly reduces the available FOV for stabilization.
* Shutter: Aim for 45° to 90° shutter. For 30 fps it will be 1/240 to 1/120 and for 60 fps - 1/480 to 1/240.
* ISO: As low as possible. There seems to be some auto gain regardless of settings.
* Color style: Normal (Flat squeezes the range and doesn’t actually provide more dynamic range)
* Distortion correction: Disabled
* Electronic Image Stabilization: Disabled
* General settings (there were just found to work decently decently well, but do some testing yourself)
  * Saturation: Medium
  * Exposure compensation: 0
  * Contrast: Low
  * Sharpness: Low
  * Metering: Average
  * White balance: Sunny (change depending on scenario, _don’t_ use auto)
  * Low light enhancement: you decide.
