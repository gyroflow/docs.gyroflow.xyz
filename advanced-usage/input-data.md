# 🎬 Input data

## Video information

### Video frame rate

If you have your video recorded in VFR mode (project fps different than real-time recording fps, e.g. 120 fps recorded as 30 fps slowed down), then the video playback will not match your motion data, as motion data is always recorded in real time.&#x20;

You can let Gyroflow know what was the real-time frame rate used (e.g. 120fps) and it will stretch the motion data to match the playback rate of the video. In some cases, this value will be read from the file metadata, but if it's not, you can click on the ✏️**Edit** icon and enter the **real time fps** in **Video information -> Frame rate** field.

This value only affects the motion data, so it will not make the video playback faster.

{% hint style="warning" %}
Remember to put exact frame rate, for NTSC it's 23.976, 29.97, 47.952, 59.94, 119.88 etc. For PAL it's 25, 50, 100 etc.)
{% endhint %}

### Video rotation

If you captured the video on the side or upside down, you can edit that field and enter rotation in degrees.

## Motion data

### Low pass filter

Low pass filter is useful to filter out high frequency vibrations or oscillations picked up by the IMU sensor. If you're using the camera on a drone, and the camera mount is not filtering the vibrations from the motors, then your motion data can be noisy and it will affect the stabilization. In this case, you can apply Low pass filter to filter out the high frequency vibrations while keeping the lower frequency camera motion intact.

The usual values range from 30 Hz to 70 Hz.

In some extreme cases, very low low pass filter can be useful if the motion data is very bad (e.g. GoPro Hero 7), but it will not magically fix all issues. You have to play with this value and find one that works for your case. Try to find a value which filters out the vibrations but doesn't filter the general motion too much.

The low pass filter is applied to gyroscope and accelerometer readings. If the Integration method is None, then it applies it to the Quaternions as well.

TODO: example video of this in action

### Gyro rotation

To achieve correct stabilization, the rotation of the motion data needs to match the video. If the IMU sensor is not pointing in the same direction as the camera sensor, then you'll have to rotate the motion data to match. All cameras with internal gyro data already have the correct rotation, but when using external gyro source, chances are you may have to enter the rotation values here.

A common example is using [blackbox gyro data](#user-content-fn-1)[^1] with camera tilted upwards, or a naked BMPCC, where the IMU sensor is on the main board, and the camera sensor is separated.

You can use optical flow data to determine the rotation values. When you change the rotation here, the lines on chart will move as well. Optical flow never moves, so you can change the rotation values by a few degrees and observe how it's changing on the chart, and when they match/align with the optical flow lines.

TODO: Video example of that

### Accelerometer rotation

In some rare cases, the gyroscope and accelerometer are actually two separate chips, in two different places. If you **right click on the Rotation label** you'll be able to enable **Separate accelerometer rotation.** You can then control gyroscope and accelerometer rotation separately. One example of such setup is the naked GoPro in [Umma95](https://ummagawd.com/products/umma95-beta95x-naked-gopro-cinewhoop-kit) design (you can try accelerometer pitch: 90°).

### Gyro bias

TODO

### IMU orientation

TODO

### Integration method

IMU integration (or sensor fusion) is the process of calculating the final camera position in space, using readings from the gyroscope and an accelerometer. Raw IMU sensor readings indicate velocity (change in position or rotation over time), and not the actual position of the device.

Gyroflow needs to know the actual position in space, so it needs to convert these raw readings to final position. It does that by using the integration algorithm. There are a few IMU integration algorithms available in Gyroflow:

* **None** - This option is available when the camera already has the actual device position in the metadata, because it calculated it on-board. For example GoPro and DJI cameras calculate it in-camera, so Gyroflow doesn't have to do that. When using this option, Gyroflow will simply use the integrated quaternions provided by the camera.
* **VQF** - modern and robust integration algorithm. It is the default algorithm and works well for most cases. It's based on [this paper](https://arxiv.org/pdf/2203.17024.pdf) if you want to read more about it.&#x20;
* **Complementary** - a very good integration algorithm, works well in most cases. You can try this one when the VQF doesn't deliver good results. It's based on [this paper](https://www.ncbi.nlm.nih.gov/pmc/articles/PMC4570372/pdf/sensors-15-19302.pdf) if you want to read more about it.&#x20;
* Si**mple gyro** - simplest gyroscope integration method. This is a very simple algorithm without without using the accelerometer or any bias estimations. Therefore horizon lock won't work with this method. In practice, it's rarely useful, unless you have somehow damaged accelerometer data and want to skip it.&#x20;
* **Simple gyro with accel** - Similar to the previous one, but also includes accelerometer, so the horizon lock can work. In practice, it's rarely useful.&#x20;
* **Madgwick** - An integration algorithm based on [this paper](https://courses.cs.washington.edu/courses/cse466/14au/labs/l4/madgwick\_internal\_report.pdf). Works well in most cases.
* **Mahony** - An integration algorithm based on [this paper](https://hal.science/hal-00488376/document).



[^1]: from your drone flight controller
