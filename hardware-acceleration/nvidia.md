---
description: >-
  NVIDIA graphic cards support hardware accelerated encoding on Windows and
  Linux
---

# 🟢 NVIDIA

NVIDIA GPUs since GeForce GTX 600 (2012) have the hardware accelerated video encoder called NVENC. At first it was just supporting H.264/AVC, but H.265/HEVC is also supported since GTX 750 (2015).

The NVENC encoder is well supported in Gyroflow and offers fast video render times on Windows and Linux.

{% hint style="warning" %}
**Important!** Always make sure you have the latest driver installed from the official [NVIDIA Drivers](https://www.nvidia.com/download/index.aspx?lang=en-us) page. As long as your GPU has the hardware encoder, most acceleration issues are solved by installing the latest driver.
{% endhint %}

## Supported graphic cards

Refer to the official [Support Matrix](https://developer.nvidia.com/video-encode-and-decode-gpu-support-matrix-new) at nvidia.com.

Make sure you are checking the correct codec (H.264 or H.265) and video bit-depth.

Read more about NVENC on [Wikipedia](https://en.wikipedia.org/wiki/Nvidia\_NVENC).

## 10-bit support

Hardware accelerated encoding of 10-bit videos is supported only by the latest video cards. Refer to the official [Support Matrix](https://developer.nvidia.com/video-encode-and-decode-gpu-support-matrix-new) to check if your GPU supports 10-bit video encoding.

## ProRes/DNxHD support

NVIDIA's NVENC supports only H.264 and H.265. No other codec is hardware accelerated on NVIDIA cards.

## Maximum supported resolution

8192x8192 is the maximum supported resolution on modern cards, and 4096x4096 on older. Check the [Wikipedia](https://en.wikipedia.org/wiki/Nvidia\_NVENC) article to learn more.
