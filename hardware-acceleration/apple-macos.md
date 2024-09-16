---
description: >-
  macOS supports hardware video encoding (H.264/H.265) on most devices, with
  ProRes acceleration added in the Apple Silicon chips.
---

# ⚪ Apple

Hardware video encoding on macOS uses the _VideoToolbox_ framework from Apple. Most devices are supported.

Intel devices support basic H.264 and H.265 modes (8-bit only), while M1 chips and newer added more formats and the ProRes encoder.

{% hint style="info" %}
Apple Silicon (M1-M4) devices are recommended, because they support more video features, including ProRes encoder.
{% endhint %}

It's also highly recommended to update to the newest macOS version, as the video encoding features are evolving pretty quickly.

## Hardware acceleration support

<table><thead><tr><th width="240">Processor</th><th width="85">H.264</th><th width="91">H.265</th><th>ProRes</th></tr></thead><tbody><tr><td>Intel</td><td>✅</td><td>✅</td><td><a href="https://emojipedia.org/cross-mark/">❌</a></td></tr><tr><td>M1 Air</td><td>✅</td><td>✅</td><td><a href="https://emojipedia.org/cross-mark/">❌</a></td></tr><tr><td>M1 Pro/Max/Ultra</td><td>✅</td><td>✅</td><td>✅</td></tr><tr><td>M2<br>M3<br>M4</td><td>✅</td><td>✅</td><td>✅</td></tr></tbody></table>



