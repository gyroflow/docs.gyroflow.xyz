# ESP-gyrologger

[esp-gyrologger](https://github.com/VladimirP1/esp-gyrologger) is an open source motion data logger by Vladimir Pinchuk. It's a firmware for the ESP32 and ESP32-C3 microcontrollers with a BMI270, BMI160 or MPU6050 gyro. It's small and light, so you can mount it on any camera that doesn't record motion data, and load its logs in Gyroflow.

* It's set up from a web page over its own Wi-Fi network, where you also change the settings and calibrate the accelerometer.
* You can also connect a button and a LED to start and stop the recording.
* You'll have to synchronize the logs with the video, see [Synchronizing external gyro source](../../getting-started/basic-usage/synchronization.md#synchronizing-external-gyro-source).

## DIY

You can build one from an ESP32 or ESP32-C3 board with 4 MB of flash and a gyro module, connected with 4 wires. The [repository](https://github.com/VladimirP1/esp-gyrologger#how-to-flash-and-get-started-with-the-diy-version) has the wiring, the firmware download, the flashing steps and a video tutorial.

## Ready-made boards (PCBA)

There are PCB designs for a tiny dedicated board (EspLog, 11 x 14 mm) that you can order assembled from a PCB manufacturer:

* the original 4-layer design: [EasyEDA project](https://oshwlab.com/vladimir.pinchuk01/gyro-logger-esp32c3_copy)
* 2-layer designs, including one with a 4.5-30 V input: [esplog-hardware](https://github.com/VladimirP1/esplog-hardware) (KiCad 6)
