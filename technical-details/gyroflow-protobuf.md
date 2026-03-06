---
description: >-
  Gyroflow protobuf format is designed to contain even the most advanced and
  detailed data about the video capture pipeline, that is useful for
  post-stabilization.
---

# 🗄️ Gyroflow protobuf

## Download the .proto definition

You can download the latest protobuf definition from telemetry-parser's [git repository](https://github.com/AdrianEddy/telemetry-parser/blob/master/src/gyroflow/gyroflow.proto).

## Supported features

* Camera and lens metadata: Brand, model, focal length, f-number, focus distance etc.
* Lens distortion model and coefficients
* Frame readout time for rolling shutter correction
* Frame capture metadata - ISO, shutter speed, white balance, etc.
* Raw IMU samples - gyroscope, accelerometer, magnetometer readings
* Quaternions - final camera orientation after sensor fusion
* Lens OIS data - detailed information about lens OIS movements so we can stabilization when Lens OIS was enabled
* IBIS data - detailed information about the in-body image stabilization, so we can support stabilization when IBIS was enabled
* EIS data - if the camera contains any form of electronic stabilization, the protobuf can contain what exactly it did to the image so we can account for it.

***

## Technical details

This page provides detailed documentation for integrating the Gyroflow Protobuf format natively into camera firmware. By embedding this standardized telemetry directly into video files, cameras can achieve pixel-perfect software stabilization, distortion correction, and rolling shutter compensation in Gyroflow and supported NLEs.

### 1. Binary Protobuf Embedding in ISO Base Media File Format (MP4/MOV)

#### Transport format

The Gyroflow Protobuf data should be stored as binary data in a separate MP4 track of the video file. This makes it easy to read and write and it's the standard way to embed additional data in video files.

The data should be stored per-frame so that each video frame should have corresponding metadata in the metadata MP4 track.

Track Setup:

1. Create a dedicated track with `hdlr` (Handler Reference Box) set to `'meta'` (Metadata).
2. The `stsd` (Sample Description Box) should indicate a binary gyroflow format `'gyrf'`.
3. Maintain a 1:1 Sample-to-Frame relationship. For every encoded video frame in the video track, there must be exactly one corresponding sample in the metadata track.

#### Sample Structure

* First Sample: The very first metadata sample must contain the `Main` message initialized with the `Header` block (camera metadata, clip metadata) AND the first `FrameMetadata`.
* Subsequent Samples: All subsequent samples should contain the `Main` message but omit the `Header` block, including only the `FrameMetadata` for that specific frame.

#### MP4 Track Diagram

```
[ MP4 Container ]
 │
 ├── [ Video Track ] (hdlr = 'vide')
 │    ├── Frame 1 (0 ms)
 │    ├── Frame 2 (16 ms)
 │    └── Frame 3 (33 ms)
 │
 └── [ Metadata Track ] (hdlr = 'meta', stsd = 'gyrf')
      ├── Sample 1 (Syncs with Frame 1)
      │    └── protobuf Main { magic_string: "GyroflowProtobuf", Header: {...}, FrameMetadata: {...} }
      ├── Sample 2 (Syncs with Frame 2)
      │    └── protobuf Main { magic_string: "GyroflowProtobuf", FrameMetadata: {...} }
      └── Sample 3 (Syncs with Frame 3)
           └── protobuf Main { magic_string: "GyroflowProtobuf", FrameMetadata: {...} }
```

***

### 2. General overview of the included fields

The Gyroflow Protobuf schema is designed to encapsulate all the necessary metadata required for advanced video stabilization and lens correction.&#x20;

#### The Header (Static Metadata)

The `Header` message acts as the foundational context for the entire video clip. It contains information that generally remains constant throughout the recording and is divided into two sub-messages:

* `CameraMetadata`: This defines the physical hardware used to capture the footage.
  * **Identification**: Fields like `camera_brand`, `camera_model`, `lens_brand`, and `lens_model` help identify the exact gear setup.
  * **Sensor & Optics**: Fields like `sensor_pixel_width`, `sensor_pixel_height`, and `pixel_pitch_nm` define the physical characteristics of the sensor. The `lens_profile` field can be used to embed the lens profile, or the lens distortion metadata can be stored directly using fields in the `LensData`.
  * **Orientation**: `imu_orientation` and the optional `imu_rotation`/`quats_rotation` quaternions allow you to define base offsets for the sensor data, ensuring the software interprets the XYZ axes correctly.
* `ClipMetadata`: This defines the specific parameters of the video file itself.
  * **Dimensions & Timing**: Standard video properties like `frame_width`, `frame_height`, and `duration_us`.
  * **Framerates**: It separates `record_frame_rate`, `sensor_frame_rate`, and `file_frame_rate` to accurately handle variable frame rate (VFR) and slow-motion recording scenarios.

#### FrameMetadata (Dynamic Data)

The `FrameMetadata` message contains the telemetry and camera settings that change continuously during the recording. Because multiple IMU samples usually occur within the span of a single video frame, this message is built to handle arrays of data.

* **Timing & Synchronization**: `start_timestamp_us` and `end_timestamp_us` strictly define the capture window of the frame using the camera's internal clock.

{% hint style="warning" %}
It's crucial to include accurate timestamps for both the sensor readouts and IMU data samples. All the timestamps should come from the same internal monotonic camera clock. The clock doesn't have to be synchronized with wall time.
{% endhint %}

* **Per-Frame Camera Settings**: Exposure and image properties can fluctuate, especially in auto-exposure modes. Fields like `iso`, `exposure_time_us`, and `white_balance_kelvin` track these shifts.
  * Dynamic cropping (`crop_x`, `crop_width`, `digital_zoom_ratio`) allows the metadata to reflect digital zooms or sensor punches that happen mid-recording.
* **LensData**: This field captures dynamic optical changes, such as shifts in `focal_length_mm` or `focus_distance_mm`.
  * It also embeds mathematical distortion models and their corresponding `distortion_coefficients` and `camera_intrinsic_matrix` to accurately map lens warping at that specific moment in time.
* **Motion Data**:
  * `IMUData`: This is the core raw telemetry. It contains arrays of gyroscope (rotation in degrees/sec) and accelerometer (acceleration in m/s²) readings, alongside precise `sample_timestamp_us` markers for each sample.
  * `QuaternionData`: If the camera performs its own sensor fusion, this field provides the calculated orientation (W, X, Y, Z angles) mapped to a timestamp.

***

### 3. Frame Readout Time and Rolling Shutter Correction

Rolling shutter occurs because CMOS sensors read pixels row-by-row rather than instantly. To correct this, Gyroflow maps _every single pixel row_ to an exact IMU timestamp.

#### Timestamps & VSync

Gyroflow requires timestamps to be strictly linked to the internal camera clock, down to the microsecond (`_us`).

* `start_timestamp_us`: The exact moment the first row of the crop area is exposed/read.
* `end_timestamp_us`: The exact moment the last row of the crop area is exposed/read.

The `frame_readout_time_us` in `ClipMetadata` must represent the readout time of the _captured crop_ (not the whole physical sensor, unless reading the whole sensor).

#### Readout & IMU Interpolation Diagram

```
TODO: diagram
```

{% hint style="info" %}
Gyroflow does not use IMU data outside of the captured pixels. The `IMUData` array provided in `FrameMetadata` can include all the IMU samples (even outside of the capture window), but you have to make sure the timestamps of the IMU samples and sensor the readouts are in sync, so Gyroflow can skip not needed samples.
{% endhint %}

***

### 4. Lens Data and Distortion Models

To accurately stabilize a video, Gyroflow needs to undistort the image before applying the rotation. The `LensData` message defines camera intrinsics and lens distortion parameters.

#### Standard Models

* [OpenCV Fisheye model](https://docs.opencv.org/4.13.0/db/d58/group__calib3d__fisheye.html): Default model used in gyroflow. It also works for non-fisheye lenses. Uses 4 coefficients (`p1, p2, p3, p4`)
* [OpenCV Standard model](https://docs.opencv.org/4.13.0/d9/d0c/group__calib3d.html): Classic polynomial radial/tangential models (`k1,k2,p1,p2,k3,k4,k5,k6`).
* [Poly3 / Poly5 / PTLens](https://lensfun.github.io/calibration-tutorial/lens-distortion.html): Lensfun models.

#### Generic Polynomial

For manufacturers using complex custom glass mapping, the `GenericPolynomial` model calculates physical distortion offsets.

Math implementation:

Let $$(X, Y)$$ be normalized image coordinates.

1. Radius: $$r = \sqrt{X^2 + Y^2}$$
2. Angle: $$\theta = \arctan(r)$$
3.  Distortion calculation using polynomial coefficients ($$k_0$$ to $$k_5$$):

    $$\theta_d = \theta \cdot k_0 + \theta^2 \cdot k_1 + \theta^3 \cdot k_2 + \theta^4 \cdot k_3 + \theta^5 \cdot k_4 + \theta^6 \cdot k_5$$
4. Scaling factor: $$scale = \theta_d / r$$ (if $$r=0$$, scale is $$1.0$$)
5.  Apply post-scale parameters ($$k_6$$, $$k_7$$):

    $$X_{distorted} = X \cdot scale \cdot k_6$$

    $$Y_{distorted} = Y \cdot scale \cdot k_7$$

Intrinsic Matrix:

The protobuf requires a row-major 3x3 intrinsic matrix. Usually defined as:

```
[[fx,  0, cx],
 [ 0, fy, cy],
 [ 0,  0,  1]]
```

***

### 5. Lens OIS Data (Optical Image Stabilization)

Optical stabilization physically shifts a floating lens element to counteract camera shake. Because the glass moves independently of the camera body, the IMU (which is rigidly attached to the body) records rotation that the _sensor didn't actually see_.

By supplying `LensOISData` (the X/Y shift of the optical element in nanometers at specific timestamps), Gyroflow mathematically calculates the exact optical deviation angle. It then subtracts the OIS movement from the IMU quaternion data to establish the absolute trajectory of the optical path before applying its own digital stabilization.

The sampling rate of OIS data can be much lower than the IMU data, because it's a physical element, it will move slowly. Gyroflow will interpolate any gaps. Typically 3-10 samples of OIS data per frame should be enough.

```
  TODO: diagram
```

***

### 6. IBIS Data (In-Body Image Stabilization)

IBIS mechanically shifts and rolls the physical image sensor on its X, Y, and Roll axes.

How it works in the Protobuf:

`IBISData` requires the exact timestamp, `shift_x`, `shift_y` (in nanometers), and `roll_angle_degrees`.

Since the sensor is physically moving inside the camera body during exposure, Gyroflow needs this data for the same reason it needs OIS: the IMU records the camera body moving, but the pixels being recorded are shifting within the body.

The sampling rate of IBIS can be much lower than the IMU data, because it's a physical element, it will move slowly. Gyroflow will interpolate any gaps. Typically 3-10 samples of IBIS data per frame should be enough.

#### IBIS interaction with IMU

If IBIS is active, Gyroflow maps the mechanical sensor position at the exact `row_readout_time`.

* Sensor X/Y Shift: Converted from nanometers to pixel offsets based on `pixel_pitch_nm`.
* Sensor Roll: Added directly to the Z-axis rotation matrix before inverse-projection.

If IBIS completely countered a bump, Gyroflow relies on the `IBISData` to know that the frame is already stable at that specific timestamp, preventing Gyroflow from digitally correcting a bump that IBIS already fixed (preventing double-correction artifacts).

```
  TODO: diagram
```

***

### 7. EIS Data (Electronic Image Stabilization)

When the camera applies internal _digital_ stabilization (EIS), the pixels stored in the final MP4 are vastly different from the raw sensor readout. To stabilize this in post, Gyroflow needs to know exactly how the camera deformed the original sensor crop.

```
  TODO: diagram
```

There are three options to choose from:

#### A. QUATERNION

The camera encodes a quaternion representing the internal rotation the camera applied to the frame.

Gyroflow applies the inverse of this quaternion to the frame to "un-stabilize" the footage back to raw sensor data, and then applies its own smoother quaternion path.

This method is used by GoPro (internal Hypersmooth)

#### B. MESH\_WARP&#x20;

The camera divides the video frame into a grid (`grid_width` x `grid_height`). The `values` array contains a list of floats representing how each grid intersection was displaced (X, Y) relative to the original sensor read.

This allows to encode arbitrary internal frame distortions, including internal rolling shutter correction, lens distortion correction, and crop movements natively done by the camera. Gyroflow reads the displacement mesh, reverses the deformation per-pixel, and maps the original pixels to its own computed stable mesh.

This method is used by Sony (Active mode)

#### C. MATRIX\_4X4

A standard 16-float row-major matrix representing an affine/perspective 3D transform applied by the camera to the frame. Works similarly to QUATERNION but allows the camera to pass internal scaling and translations directly.

***

## 8. Other

If your camera has any specific needs, we're open to extend the protobuf to include any additional fields or features.

If you have any questions, feel free to contact us at [devteam@gyroflow.xyz](mailto:devteam@gyroflow.xyz) or [adrian.eddy@gmail.com](mailto:adrian.eddy@gmail.com)

***

## Protobuf

{% code title="gyroflow.proto" lineNumbers="true" %}
```protobuf
syntax = "proto3";

// Main entry point of the data
// The first message will contain the Header with CameraMetadata and ClipMetadata
// All subsequent per-frame samples will contain the FrameMetadata, without Header
message Main {
    string magic_string     = 1; // Magic string useful for format detection in binary data. Always "GyroflowProtobuf"
    uint32 protocol_version = 2; // Version of the protocol, currently 1.

    Header        header = 3;
    FrameMetadata frame  = 4;
}

// One-time metadata containing information about the camera, lens and this particular video clip
message Header {
    message CameraMetadata {
                 string camera_brand         = 1; // Camera manufacturer
                 string camera_model         = 2; // Camera model
        optional string camera_serial_number = 3; // Camera serial number
        optional string firmware_version     = 4; // Camera firmware version
                 string lens_brand           = 5; // Lens manufacturer
                 string lens_model           = 6; // Lens model
                 uint32 pixel_pitch_nm       = 7; // Sensor pixel pitch in nanometers
                 uint32 sensor_pixel_width   = 8; // Full sensor width in pixels
                 uint32 sensor_pixel_height  = 9; // Full sensor height in pixels
        optional float  crop_factor          = 10; // Crop factor in relation to full frame sensor size. e.g. 1.6x for APS-C
        optional string lens_profile         = 11; // The Gyroflow lens identifier, or a path to lens profile json file (relative to the `camera_presets` directory), or the json contents directly
        optional string imu_orientation      = 12; // IMU orientation used by Gyroflow as XYZ, Xyz, Zyx etc. Defaults to "XYZ". Read more in the Gyroflow documentation about this orientation convention.
        optional Quaternion imu_rotation     = 13; // Arbitrary IMU rotation. Applies to the raw IMU samples (FrameMetadata.imu field).
        optional Quaternion quats_rotation   = 14; // Arbitrary IMU rotation. Applies to the quaternions after sensor fusion (FrameMetadata.quaternions field).
        optional string additional_data      = 15; // Optional note or additional data. If it starts with {, it will be parsed as JSON
    }
    message ClipMetadata {
        enum ReadoutDirection {
            TopToBottom = 0; // Sensor reads pixels from top to bottom.
            BottomToTop = 1; // Sensor reads pixels from bottom to top.
            RightToLeft = 2; // Sensor reads pixels from right to left.
            LeftToRight = 3; // Sensor reads pixels from left to right.
        }

        uint32 frame_width            = 1; // Video frame width in pixels
        uint32 frame_height           = 2; // Video frame height in pixels
        float  duration_us            = 3; // Clip duration in microseconds
        float  record_frame_rate      = 4; // Recording frame rate
        float  sensor_frame_rate      = 5; // Sensor frame rate. In most cases it will be equal to `record_frame_rate`
        float  file_frame_rate        = 6; // File frame rate. May be different in VFR mode. e.g. 120 fps recorded as 30 fps file
        int32  rotation_degrees       = 7; // Video rotation in degrees. For example 180 degrees for upside-down, or 90 for vertical mode.
        uint32 imu_sample_rate        = 8; // Sampling rate of the IMU chip.
        optional string color_profile = 9; // Shooting color profile, eg. Natural, Log, etc
        float  pixel_aspect_ratio     = 10; // For anamorphic lenses
        float  frame_readout_time_us  = 11; // Time it takes to read the video frame from the sensor, for rolling shutter correction. NOTE: It should be the time between first row of pixels to the last row of pixels, not for full sensor readout (if the crop is involved).
        ReadoutDirection frame_readout_direction = 12; // Frame readout direction
    }

    CameraMetadata camera = 1;
    ClipMetadata   clip   = 2;
}

message FrameMetadata {
    double start_timestamp_us = 1; // Frame capture start - the timestamp when the first row of pixels was captured. Internal camera clock timestamp. Unit: microseconds
    double end_timestamp_us   = 2; // Frame capture end - the timestamp when the last row of pixels was captured. Internal camera clock timestamp. Unit: microseconds
    uint32 frame_number       = 3; // Frame number in sequence. The first frame of the video clip should have this set to 1.

    optional uint32 iso                       = 4; // ISO Value
    optional float  exposure_time_us          = 5; // Actual exposure time in microseconds
    optional uint32 white_balance_kelvin      = 6; // White balance in kelvins
    optional float  white_balance_tint        = 7; // White balance tint value
    optional float  digital_zoom_ratio        = 8; // Digital zoom ratio. If the video is zoomed in digitally, this value should indicate that. E.g. 0.9 for 10% digital crop
    optional int32  shutter_speed_numerator   = 9; // Shutter speed numerator. E.g. 1 in case of 1/240 shutter speed.
    optional int32  shutter_speed_denumerator = 10; // Shutter speed denumerator. E.g. 240 in case of 1/240 shutter speed.
    optional float  shutter_angle_degrees     = 11; // Shutter angle in degrees. E.g. 180
    optional float  crop_x                    = 12; // Sensor crop area in pixels, X coordinate
    optional float  crop_y                    = 13; // Sensor crop area in pixels, Y coordinate
    optional float  crop_width                = 14; // Sensor crop area in pixels, width
    optional float  crop_height               = 15; // Sensor crop area in pixels, height

    repeated LensData       lens        = 16; // Per-frame lens information, like focal length, distortion coefficients etc
    repeated IMUData        imu         = 17; // Per-frame raw IMU data samples, will likely have multiple samples in one video frame
    repeated QuaternionData quaternions = 18; // Per-frame quaternion data. Optional, can contain camera orientation after sensor fusion
    repeated LensOISData    ois         = 19; // Per-frame Lens optical stabilization data. Not present when OIS is disabled.          ??? Exact data and format to be determined ???
    repeated IBISData       ibis        = 20; // Per-frame in-body image stabilization (IBIS) data. Not present when IBIS is disabled. ??? Exact data and format to be determined ???
    repeated EISData        eis         = 21; // Per-frame electronic in-camera stabilization data. Not present when EIS is disabled.  ??? Exact data and format to be determined ???
}

message LensData {
    enum DistortionModel {
        OpenCVFisheye  = 0; // OpenCV's fisheye model. More details: https://docs.opencv.org/4.x/db/d58/group__calib3d__fisheye.html
        OpenCVStandard = 1; // OpenCV's standard model. More details: https://docs.opencv.org/4.x/d9/d0c/group__calib3d.html
        Poly3          = 2; // LensFun's Poly3 model. More details: https://lensfun.github.io/manual/latest/group__Lens.html#gaa505e04666a189274ba66316697e308e
        Poly5          = 3; // LensFun's Poly5 model. More details: https://lensfun.github.io/manual/latest/group__Lens.html#gaa505e04666a189274ba66316697e308e
        PTLens         = 4; // LensFun's PTLens model. More details: https://lensfun.github.io/manual/latest/group__Lens.html#gaa505e04666a189274ba66316697e308e
        GenericPolynomial = 5; // ??? Not implemented yet. ???
    }
    DistortionModel distortion_model       = 1;
    repeated float distortion_coefficients = 2; // Distortion model coefficients as an array of float values.
    repeated float camera_intrinsic_matrix = 3; // Row-major 3x3 camera intrinsic matrix. Usually [[fx, 0, cx], [0, fy, cy], [0, 0, 1]], where fx and fy are focal length values in pixels (f_mm = f_pixels * sensor_width_mm / image_width_px ; f_pixels = f_mm / sensor_width_mm * image_width_px), and cx and cy is the principal point in pixels (usually width/2, height/2).
    optional float focal_length_mm         = 4; // Native lens focal length in mm
    optional float f_number                = 5; // Lens aperture number. E.g. 2.8
    optional float focus_distance_mm       = 6; // Focal plane distance in millimeters
}

message IMUData {
    double sample_timestamp_us    = 1;  // Exact timestamp of the sampling time from the internal camera clock. Unit: microseconds
    float gyroscope_x             = 2;  // Gyroscope X reading. Unit: degrees/sec
    float gyroscope_y             = 3;  // Gyroscope Y reading. Unit: degrees/sec
    float gyroscope_z             = 4;  // Gyroscope Z reading. Unit: degrees/sec
    float accelerometer_x         = 5;  // Accelerometer X reading. Unit: m/s²
    float accelerometer_y         = 6;  // Accelerometer Y reading. Unit: m/s²
    float accelerometer_z         = 7;  // Accelerometer Z reading. Unit: m/s²
    optional float magnetometer_x = 8;  // Magnetometer X reading. Unit: µT
    optional float magnetometer_y = 9;  // Magnetometer Y reading. Unit: µT
    optional float magnetometer_z = 10; // Magnetometer Z reading. Unit: µT
}

message Quaternion {
    float w = 1; // Quaternion component W (angle)
    float x = 2; // Quaternion component X
    float y = 3; // Quaternion component Y
    float z = 4; // Quaternion component Z
}

message QuaternionData {
    double sample_timestamp_us = 1; // Exact timestamp of the sampling time from the internal camera clock. Unit: microseconds
    Quaternion quat = 2; // Quaternion
}

message LensOISData {
    double sample_timestamp_us = 1; // Exact timestamp of the sampling time from the internal camera clock. Unit: microseconds
    float x = 2; // Optical element shift value in the X axis in nanometers
    float y = 3; // Optical element shift value in the Y axis in nanometers
}

message IBISData {
    double sample_timestamp_us = 1; // Exact timestamp of the sampling time from the internal camera clock. Unit: microseconds
    float shift_x = 2; // X Sensor shift value in nanometers
    float shift_y = 3; // Y Sensor shift value in nanometers
    float roll_angle_degrees = 4; // Sensor roll rotation angle in degrees.
}

message EISData {
    enum EISDataType {
        QUATERNION = 0; // Rotation only, indicates how the frame was rotated internally by the camera EIS, from pixels read from the sensor to the final pixels in the encoded video file.
        MESH_WARP  = 1; // Mesh warp. Allows for arbitrary mapping of the video frame. Contains exact transform/deform of the video frame read from the sensor to the final pixels in the encoded video file.
        MATRIX_4X4 = 2; // 4x4 matrix - rotation, translation and scaling. Indicates how the frame was transformed in the 3d space by the camera EIS, from pixels read from the sensor to the final pixels in the encoded video file.
    }
    optional double sample_timestamp_us = 1; // Exact timestamp of the sampling time from the internal camera clock. Unit: microseconds. Timestamp is ignored if there's only one entry of EISData per frame.
    EISDataType type                    = 2; // Type of EIS. Can be quaternion, mesh warp or 4x4 transform matrix.
    optional Quaternion quaternion      = 3; // If type is QUATERNION, this field contains the quaternion data
    optional MeshWarpData mesh_warp     = 4; // If type is MESH_WARP, this field contains the mesh values
    repeated float matrix_4x4           = 5; // If type is MATRIX_4x4, this field contains the 16 float matrix values (row-major order).
}

message MeshWarpData {
    int32 grid_width  = 1; // Number of video frame divisions in the horizontal direction.
    int32 grid_height = 2; // Number of video frame divisions in the vertical direction.
    repeated float values = 3; // grid_width * grid_height float numbers representing new position of a coordinate at X and Y grid position.
}
```
{% endcode %}
