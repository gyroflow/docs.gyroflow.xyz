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

TODO: CPU support?

## 10-bit support

Hardware accelerated encoding of 10-bit videos is supported only by the latest hardware. Refer to the [Wikipedia](https://en.wikipedia.org/wiki/Intel\_Quick\_Sync\_Video) page to check if your GPU supports 10-bit video encoding.

## ProRes/DNxHD support

_Intel® Quick Sync Video_ supports only H.264 and H.265. No other codec is hardware accelerated on Intel hardware.

## Maximum supported resolution

8192x8192 is the maximum supported resolution on modern cards.

TODO: verify this



[^1]: Intel® Quick Sync Video
