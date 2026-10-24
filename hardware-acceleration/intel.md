---
description: Intel graphic cards support hardware accelerated encoding on Windows and Linux
---

# 🔵 Intel

Intel GPUs have the hardware accelerated video encoder called _Intel® Quick Sync Video_.&#x20;

The QSV[^1] encoder is well supported in Gyroflow and offers fast video render times on Windows and Linux.

{% hint style="warning" %}
**Important!** Always make sure you have the latest driver installed from the official [Intel drivers](https://www.intel.com/content/www/us/en/support/detect.html) page. As long as your GPU has the hardware encoder, most acceleration issues are solved by installing the latest driver.
{% endhint %}

## Supported graphic cards

Refer to the [Wikipedia](https://en.wikipedia.org/wiki/Intel\_Quick\_Sync\_Video) page for a list of supported hardware.

Quick Sync is part of Intel's integrated graphics and of the Intel Arc GPUs. It's available on Intel CPUs with integrated graphics, but not on the ones without it (models ending with F, e.g. Core i5-12400F).

## 10-bit support

Hardware accelerated encoding of 10-bit videos is supported only by the latest hardware. Refer to the [Wikipedia](https://en.wikipedia.org/wiki/Intel\_Quick\_Sync\_Video) page to check if your GPU supports 10-bit video encoding.

## AV1 support

Since Gyroflow 1.7.0, you can export AV1. It's encoded on the GPU only, so it needs an Intel Arc or newer GPU.

## ProRes/DNxHD support

_Intel® Quick Sync Video_ supports only H.264, H.265 and AV1. No other codec is hardware accelerated on Intel hardware.

## Maximum supported resolution

* H.264 - 4K on all generations.
* H.265 - 4K on 6th-10th gen Core (Skylake to Comet Lake), 8K on Ice Lake and 11th-14th gen Core, and 16K (16384x12288) on Intel Arc and Core Ultra.
* AV1 - 8K.

Gyroflow itself limits H.264 to 4096x4096, and H.265 and AV1 to 8192x8192. The per-generation limits are listed in [Intel's media driver documentation](https://github.com/intel/media-driver/blob/master/docs/media_features.md#supported-encoding-input-format-and-max-resolution).



[^1]: Intel® Quick Sync Video
