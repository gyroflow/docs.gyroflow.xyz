---
description: Gyro data can be exported to multiple formats
---

# 🗃️ CSV Export

### Exporting from the app <a href="#gyroflow-to-csv" id="gyroflow-to-csv"></a>

You can export the gyro data using the **Export** button at the bottom of the **Motion data** panel in the app

<figure><img src="../.gitbook/assets/image (1).png" alt=""><figcaption></figcaption></figure>

\- **Camera data** - contains original gyroscope and accelerometer data + integrated quaternions and euler angles, and optionally the focus distance, iris (f/T-stop), zoom and focal length data. It can be exported to CSV, JSON, USD or JSX (which can be imported in Adobe After Effects by using File -> Run Script)\
\- **Full metadata** - contains original metadata as parsed from the source file, without any Gyroflow calculations\
\- **Parsed metadata** - contains metadata which is used by Gyroflow, and is already pre-calculated as needed by Gyroflow stabilization algorithms\
\- **Project file (including processed gyro data) -** is a project file + parsed metadata + synced timing info

When exporting the camera data, you can choose which fields to include. Fields that aren't present in the loaded file are greyed out. You can also choose between **Export all samples** and **Export one sample per frame**. With one sample per frame, it's the sample in the middle of the frame (ignoring the rolling shutter correction), and since Gyroflow 1.7.0 it can include the gyroscope and accelerometer data too, taken from the raw IMU sample closest to that time.



### Gyroflow to CSV python script <a href="#gyroflow-to-csv" id="gyroflow-to-csv"></a>

Gyroflow to CSV by [EmberLightVFX](https://github.com/EmberLightVFX) can convert Gyroflow project files to a CSV format containing the camera motion. This can be imported in software such as Blackmagic Fusion, Nuke and Unreal Engine, and may be used for visual effects work.

You can learn more [here](https://github.com/EmberLightVFX/GyroFlow-to-CSV).
