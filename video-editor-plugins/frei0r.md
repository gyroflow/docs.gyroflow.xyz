---
description: Gyroflow frei0r plugin
---

# 🪄 frei0r

**frei0r** is an effect framework which is supported in the following applications:

* [MLT](https://www.mltframework.org/)
* [LiquidSoap](https://www.liquidsoap.info/)
* [Kdenlive](https://www.kdenlive.org/)
* [Shotcut](https://www.shotcut.org/)
* [FFMpeg](https://ffmpeg.org/)
* [PureData](https://puredata.info/)
* [Open Movie Editor](http://www.openmovieeditor.org/)
* [Gephex](https://gephex.org/)
* [LiVES](http://lives.sf.net/)
* [FreeJ](https://freej.dyne.org/)
* [VeeJay](http://veejayhq.net/)
* [Flowblade](https://jliljebl.github.io/flowblade/)

This plugin allows you to apply the stabilization right in your frei0r-capable video editor. It uses a project file exported from Gyroflow, including its motion data filters, like the low-pass filter.

### Shotcut

The plugin has its own filter panel in Shotcut, where you can open the Gyroflow project file, change the smoothness, show the stabilization overview, and scale the timestamps for footage whose playback speed has changed. The stabilization stays in sync when you trim or split the clips. The filter panel files are in the `shotcut/gyroflow` folder of the [gyroflow-plugins repository](https://github.com/gyroflow/gyroflow-plugins/tree/main/shotcut/gyroflow).

### More information and download:

## [https://github.com/gyroflow/gyroflow-plugins](https://github.com/gyroflow/gyroflow-plugins)
