# Other Cameras

## WitMotion

Gyroflow supports motion data recorded with [WT901 ](https://www.wit-motion.com/BLE/52.html)device from WitMotion. While it works, users reported some inaccuracies when recording with faster sample rate, so that device is generally not recommended

## VuzeXR

VuzeXR is a 360° camera, and while stabilizing 360° cameras is not supported, Gyroflow can read gyro data recorded by this camera.

## KanDao

Gyroflow can read gyro data from KanDao Obsidian Pro camera, but similarly to VuzeXR - can't stabilize 360° videos

## CAMM format

Lenovo VR180 camera records gyro data in the [CAMM ](https://developers.google.com/streetview/publish/camm-spec)format, and Gyroflow supports reading that format.

## XTU

Some XTU action cameras record gyro data in .gcsv files and the videos can be stabilized in Gyroflow
