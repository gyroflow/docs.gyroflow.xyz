---
description: >-
  macOS supports hardware video encoding (H.264/H.265) on most devices, with
  ProRes acceleration added in the Apple Silicon chips.
---

# ⚪ Apple macOS

Hardware video encoding on macOS uses the _VideoToolbox_ framework from Apple. Most devices are supported.

Intel devices support basic H.264 and H.265 modes (8-bit only), while M1 and M2 chips added more formats and the ProRes encoder.

{% hint style="info" %}
Apple Silicon (M1/M2) devices are recommended, because they support more video features, including ProRes encoder.
{% endhint %}

It's also highly recommended to update to the newest macOS version, as the video encoding features are evolving pretty quickly.

## Hardware acceleration support

| Processor        | H.264 | H.265 | ProRes                                  |
| ---------------- | ----- | ----- | --------------------------------------- |
| Intel            | ✅     | ✅     | [❌](https://emojipedia.org/cross-mark/) |
| M1 Air           | ✅     | ✅     | [❌](https://emojipedia.org/cross-mark/) |
| M1 Pro/Max/Ultra | ✅     | ✅     | ✅                                       |
| M2 (all)         | ✅     | ✅     | ✅                                       |



