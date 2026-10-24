---
description: >-
  NVIDIA graphic cards support hardware accelerated encoding on Windows and
  Linux
---

# 🟢 NVIDIA

NVIDIA GPUs since GeForce GTX 600 (2012) have the hardware accelerated video encoder called NVENC. At first it was just supporting H.264/AVC, but H.265/HEVC is also supported since the GeForce GTX 900 series (2014).

The NVENC encoder is well supported in Gyroflow and offers fast video render times on Windows and Linux.

{% hint style="warning" %}
**Important!** Always make sure you have the latest driver installed from the official [NVIDIA Drivers](https://www.nvidia.com/download/index.aspx?lang=en-us) page. As long as your GPU has the hardware encoder, most acceleration issues are solved by installing the latest driver.
{% endhint %}

## Supported graphic cards

Refer to the official [Support Matrix](https://developer.nvidia.com/video-encode-and-decode-gpu-support-matrix-new) at nvidia.com.

Make sure you are checking the correct codec (H.264 or H.265) and video bit-depth.

Read more about NVENC on [Wikipedia](https://en.wikipedia.org/wiki/Nvidia\_NVENC).

## 10-bit support

H.265 10-bit encoding is supported since the GeForce GTX 10 series (Pascal). AV1 supports 10-bit as well. Refer to the official [Support Matrix](https://developer.nvidia.com/video-encode-and-decode-gpu-support-matrix-new) for your GPU.

## AV1 support

Since Gyroflow 1.7.0, you can export AV1. It's encoded on the GPU only, so it needs a GeForce RTX 40 series or newer card.

## ProRes/DNxHD support

NVIDIA's NVENC supports only H.264, H.265 and AV1. No other codec is hardware accelerated on NVIDIA cards.

## Maximum supported resolution

* H.264 - 4096x4096 on all cards.
* H.265 - 4096x4096 on the GTX 900 series, 8192x8192 since the GTX 10 series.
* AV1 - 8192x8192.

Check the [Wikipedia](https://en.wikipedia.org/wiki/Nvidia\_NVENC) article to learn more.
