---
description: >-
  In the Stabilization tab, you can control all parameters related to the
  stabilization and zooming.
---

# 🔥 Stabilization

## FOV

Field Of View - this value can be though of as a zoom slider. For most use cases, it should stay at 1.0. With Dynamic Zooming enabled, the value of 1.0 will ensure that the stabilization uses the maximum possible pixels while not showing any black bars/sides.

If you increase it over 1, you'll zoom out the video and show the outer frame, you can use that to see what stabilization is doing.

If you decrease it below 1, you'll zoom in the footage.

## Smoothing algorithm

Gyroflow ships with 3 smoothing algorithms, but for 99.5% use cases the **Default** algorithm should be used.&#x20;

There are two common issues related to the selected algorithm:

1. Video turns black when stabilization is enabled - most likely you have **Fixed camera** selected. Change to **Default**.
2. Video zooms in too much - most likely you have **Plain 3D** selected. Change to **Default**.

You can learn more about other algorithms and when they are useful in the [**🔥Advanced usage -> Stabilization**](../../advanced-usage/stabilization.md) section.

## Smoothness amount

This is the main slider to control amount of camera movement smoothing. Setting it to 0 will essentially disable any smoothing and setting it to 1 or more, will make your footage more smooth.

Remember that more smoothing requires more zooming, so you have to find a good balance between smoothness and zooming. The default of **0.5** works well for most cases.

## Horizon Lock

You can activate the horizon locking function here. The resulting video will keep the horizon level, while stabilizing other axes. This will cause the video to zoom in more, because Gyroflow will have less pixels to work with when the frame is rotated to keep the horizon level.&#x20;

The effectiveness of this function is depending on the camera motion data. Some cameras have accurate orientation information, and some have worse. It will also depend on the selected **Integration method**, so if you're not getting good results, you can try to change the **Integration method** to something else. Note that in some cases, accurate horizon lock will not work, due to not enough, or bad data provided by the camera.

In some cases, you will be able to adjust the roll value using **Horizon lock roll** slider to keep your horizon levelled, and if you're still not getting good results, you can also [**🔑 keyframe**](../../advanced-usage/keyframes.md) the roll adjustment over time.

Some cameras also contain gravity vectors, which is additional data useful to lock the horizon. In that case there will be a checkbox to **Use gravity vectors**. Note that this will not always be better option, so you have to test your case.

Some people like to keyframe the horizon locking amount to only apply to some parts of the video, so you can use keyframes to do that too. Read more about keyframes in the [🔑 **Advanced -> Keyframes**](../../advanced-usage/keyframes.md) section.

## Zooming

With post-stabilization, we only have as much information as the sensor captured. If we move these pixels around, we'll end up missing some information on the sides. Because of that, applying the stabilization only will result in moving empty sides around the video.

To compensate for that, we need to zoom in the image to show the available pixels only. This is also referred to as cropping.

Gyroflow has 3 available zooming modes:

1. **No zooming** - This mode simply disables all zooming and applies only the stabilization.
2. **Dynamic zooming** - In this mode, the zoom amount will be dynamically changing throughout the video to use as much pixels as we have available. Some parts of the video will need to be zoomed in more and some less, and this mode creates smooth transition between these zoom levels.
3. **Static zoom** - this mode will apply one, static crop for the entire duration of the video. This will remove the zooming in-and-out effect, but it will use the maximum needed zoom level, thus cropping the video a lot. It may work in some cases if there is not much movement, but it's generally recommended the use the _Dynamic zooming_ mode instead (and if you want to avoid the visible zooming in-and-out effect, use high zooming speed, like 20-30s).

### Zooming speed

This is the amount of time it takes to transition from one zoom level to another, meaning the the shorter the time, the faster the zooming will be. You can see that by setting this value to 0.1s.&#x20;

The usual values are ranging from 2s to 5s. Setting longer value will make your zooming changes throughout the video slower, but that also means it will stay zoomed in for longer period of time. Adjust this value to your taste.

You can also switch the zooming method to **Envelope follower** to use a different zooming algorithm. It may look more natural, but you'll have to test for yourself. You can see how the zooming level changes on the chart by observing the zooming level graph (light overlay as described in the [📈 **Timeline and gyro chart**](timeline-and-gyro-chart.md) section).

Read more about advanced zooming options in [**🔥Advanced -> Stabilization**](../../advanced-usage/stabilization.md).

## Rolling shutter correction

Rolling shutter correction is a very important part of accurate stabilization process. Most digital cameras have rolling shutter sensors, meaning that the image is not captured all at once, but it is read sequentially from top to bottom. This makes the captured video prone to skews and wobbles when the side-to-side motion is significant. You can observe this effect by quickly panning left-right with your camera and looking at straight lines - they will be bent. You can learn more about rolling shutter \[here] (TODO: add link)

To compensate for that, Gyroflow needs to know the **Frame readout time** which is the time it took the sensor to capture the entire image. Some cameras have this value built-in, and some cameras have rolling shutter already corrected in the camera (e.g. GoPro). There are also cameras with global shutter which don't create this effect, but they are rare (one example is RED KOMODO).

All other cameras need to have this option enabled and the **Frame readout time** correctly filled in.

#### Example of rolling shutter distortions

TODO: video

#### How to determine the frame readout time?

It's best to determine the value when doing lens calibration and save that value with the lens profile. You can read how to do that on the [🏁 **Lens Calibration**](../lens-calibration.md) page.

Otherwise, you can google the camera lab test, which usually have the frame readout time tested and provided. It is sometimes (but rarely) also provided by the camera manufacturer.

You can also try to simply eyeball it. Due to it's nature, the value can only have limited range, so you can move the slider from 0 to max and see if your footage looks better. It's best to have the footage synchronized already (more or less) to observe just the rolling shutter correction.

{% hint style="warning" %}
Frame readout time affects synchronization, so make sure you synchronize your footage again if you change this value.
{% endhint %}

Frame readout time will also be different depending on the camera shooting mode (e.g. in 4k vs 1080p).

{% hint style="info" %}
If you're using GoPro, Insta360, DJI or BRAW, then you don't have to worry about it as it's handled automatically.
{% endhint %}

### For more control over stabilization settings, see the Advanced section:

{% content-ref url="../../advanced-usage/stabilization.md" %}
[stabilization.md](../../advanced-usage/stabilization.md)
{% endcontent-ref %}

