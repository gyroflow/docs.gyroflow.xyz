# 🔀 IMU orientation and rotation

### Gyro rotation

To achieve correct stabilization, the rotation of the motion data needs to match the video. If the IMU sensor is not pointing in the same direction as the camera sensor, then you'll have to rotate the motion data to match. All cameras with internal gyro data already have the correct rotation, but when using external gyro source, chances are you may have to enter the rotation values here.

A common example is using [blackbox gyro data](#user-content-fn-1)[^1] with camera tilted upwards, or a naked BMPCC, where the IMU sensor is on the main board, and the camera sensor is separated.

You can use optical flow data to determine the rotation values. When you change the rotation here, the lines on chart will move as well. Optical flow never moves, so you can change the rotation values by a few degrees and observe how it's changing on the chart, and when they match/align with the optical flow lines.

TODO: Video example of that

### Accelerometer rotation

In some rare cases, the gyroscope and accelerometer are actually two separate chips, in two different places. If you **right click on the Rotation label** you'll be able to enable **Separate accelerometer rotation.** You can then control gyroscope and accelerometer rotation separately. One example of such setup is the naked GoPro in [Umma95](https://ummagawd.com/products/umma95-beta95x-naked-gopro-cinewhoop-kit) design (you can try accelerometer pitch: 90°).

### IMU orientation

Similar to **gyro rotation**, the IMU orientation field describes the direction of the IMU chip. IMU orientation however, describes the general direction of the IMU chip.

The default value, indicating no direction changes is `XYZ`. This means that the X, Y and Z axes correspond directly to the expected X, Y and Z axes.&#x20;

If we need to invert an axis, the letter will be lowercased, so `xYZ` will mean the X axis corresponds to the inverted expected X axis, and Y and Z are not changed.&#x20;

If we need to swap the axes, we can put another axis on the expcted position, for example `ZYX` will mean that the X axis corresponds to the Z axis, and Z axis corresponds to the X axis, and Y axis is not changed.

The first letter describes what axis is expected at the X axis, the second letter at the Y axis, and third at the Z axis.

You can use the same technique as described in the **Gyro rotation** section to determine the IMU orientation: Adjust the value and observe the lines on the chart until the light lines (optical flow) and dark lines (motion data) align up.&#x20;

You can also right click on the timeline and choose **Guess IMU orientation here** to attempt to determine the IMU orientation automatically. Note that this function needs large enough **Sync search size**, same as used for the [**⌛Synchronization**](../getting-started/basic-usage/synchronization.md).

TODO: video example

[^1]: from your drone flight controller
