---
description: AMD graphic cards support hardware accelerated encoding on Windows and Linux
---

# 🔴 AMD

AMD GPUs have the hardware accelerated video encoder called _Advanced Media Framework_.&#x20;

The AMF[^1] encoder is well supported in Gyroflow and offers fast video render times on Windows and Linux.

{% hint style="warning" %}
**Important!** Always make sure you have the latest driver installed from the official [AMD Drivers](https://www.amd.com/en/support) page. As long as your GPU has the hardware encoder, most acceleration issues are solved by installing the latest driver.

However, someone reported a crash in a certain driver with Rx550, so if you're getting [this issue](https://github.com/gyroflow/gyroflow/issues/677), try a different driver version.
{% endhint %}

## Supported graphic cards

Refer to the official product page on amd.com to find out if it supports hardware video encoding.

For example the [AMD Radeon™ RX 6800](https://www.amd.com/en/products/graphics/amd-radeon-rx-6800) supports both H.264 and H.265 encoding, as per the specification:

<figure><img src="../.gitbook/assets/amd_specs.png" alt=""><figcaption><p>Look for H264 Encode and HEVC Encode in the specs</p></figcaption></figure>

## 10-bit support

H.265 10-bit encoding is supported since the Radeon RX 5000 series (VCN 2.0), and in Ryzen APUs with VCN 2.0 or newer. H.264 is always 8-bit. AV1 (RX 7000 series and newer) supports 10-bit as well. See the [AMF hardware support table](https://github.com/GPUOpen-LibrariesAndSDKs/AMF/wiki/GPU-and-APU-HW-Features-and-Support) for your GPU.

## AV1 support

Since Gyroflow 1.7.0, you can export AV1. It's encoded on the GPU only, so it needs a Radeon RX 7000 series or newer card.

## ProRes/DNxHD support

AMD's AMF encoder supports only H.264, H.265 and AV1. No other codec is hardware accelerated on AMD cards.

## Maximum supported resolution

* H.264 - 4K on all cards.
* H.265 - 4K on the RX 5000 series (VCN 2.0), 8K on the RX 6000 series (VCN 3.0) and newer.
* AV1 - 8K.

Gyroflow itself limits H.264 to 4096x4096, and H.265 and AV1 to 8192x8192. The per-GPU limits are listed in the [AMF hardware support table](https://github.com/GPUOpen-LibrariesAndSDKs/AMF/wiki/GPU-and-APU-HW-Features-and-Support).

## Known issues

Some AMD GPU encoders have a bug where it limits the bitrate to 20 Mbps, if the target bitrate is greater than 100 Mbps. Please check the file bitrate after rendering and if you're affected by this bug, you can either:&#x20;

* Set output bitrate to less than 100 Mbps
* Use **Custom encoder options**: `-rc cqp -qp_i 28 -qp_p 28`

[^1]: Advanced Media Framework
