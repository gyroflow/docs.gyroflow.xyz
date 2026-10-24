---
description: Newer Canon mirrorless cameras are supported out of the box.
---

# Canon

## Supported models

<table><thead><tr><th width="289">Model</th><th width="174">Gyro data</th><th width="320">Lens profile</th></tr></thead><tbody><tr><td>EOS C50<br>EOS C80<br>EOS C400</td><td>✅</td><td>✅ Automatic <sup><sub>(see</sub></sup> <a href="canon.md#supported-lenses-for-automatic-lens-profile"><sup><sub>Supported Lenses</sub></sup></a><sup><sub>)</sub></sup></td></tr><tr><td>EOS R6 Mark II<br>EOS R6 Mark III</td><td>❌<br>✅</td><td>❌<br>✅ Automatic <sup><sub>(see</sub></sup> <a href="canon.md#supported-lenses-for-automatic-lens-profile"><sup><sub>Supported Lenses</sub></sup></a><sup><sub>)</sub></sup></td></tr><tr><td>EOS R6 V</td><td>✅</td><td>✅ Automatic <sup><sub>(see</sub></sup> <a href="canon.md#supported-lenses-for-automatic-lens-profile"><sup><sub>Supported Lenses</sub></sup></a><sup><sub>)</sub></sup></td></tr><tr><td>EOS R5<br>EOS R5 Mark II</td><td>❌<br>✅</td><td>❌<br>✅ Automatic <sup><sub>(see</sub></sup> <a href="canon.md#supported-lenses-for-automatic-lens-profile"><sup><sub>Supported Lenses</sub></sup></a><sup><sub>)</sub></sup></td></tr></tbody></table>

That list might not be exhaustive as Canon will probably add the motion data to all their new cameras.&#x20;

With a supported lens, Gyroflow reads the motion data and the focal length of every frame from the clip, so no lens profile is needed and zooms are followed.

## Supported formats

* **MP4** and **MXF** files play in Gyroflow, and the motion data is read from the same file.
* **Cinema RAW Light** (.crm) - Gyroflow can't play these files, but it can read their motion data. Open a playable copy of the clip (e.g. the proxy recorded by the camera, or a transcode) as the video, and then load the original **.crm** file in **Motion data -> Open file**.

## Supported modes

In order to stabilize the videos correctly in Gyroflow, all internal stabilization in camera **must be disabled.** This includes Lens IS, Digital IS and IBIS.

At this point, Canon's gyro data timing is not always 100% accurate. You may have to do an [Auto Sync](../basic-usage/synchronization.md) on some clips.

## Supported Lenses for automatic lens profile

The list of supported lenses which work automatically in Gyroflow:

#### RF Lenses

* RF 20mm F1.4L VCM
* RF 24mm F1.4L VCM&#x20;
* RF 35mm F1.4L VCM&#x20;
* RF 50mm F1.4L VCM&#x20;
* RF 85mm F1.4L VCM
* RF 24-105mm F2.8L IS USM Z <sup>(RF 1.4x or 2x Extender are also compatible)</sup>
* RF 70-200mm F2.8L IS USM Z <sup>(RF 1.4x or 2x Extender are also compatible)</sup>

#### RF Cinema Lenses

* RF CN7x17 KAS T/R1
* RF CN5x11 IAS T/R1
* RF CN-R 14mm T3.1L F
* RF CN-R 20mm T1.5L F
* RF CN-R 24mm T1.5L F
* RF CN-R 35mm T1.5L F
* RF CN-R 50mm T1.3L F
* RF CN-R 85mm T1.3L F
* RF CN-R 135mm T2.2L F

#### EF Mount: (Requires an EF to EOS R Mount Adapter)

* EF CN8x15 IAS S/E1
* EF CN-E 20-50mm T2.4L F
* EF CN-E 45-135mm T2.4L F
* EF CN-E 14-35mm T1.7L F
* EF CN-E 31.5-95mm T1.7L F

{% hint style="warning" %}
Make sure your camera and the lens are updated to latest firmware version.
{% endhint %}

## Zoom lenses

Zoom lenses are supported automatically, and you can zoom in and out throughout the video, if your lens is on the list provided above.

If you want to smooth out fast zooms, enable **Stabilize focal length** in the Stabilization panel. Learn more in [🔭 **Advanced usage -> Lens profiles**](../../advanced-usage/lens-profiles.md#zoom-lenses).

## My lens is not on that list

If your lens is not supported, you have to provide a lens profile manually. You can either find it in the list of community-provided lens profiles, or calibrate yourself by following [🏁this guide](../lens-calibration.md).

## Shutter speed, ND filters and motion blur

You should avoid motion blur when recording, read more why in the [📸 **Common filming tips and issues**](../common-filming-tips-and-issues.md).
