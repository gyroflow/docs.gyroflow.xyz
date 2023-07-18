---
description: >-
  This page contains troubleshooting steps when you run into an error in
  Gyroflow.
---

# 🐞 Troubleshooting

## Failed to export HEVC

If you see an error message like this:

<figure><img src="../.gitbook/assets/ffmpeg-error-558323010.png" alt=""><figcaption></figcaption></figure>

...then:

* If you're on Windows or Linux, make sure you're using the latest drivers for your GPU.
* If you're using an older GPU, it may not support hardware HEVC encoding, in which case, you should try H.264 instead.
* If H.264 also fails, try disable GPU encoding completely.
