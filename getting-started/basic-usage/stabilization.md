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

TODO

## Zooming

With post-stabilization, we only have as much information as the sensor captured. If we move these pixels around, we'll end up missing some information on the sides. Because of that, applying the stabilization only will result in moving empty sides around the video.

To compensate for that, we need to zoom in the image to show the available pixels only. This is also referred to as cropping.

Gyroflow has 3 available zooming modes:

1. **No zooming** - This mode simply disables all zooming and applies only the stabilization.
2. **Dynamic zooming** - In this mode, the zoom amount will be dynamically changing throughout the video to use as much pixels as we have available. Some parts of the video will need to be zoomed in more and some less, and this mode creates smooth transition between these zoom levels.
3. **Static zoom** - this mode will apply one, static crop for the entire duration of the video. This will remove the zooming in-and-out effect, but it will use the maximum needed zoom level, thus cropping the video a lot. It may work in some cases if there is not much movement, but it's generally recommended the use the _Dynamic zooming_ mode instead (and if you want to avoid the visible zooming in-and-out effect, use high zooming speed, like 20-30s).

### Zooming speed

TODO

## Rolling shutter correction

TODO

### For more control over stabilization settings, see the Advanced section:

{% content-ref url="../../advanced-usage/stabilization.md" %}
[stabilization.md](../../advanced-usage/stabilization.md)
{% endcontent-ref %}

