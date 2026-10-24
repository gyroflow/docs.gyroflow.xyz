# Mobile Phones

You can also use your **iPhone** or **Android** phone to capture gyroscope data.

### Apple iPhone

* [Sensor Logger](https://apps.apple.com/us/app/sensor-logger/id1531582925)
* [G-Field Recorder](https://apps.apple.com/at/app/g-field-recorder/id1154585693)
* [Gyro](https://apps.apple.com/us/app/gyro-record-device-motion-data/id1161532981)
* [GyLog](https://apps.apple.com/app/id6759689665)

### Android

* [Sensor Logger](https://play.google.com/store/apps/details?id=com.kelvin.sensorapp\&hl=de_AT\&gl=US)
* [Sensor Record](https://play.google.com/store/apps/details?id=de.martingolpashin.sensor_record)
* [Open Camera Sensors](https://github.com/MobileRoboticsSkoltech/OpenCamera-Sensors)
* [MotionCam Pro](https://play.google.com/store/apps/details?id=com.motioncam.pro)
* [Film it](https://play.google.com/store/apps/details?id=com.software.illusions.unlimited.filmit\&hl=en_US)
* [GyLog](https://play.google.com/store/apps/details?id=com.kumoinc.gylogrecorder)

### Using as a camera

If you're filming on your phone, it's best to use an app that records the video and the gyro at the same time (e.g. MotionCam Pro, Film it or Open Camera Sensors), it will be easier to synchronize.

If your phone has OIS (Optical image Stabilization) and it's not possible to disable it - it will interfere with stabilization results.

Gyroflow will not work if you had any internal stabilization enabled on your phone.

### MotionCam Pro RAW video

Since Gyroflow 1.7.0, Gyroflow reads the motion data and the lens calibration from MotionCam Pro RAW video files (.mcraw), but it can't play these files. Open a playable copy of the clip (e.g. a transcode or a proxy) as the video, and then load the original **.mcraw** file in **Motion data -> Open file**.

### Using as external gyro logger

If you want to record gyro data for use with an external camera, you'll have to figure out the [**🔀 IMU Orientation**](../../advanced-usage/imu-orientation-and-rotation.md), depending on the way you mounted the phone.

