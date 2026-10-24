# 🌀 Using external gyro source

If your camera doesn't record motion data, you can record it with a separate device mounted on the camera, and load it in Gyroflow.

## Devices

* [NiYien-A1](niyien-a1.md) - a logger made for Gyroflow, mounted on the hot shoe.
* [Action camera as a logger](action-camera-as-a-logger.md) - e.g. a GoPro mounted on the camera.
* [Flight controller](flight-controller.md) - the blackbox log of your drone.
* [ESP-gyrologger](esp-gyrologger.md) - a small open source logger you can build or order.
* [Flowshutter](https://old-docs.gyroflow.xyz/flowshutter/flowshutter/) - an older camera remote that syncs the recording with a flight controller's blackbox (no longer developed).
* [Mobile phones](../../getting-started/supported-cameras/mobile-phones.md) - apps that record the phone's gyro.
* Any other logger that writes the [GCSV format](../../technical-details/gcsv-format.md).

## Workflow

1. Mount the logger rigidly to the camera, ideally facing the same direction. A logger that moves or vibrates on its own records motion the camera didn't do.
2. Open the video and choose the lens profile of your camera.
3. Load the log in `Motion data -> Open file`. If it's named like the video (e.g. `clip.gcsv` next to `clip.mp4`), it's loaded automatically.
4. Set the [IMU orientation and rotation](../imu-orientation-and-rotation.md), if the logger isn't aligned with the camera.
5. Synchronize: set **Rough gyro offset** and **Sync search size** to cover the time between starting the logger and the camera, and run **Auto sync**. See [Synchronizing external gyro source](../../getting-started/basic-usage/synchronization.md#synchronizing-external-gyro-source).

{% hint style="info" %}
Without any logger, you can also measure the camera motion from the video itself, with [Optical correction](../input-data.md#optical-correction) (since Gyroflow 1.7.0).
{% endhint %}
