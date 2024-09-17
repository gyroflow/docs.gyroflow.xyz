---
description: >-
  GoPro cameras are supported in Gyroflow and are proven to deliver great video
  and stabilization quality.
---

# GoPro

## Supported models

<table><thead><tr><th width="167">Model</th><th width="117">Gyro data</th><th width="126">Lens profile</th><th width="159">Synchronization</th><th>Remarks</th></tr></thead><tbody><tr><td>Hero 4 or older</td><td>❌</td><td>❌</td><td>❌</td><td></td></tr><tr><td>Hero 5 Black<br>Hero 5 Session</td><td>✅</td><td>✅ Official</td><td>❗Needed</td><td>❌ <a data-footnote-ref href="#user-content-fn-1">Bad on FPV drone</a><br><a data-footnote-ref href="#user-content-fn-2">🚧 Superview</a></td></tr><tr><td>Hero 6 Black</td><td>✅</td><td>✅ Official</td><td>❗Needed</td><td><a href="http://everythingfpv.com/how-to-downgrade-your-gopro-hero6-to-1-6/">Firmware v1.6</a><a data-footnote-ref href="#user-content-fn-3"> recommended.</a><br><a data-footnote-ref href="#user-content-fn-4">🚧 Superview</a></td></tr><tr><td>Hero 7 White<br>Hero 7 Silver</td><td>❌</td><td>❌</td><td>❌</td><td></td></tr><tr><td>Hero 7 Black</td><td>✅</td><td>✅ Official</td><td>❗Needed</td><td>❌ <a data-footnote-ref href="#user-content-fn-5">Bad on FPV drone</a><br><a data-footnote-ref href="#user-content-fn-6">❌ Hypersmooth<br></a><a data-footnote-ref href="#user-content-fn-7">🚧 Superview</a></td></tr><tr><td>Hero 8 Black<br>Hero 9 Black<br>Hero 10 Black</td><td>✅</td><td>✅ Official</td><td>✅ Not needed</td><td><a data-footnote-ref href="#user-content-fn-8">✅ Hypersmooth<br></a><a data-footnote-ref href="#user-content-fn-9">🚧 Superview</a></td></tr><tr><td>Hero 11 Black<br>Hero 11 Mini<br>Hero 12 Black<br>Hero 13 Black</td><td>✅</td><td>✅ Official</td><td>✅ Not needed</td><td><p><a data-footnote-ref href="#user-content-fn-10">✅ Hypersmooth</a></p><p>✅ Autoboost<br><a data-footnote-ref href="#user-content-fn-11">🚧 Superview</a><br><a data-footnote-ref href="#user-content-fn-12">🚧 Hyperview</a></p></td></tr></tbody></table>

## Sensor and aspect ratio

All GoPros until 11 use a 4:3 sensor, and Hero 11 uses 8:7 sensor. For Gyroflow stabilization, it's best to capture full sensor information, thus 4:3 or 8:7. However, these modes have less frame rate options than 16:9 resolutions, so if you want to shoot at maximum fps, it's better to choose 16:9 mode. Otherwise choose 4:3 or 8:7.

Gyroflow will output 16:9 aspect ratio by default after stabilization. This can be changed to anything you like in Export settings.

## Lens and field of view

All lens modes are supported, but it's best to choose `Wide`.

Superview and Hyperview modes are supported, but not recommended. They capture full sensor information (4:3 and 8:7 respectively) and digitally stretch that video to 16:9 in camera, in a non-linear way (center is less stretched and sides are more stretched).

These modes use proprietary stretching equation which is not exactly mapped in Gyroflow, so stabilization will not be as accurate in these modes.

## Split recording

By default, GoPro files are limited to 4 GB in size. This can be increased by using [GoPro Labs](https://gopro.github.io/labs/) firmware and enabling the [Large chapters](https://gopro.github.io/labs/control/chapters/) feature.

If you already have your recording split at 4 GB file size limit, you can merge them losslessly in Gyroflow. If you open the first file, Gyroflow should detect the sequence automatically and ask you to merge. Otherwise you can just drag & drop all your files in the sequence to Gyroflow to merge.

You can find more information on the [**🎞 File joiner**](../file-joiner.md) page.

## In-camera stabilization

Since Hero 8, in-camera stabilization (Hypersmooth) can be enabled and is supported in Gyroflow.\
All previous models need to have all internal stabilization (EIS) disabled during recording.

## Shutter speed, ND filters and motion blur

You should avoid motion blur when recording, read more why in the [📸 **Common filming tips and issues**](../common-filming-tips-and-issues.md).

## Hero 5 Black, Hero 5 Session, Hero 7 on an FPV drone

For drone footage, the gyro data from the Hero 5 and 7 is unusable, due to bad noise filtering and aliasing. For this reason, Hero 5 Session, Hero 5 Black and Hero 7 **are not recommended** for cinematic FPV footage. One workaround is to use external gyro logging, for instance using the drone blackbox. Another workaround is the use of vibration-dampening mount, which can be a hassle compared to other GoPros.

Some users have reported that using intense low pass filtering of the gyro data (e.g. cutoff of 4 Hz or less) can yield acceptable results for some drone footage. This cannot correct for e.g. higher frequency propwash oscillations or vibrations, but can smooth out the general movement.

In general try to avoid these cameras if possible. If you want a budget-friendly camera, the **Hero 6 Black** is an great choice and doesn't have these issues.

## Using GoPro as external logger

{% content-ref url="../../advanced-usage/using-external-gyro-source/action-camera-as-a-logger.md" %}
[action-camera-as-a-logger.md](../../advanced-usage/using-external-gyro-source/action-camera-as-a-logger.md)
{% endcontent-ref %}

[^1]: Usage on FPV drone is not recommended because of motor vibrations which messes up the gyro data.\
    Handheld footage should work fine

[^2]: Superview mode is supported, but it's not 100% accurate. It may work for your case but is generally not recommended

[^3]: Hero 6 is a well supported and reliable model

[^4]: Superview mode is supported, but it's not 100% accurate. It may work for your case but is generally not recommended

[^5]: Usage on FPV drone is not recommended because of motor vibrations which messes up the gyro data.\
    Handheld footage should work fine

[^6]: Hypersmooth mode is not supported, ie. all in-camera stabilization needs to be turned **OFF**

[^7]: Superview mode is supported, but it's not 100% accurate. It may work for your case but is generally not recommended

[^8]: Hypersmooth mode is supported.\
    This means you can record with in-camera stabilization turned on and then stabilize further in Gyroflow

[^9]: Superview mode is supported, but it's not 100% accurate. It may work for your case but is generally not recommended

[^10]: Hypersmooth mode is supported.\
    This means you can record with in-camera stabilization turned on and then stabilize further in Gyroflow

[^11]: Superview mode is supported, but it's not 100% accurate. It may work for your case but is generally not recommended

[^12]: Hyperview mode is supported, but it's not 100% accurate. It may work for your case but is generally not recommended
