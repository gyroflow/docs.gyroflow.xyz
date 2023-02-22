---
description: Frequently asked questions
---

# ❔ FAQ

{% hint style="info" %}
Most issues with settings can be solved by resetting them to defaults. Go to **Advanced** on bottom right and click <mark style="color:red;">**Reset all settings to defaults.**</mark>
{% endhint %}

## ❓GPU encoding is not available or not working

Support for codecs and modes varies greatly between GPU brands and models, for more information refer to specific guides for [NVIDIA](../hardware-acceleration/nvidia.md), [AMD](../hardware-acceleration/amd.md), [Intel](../hardware-acceleration/intel.md), [macOS ](../hardware-acceleration/apple-macos.md)or [Android](../hardware-acceleration/android.md). If you don't know what GPU you have, refer to your computer specifications.

**Always make sure you have the latest GPU driver.**

If your computer is older than 2016, chances are H.265/HEVC will not be available, and if it's older than 2012, H.264 may also not be available.

H.264 doesn't support 10-bit GPU encoding, and ProRes is accelerated only on M1/M2 macs.

## ❓I'm using O3 Air Unit and I have a lot of vibrations after stabilizing

Read more [**in the DJI section**](supported-cameras/dji.md#dji-o3-air-unit-vibration-issues).

## ❓I see only black screen when stabilization is enabled.

Most likely you have **Fixed camera** method selected in the Stabilization section. Change to **Default** or use **Advanced -> Reset all settings to defaults** (on the bottom right)**.**

## ❓I'm getting a crash when trying to export and I'm using macOS with Intel processor

Go to **Advanced -> Device for video processing** and select one starting with **\[wgpu]**

## ❓**I'm having trouble stabilizing a RunCam camera**

Refer to the [**RunCam section**](supported-cameras/runcam.md). Make sure to learn the synchronization process by reading [**🔧 Basic usage**](basic-usage/), [**📈 Timeline and gyro chart**](basic-usage/timeline-and-gyro-chart.md) and [**⌛ Synchronization**](basic-usage/synchronization.md).

## ❓Colors are changed after exporting from Gyroflow

Read about this issue on the [**🎨 Color differences**](../advanced-usage/color-differences.md) page.

## ❓My video is zoomed in too much

Make sure to use **Default** algorithm in the Stabilization section. Read more on the [**🔥 Stabilization**](basic-usage/stabilization.md) page.

## ❓I'm seeing black bars around my video

Enable **Dynamic zooming** in the Stabilization section.

## ❓How to export vertical video?

Go to **Export settings**, uncheck the lock icon and enter 9:16 resolution there, e.g. 1080x1920. Read more on the [**🎬 Exporting**](basic-usage/exporting.md#output-size) page.

## ❓Where can I ask questions?

You can get support and ask all questions on [**Discord**](https://discord.com/invite/BBJ2UVAr2D).

