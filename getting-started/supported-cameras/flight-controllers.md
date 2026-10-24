# Flight controllers

The gyro data logged by a drone's flight controller can stabilize the video of any camera on that drone. You'll have to synchronize the log with the video, see [⌛ Synchronization](../basic-usage/synchronization.md#synchronizing-external-gyro-source).

## Betaflight

Gyroflow reads Betaflight blackbox logs (`.bbl`, `.bfl`, or `.csv` exported from Blackbox Explorer). INAV, Emuflight and other forks use the same format.

* Set up the logging with the Gyroflow logger presets, see [Flight controller](../../advanced-usage/using-external-gyro-source/flight-controller.md#gyroflow-logger-preset).
* Enter the camera uptilt in `Motion data -> Rotation -> Pitch`, see [Camera angle](../../advanced-usage/using-external-gyro-source/flight-controller.md#camera-angle).
* If the log file contains several flights, choose yours in **Select log**, see [Multiple logs in one file](../../advanced-usage/using-external-gyro-source/flight-controller.md#multiple-logs).

## ArduPilot

Gyroflow reads ArduPilot logs (`.bin` or `.log`) directly, there's no need to convert them.

Set up the logging as described in ArduPilot's [Video Stabilization Using IMU data and Gyroflow](https://ardupilot.org/copter/docs/common-gyroflow.html):

* enable the video stabilization bit in `LOG_BITMASK` (bit 20 for Copter and Rover, bit 21 for Plane),
* set `SCHED_LOOP_RATE` to 300 Hz or higher (Plane and Rover default to 50 Hz),
* optionally, set `LOG_FILE_DSRMROT` to 1, to get one log per flight.

Enter the camera angle in `Motion data -> Rotation -> Pitch`: positive when the camera is tilted up, negative when it's tilted down (on planes, often around -10° to -20°).

### Wobble in planes

Jello or wobble that Gyroflow doesn't remove often comes from a soft-mounted camera: it moves differently than the autopilot, whose IMU recorded the motion. ArduPilot suggests hard mounting the camera to the airframe, but warns that it can also make the results worse, so test both.

Since Gyroflow 1.7.0, you can also use [Optical correction](../../advanced-usage/input-data.md#optical-correction) with **Correction** set to **Per row**. It corrects the motion data wherever the image disagrees with it, and removes the jello.
