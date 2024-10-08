---
description: >-
  Gyroflow protobuf format is designed to contain even the most advanced and
  detailed data about the video capture pipeline, that is useful for
  post-stabilization.
---

# 🗄️ Gyroflow protobuf

## Download the .proto definition

You can download the latest protobuf definition from telemetry-parser's git repository.

**`TODO:`**` ``add info`

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

## Transport format

Typically the protobuf will be stored as binary data in a separate MP4 track of the video file. This makes it easy to read and write and it's the standard way to embed additional data in video files.

**`TODO:`**` ``examples and screenshots`

## Sample files

**`TODO:`**` ``add sample files`

## Technical details

Most fields in the **gyroflow.proto** have comments so the file should be self-explanatory. However, here are some additional details about more advanced features:

### Frame capture pipeline

**`TODO:`**` ``diagram and description`

**`TODO:`**` ``timestamp_offset_us`

### Lens OIS

**`TODO:`**` ``diagram with the typical design and the values we can use`

### IBIS

**`TODO:`**` ``diagram with the typical design and the values we can use`

### EIS

**`TODO:`**` ``possible types and diagrams with examples`

#### Quaternion-based EIS

**`TODO:`**` ``add info`

#### Mesh warp-based EIS

**`TODO:`**` ``add info`

#### 4x4 matrix based EIS

**`TODO:`**` ``add info`



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

    repeated LensData       lens        = 12; // Per-frame lens information, like focal length, distortion coefficients etc
    repeated IMUData        imu         = 13; // Per-frame raw IMU data samples, will likely have multiple samples in one video frame
    repeated QuaternionData quaternions = 14; // Per-frame quaternion data. Optional, can contain camera orientation after sensor fusion
    repeated LensOISData    ois         = 15; // Per-frame Lens optical stabilization data. Not present when OIS is disabled.          ??? Exact data and format to be determined ???
    repeated IBISData       ibis        = 16; // Per-frame in-body image stabilization (IBIS) data. Not present when IBIS is disabled. ??? Exact data and format to be determined ???
    repeated EISData        eis         = 17; // Per-frame electronic in-camera stabilization data. Not present when EIS is disabled.  ??? Exact data and format to be determined ???
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
    float focal_length_mm                  = 4; // Native lens focal length in mm
    float f_number                         = 5; // Lens aperture number. E.g. 2.8
    float focus_distance_mm                = 6; // Focal plane distance in millimeters
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
    float x = 3; // Optical element shift value in the X axis in nanometers
    float y = 4; // Optical element shift value in the Y axis in nanometers
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
    EISDataType type          = 2; // Type of EIS. Can be quaternion, mesh warp or 4x4 transform matrix.
    Quaternion quaternion     = 3; // If type is QUATERNION, this field contains the quaternion data
    MeshWarpData mesh_warp    = 4; // If type is MESH_WARP, this field contains the mesh values
    repeated float matrix_4x4 = 5; // If type is MATRIX_4x4, this field contains the 16 float matrix values (row-major order).
}

message MeshWarpData {
    int32 grid_width  = 1; // Number of video frame divisions in the horizontal direction.
    int32 grid_height = 2; // Number of video frame divisions in the vertical direction.
    repeated float values = 3; // grid_width * grid_height float numbers representing new position of a coordinate at X and Y grid position.
}
```
{% endcode %}
