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
                 uint32 pixel_pitch_x_nm     = 7; // Sensor pixel pitch in nanometers, horizontal direction.
                 uint32 pixel_pitch_y_nm     = 8; // Sensor pixel pitch in nanometers, vertical direction. Required — for square-pixel sensors, set equal to pixel_pitch_x_nm.
                 uint32 sensor_pixel_width   = 9; // Full sensor width in pixels
                 uint32 sensor_pixel_height  = 10; // Full sensor height in pixels
        optional float  crop_factor          = 11; // Crop factor in relation to full frame sensor size. e.g. 1.6x for APS-C
        optional string lens_profile         = 12; // The Gyroflow lens identifier, or a path to lens profile json file (relative to the `camera_presets` directory), or the json contents directly
        // Axis-permutation-and-sign string describing how IMU axes are labeled
        // relative to the camera body. Three characters, each one of
        // {X, Y, Z, x, y, z} where capital = positive axis and lowercase =
        // negated axis. Default "XYZ" = identity (IMU axes match camera body).
        // Example: "Xyz" means X stays, Y and Z are flipped. The transform is
        // applied to each raw IMU 3-vector as a pure permutation/sign before
        // any sensor fusion.
        optional string imu_orientation      = 13;
        // Additional rigid rotation that aligns the IMU sensor frame with the
        // camera body frame. Applied to each raw IMU 3-vector v as
        //     v' = imu_rotation · v
        // (standard vector rotation). Applied AFTER imu_orientation when both
        // are present (orientation first as axis remap, then rotation).
        optional Quaternion imu_rotation     = 14;
        // Additional rigid rotation that aligns the post-fusion quaternion
        // stream with the camera body frame. Applied to each quaternion q in
        // FrameMetadata.quaternions as CONJUGATION (not composition):
        //     q' = quats_rotation · q · quats_rotation⁻¹
        // The effect is to rotate the AXIS of q by quats_rotation while
        // preserving its angle — i.e. to re-express the same physical rotation
        // in a rotated coordinate frame. Use this to correct a misaligned
        // fused-orientation stream without disturbing the IMU pipeline.
        // imu_rotation and quats_rotation are independent: they apply to
        // different streams (raw IMU vs. fused quaternions). Most producers
        // need only one of them.
        optional Quaternion quats_rotation   = 15;
        optional string additional_data      = 16; // Optional note or additional data. If it starts with {, it will be parsed as JSON
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
        // Clip duration in microseconds. MUST be double — a 32-bit float loses
        // µs precision beyond ~16.7 s (mantissa = 24 bits); at 10 min the step
        // grows to ~64 µs which causes audible jitter in any time-domain math.
        double duration_us            = 3;
        float  record_frame_rate      = 4; // Recording frame rate
        float  sensor_frame_rate      = 5; // Sensor frame rate. In most cases it will be equal to `record_frame_rate`
        float  file_frame_rate        = 6; // File frame rate. May be different in VFR mode. e.g. 120 fps recorded as 30 fps file
        int32  rotation_degrees       = 7; // Video rotation in degrees. For example 180 degrees for upside-down, or 90 for vertical mode.
        uint32 imu_sample_rate        = 8; // Sampling rate of the IMU chip.
        optional string color_profile = 9; // Shooting color profile, eg. Natural, Log, etc
        float  pixel_aspect_ratio     = 10; // For anamorphic lenses
        // Time it takes to read the video frame from the sensor, for rolling
        // shutter correction. It is the time between the first row of pixels
        // and the last row of pixels of the CROP area — not the full sensor
        // readout. Stored as double for consistency with the per-frame double-
        // precision start_timestamp_us / end_timestamp_us against which it is
        // compared and arithmetically combined.
        double frame_readout_time_us  = 11;
        ReadoutDirection frame_readout_direction = 12; // Frame readout direction
    }

    CameraMetadata camera = 1;
    ClipMetadata   clip   = 2;
}

message FrameMetadata {
    // Time, in microseconds on the camera's internal clock, at the READOUT INSTANT
    // of the FIRST-read sensor row — i.e. the moment that row finished integrating
    // photons and was latched out by the sense amplifier / ADC. NOT the moment that
    // row started exposing; NOT the midpoint of its exposure.
    //
    // "First-read" follows ClipMetadata.frame_readout_direction:
    //   TopToBottom → top row     (visual row 0)
    //   BottomToTop → bottom row  (visual row frame_height - 1)
    //   LeftToRight → left column (visual column 0)
    //   RightToLeft → right column
    //
    // Rationale for the readout-latch convention: hardware naturally timestamps the
    // readout latch event, since that is the observable moment for the readout chain.
    // The exposure midpoint (the canonical per-row time for stabilization) is derived
    // by subtracting exposure_time_us / 2.
    //
    // PER-ROW INTERPOLATION: for per-row stabilization, consumers SHOULD linearly
    // interpolate per-row time between start_timestamp_us and end_timestamp_us
    // (NOT derive it from start + frame_readout_time_us, because the two timestamp
    // fields are doubles and authoritative, while frame_readout_time_us is a float
    // helper). For an output row index r in readout order (r = 0 for the first-read
    // row, r = N-1 for the last-read row, where N = ClipMetadata.frame_height for
    // vertical readout or ClipMetadata.frame_width for horizontal readout):
    //
    //     row_readout_instant(r) = start_timestamp_us
    //                            + (r / (N - 1)) · (end_timestamp_us - start_timestamp_us)
    //     row_exposure_midpoint(r) = row_readout_instant(r) - exposure_time_us / 2
    //     frame_center_of_capture  = (start_timestamp_us + end_timestamp_us) / 2
    //                              - exposure_time_us / 2
    //
    // For consumers indexing by visual (top-down) row v, convert to readout-order r
    // using frame_readout_direction (e.g. for BottomToTop: r = (N - 1) - v).
    //
    // Producers whose hardware reports a different physical event (start of row 0
    // integration, or the row 0 exposure midpoint) must shift by ±exposure_time_us / 2
    // before writing this field.
    double start_timestamp_us = 1;

    // Time, in microseconds on the camera's internal clock, at the readout instant
    // of the LAST-read sensor row (per frame_readout_direction). Together with
    // start_timestamp_us this defines the rolling-shutter timeline of the frame.
    //
    // AUTHORITATIVE for per-row interpolation. The relation
    //     end_timestamp_us ≈ start_timestamp_us + frame_readout_time_us
    // holds approximately, but the two timestamp fields are doubles measured directly
    // by the camera clock and take precedence over the float helper
    // frame_readout_time_us if the two ever disagree.
    double end_timestamp_us   = 2;

    uint32 frame_number       = 3; // Frame number in sequence. The first frame of the video clip should have this set to 1.

    optional uint32 iso                       = 4; // ISO Value
    // Actual exposure time in microseconds. Stored as double — float loses µs
    // precision beyond ~16.7 s, which matters for astro / long-exposure work.
    optional double exposure_time_us          = 5;
    optional uint32 white_balance_kelvin      = 6; // White balance in kelvins
    optional float  white_balance_tint        = 7; // White balance tint value
    optional float  digital_zoom_ratio        = 8; // Digital zoom ratio. If the video is zoomed in digitally, this value should indicate that. E.g. 0.9 for 10% digital crop
    // EXPOSURE PRECEDENCE: if more than one of exposure_time_us /
    // shutter_speed_{numerator,denominator} / shutter_angle_degrees is present,
    // consumers MUST use them in this order (highest precedence first):
    //   1. exposure_time_us (microsecond-precise; most authoritative)
    //   2. shutter_speed_numerator / shutter_speed_denominator (exact rational)
    //   3. shutter_angle_degrees (derived from frame rate + angle, as
    //      exposure_time_us = shutter_angle_degrees / 360 · 1e6
    //                       / ClipMetadata.sensor_frame_rate).
    //      Use sensor_frame_rate — not record_frame_rate or file_frame_rate —
    //      because the shutter angle is a property of the sensor's exposure
    //      cycle: the rate at which the rolling-shutter sweep physically
    //      repeats. For the typical case where sensor_frame_rate ==
    //      record_frame_rate this is a no-op; for slow-motion / high-frame-
    //      rate modes where they differ, sensor_frame_rate is the correct
    //      divisor.
    // Producers SHOULD set the most precise field they have and MAY omit the
    // less-precise ones.
    optional int32  shutter_speed_numerator   = 9;  // Shutter speed numerator. E.g. 1 in case of 1/240 shutter speed.
    optional int32  shutter_speed_denominator = 10; // Shutter speed denominator. E.g. 240 in case of 1/240 shutter speed.
    optional float  shutter_angle_degrees     = 11; // Shutter angle in degrees. E.g. 180
    optional float  crop_x                    = 12; // Sensor crop area in pixels, X coordinate
    optional float  crop_y                    = 13; // Sensor crop area in pixels, Y coordinate
    optional float  crop_width                = 14; // Sensor crop area in pixels, width
    optional float  crop_height               = 15; // Sensor crop area in pixels, height

    repeated LensData       lens        = 16; // Per-frame lens information, like focal length, distortion coefficients etc
    repeated IMUData        imu         = 17; // Per-frame raw IMU data samples, will likely have multiple samples in one video frame
    repeated QuaternionData quaternions = 18; // Per-frame quaternion data. Optional, can contain camera orientation after sensor fusion
    // Per-frame lens optical stabilization data. See LensOISData for units, sign
    // convention (unified with IBISData), and coordinate frame.
    //
    // SAMPLE-COVERAGE REQUIREMENT: for per-row rolling-shutter correction, the sample
    // timestamps must cover the row-exposure-midpoint span of this frame, i.e. at minimum
    //     [ start_timestamp_us - exposure_time_us / 2,
    //       end_timestamp_us   - exposure_time_us / 2 ]
    // plus one or two guard samples on each side for spline tangents. A typical
    // cadence is 4–8 samples per frame plus guards. A single sample per frame degenerates
    // to per-frame correction (no rolling-shutter compensation).
    //
    // Not present when OIS is disabled.
    repeated LensOISData    ois         = 19;

    // Per-frame in-body image stabilization data. See IBISData for units, sign convention,
    // and coordinate frame. Same sample-coverage requirement as `ois`. Not present when
    // IBIS is disabled.
    repeated IBISData       ibis        = 20;
    // Per-frame electronic in-camera stabilization data. Describes transforms the
    // camera applied between sensor read-out and encoded pixels. See EISData for
    // the per-variant semantics. Not present when in-camera EIS is disabled.
    repeated EISData        eis         = 21;

    // Per-frame GPS / GNSS position samples. See GPSData for layout, units, and
    // timing conventions. Sample cadence is producer-dependent (typically 1 Hz
    // for generic GNSS receivers, up to ~10–18 Hz for high-rate receivers).
    // Empty when GPS is disabled, unavailable, or simply hasn't ticked during
    // this frame's interval — most video frames at typical 1 Hz GPS rates carry
    // no entries.
    repeated GPSData        gps         = 22;
}

// Per-frame lens information: intrinsic matrix, focal length, distortion model.
//
// The lens distortion model is encoded as a `oneof distortion` variant. Exactly one
// variant is present per LensData entry. Each variant message defines its own
// coefficient layout and projection math.
//
// MULTIPLE ENTRIES PER FRAME:
//   FrameMetadata.lens is `repeated`. Multiple entries per frame are permitted to
//   handle intra-frame changes (e.g. zoom or focus actuator moving during readout,
//   per-row intrinsics for rolling-shutter-corrected anamorphic). Each entry
//   carries its own sample_timestamp_us on the same camera clock as
//   FrameMetadata.start_timestamp_us, and consumers interpolate between samples
//   by timestamp using the same row-midpoint convention used for ois / ibis.
//   When only one entry exists, the lens parameters apply uniformly to the whole
//   frame and the timestamp may be omitted.
message LensData {
    // Sample timestamp on the same camera clock as FrameMetadata.start_timestamp_us.
    // Unit: microseconds. Optional; when omitted (single-entry case), the entry
    // applies to the whole frame.
    optional double sample_timestamp_us = 12;

    // Row-major 3x3 camera intrinsic matrix. Usually [[fx, 0, cx], [0, fy, cy], [0, 0, 1]],
    // where fx and fy are focal length values in pixels
    //   (f_mm = f_pixels * sensor_width_mm / image_width_px ;
    //    f_pixels = f_mm / sensor_width_mm * image_width_px),
    // and cx, cy is the principal point in pixels (usually width/2, height/2).
    repeated float camera_intrinsic_matrix = 1;

    // Native lens focal length in millimeters.
    //
    // REQUIRED when `distortion` is GenericPolynomial (the polynomial coefficients
    // are normalized assuming this focal length; see GenericPolynomial message docs).
    //
    // RELATIONSHIP TO camera_intrinsic_matrix:
    //   focal_length_mm is the physical lens focal length and is resolution-INDEPENDENT.
    //   camera_intrinsic_matrix carries f_x / f_y in OUTPUT-PIXEL units, which IS
    //   resolution-dependent. They are related (but not equivalent) by
    //       f_x_pixels = focal_length_mm · (output_width  / sensor_width_used_mm)
    //   where sensor_width_used_mm = pixel_pitch_x_nm · crop_width / 1e6.
    //
    // PRECEDENCE for projection: camera_intrinsic_matrix is the source of truth for
    // sensor → output-pixel mapping. focal_length_mm is the source of truth for
    // anything that needs physical-lens reasoning (e.g. GenericPolynomial
    // normalization, FOV calculation, sensor geometry). When a producer sets both,
    // they MUST be self-consistent under the formula above.
    optional float focal_length_mm   = 2;
    optional float f_number          = 3; // Lens aperture number. E.g. 2.8
    optional float focus_distance_mm = 4; // Focal plane distance in millimeters

    // The distortion model. Exactly one variant is present.
    oneof distortion {
        NoDistortion       no_distortion       = 5;
        OpenCVFisheye      opencv_fisheye      = 6;
        OpenCVStandard     opencv_standard     = 7;
        LensFunPoly3       lensfun_poly3       = 8;
        LensFunPoly5       lensfun_poly5       = 9;
        LensFunPTLens      lensfun_ptlens      = 10;
        GenericPolynomial  generic_polynomial  = 11;
    }
}

// Pinhole (rectilinear) projection — the encoded pixels are already linearized
// so that r = f · tan(θ). Producers that fully correct lens distortion in-camera
// SHOULD emit this variant rather than fitting a polynomial; the consumer treats
// the image as pinhole with no further geometric remap. Mathematically
// equivalent to GenericPolynomial with coefficients [1, 0, 1/3, 0, 2/15, 0]
// (the Taylor expansion of tan) but simpler and faster on the consumer side —
// no Newton iteration, no per-pixel polynomial evaluation.
//
// For producers that apply PARTIAL in-camera correction (residual barrel /
// pincushion remains in the encoded image), use GenericPolynomial refit
// against the post-correction projection instead.
message NoDistortion {}

// OpenCV's fisheye distortion model.
// Reference: https://docs.opencv.org/4.x/db/d58/group__calib3d__fisheye.html
//
// Projection (forward):
//   θ        = angle from optical axis (radians)
//   θ_d      = θ · (1 + k₁·θ² + k₂·θ⁴ + k₃·θ⁶ + k₄·θ⁸)
//   r_pixels = θ_d · f_px        (f_px from camera_intrinsic_matrix)
//
// Coefficients: [k₁, k₂, k₃, k₄] — exactly 4 floats.
message OpenCVFisheye {
    repeated float coefficients = 1;
}

// OpenCV's standard radial + tangential distortion model.
// Reference: https://docs.opencv.org/4.x/d9/d0c/group__calib3d.html
//
// Coefficients (variable length, following OpenCV conventions):
//   4 elements:  [k₁, k₂, p₁, p₂]
//   5 elements:  [k₁, k₂, p₁, p₂, k₃]
//   8 elements:  [k₁, k₂, p₁, p₂, k₃, k₄, k₅, k₆]
//   12 elements: [k₁..k₆, p₁, p₂, s₁..s₄]                  (rational+thin-prism)
//   14 elements: [k₁..k₆, p₁, p₂, s₁..s₄, τ_x, τ_y]        (tilt)
message OpenCVStandard {
    repeated float coefficients = 1;
}

// LensFun's Poly3 radial distortion model.
// Reference: https://lensfun.github.io/manual/latest/group__Lens.html#gaa505e04666a189274ba66316697e308e
//   r_d = r · (1 + k₁ · r²)
// Coefficients: [k₁] — 1 float.
message LensFunPoly3 {
    repeated float coefficients = 1;
}

// LensFun's Poly5 radial distortion model.
//   r_d = r · (1 + k₁·r² + k₂·r⁴)
// Coefficients: [k₁, k₂] — 2 floats.
message LensFunPoly5 {
    repeated float coefficients = 1;
}

// LensFun's PTLens distortion model.
//   r_d = r · (a·r³ + b·r² + c·r + 1)
// Coefficients: [a, b, c] — 3 floats.
message LensFunPTLens {
    repeated float coefficients = 1;
}

// Generic polynomial fisheye projection — a clean generalization of OpenCVFisheye
// with full power range and dimensionless normalization.
//
// SEMANTICS — WHAT THE POLYNOMIAL DESCRIBES
//   coefficients describe the projection of the ENCODED PIXELS as they appear in
//   the recorded image — NOT the raw lens optics. If the producer applies any
//   in-camera distortion correction, geometric crop, anamorphic desqueeze, or
//   any other geometric remap between sensor read-out and the encoded pixels,
//   the polynomial MUST be refit against the resulting (post-correction)
//   projection. Concretely:
//
//     - Producer applies NO in-camera correction → polynomial = raw lens projection
//     - Producer applies in-camera correction toward pinhole → polynomial converges
//       toward the rectilinear Taylor expansion [1, 0, 1/3, 0, 2/15, 0, ...]
//     - Producer fully linearizes to pinhole → emit NoDistortion instead
//
//   The consumer ALWAYS undistorts using these coefficients without needing to
//   know whether in-camera correction was applied. The polynomial alone encodes
//   the projection state.
//
// MATHEMATICAL FORM
//   Maps the ray angle θ from the optical axis (radians) to a DIMENSIONLESS
//   normalized projected radius on the image plane:
//
//       r_normalized = Σᵢ  coefficients[i] · θ^(i+1)
//                    = c₀·θ + c₁·θ² + c₂·θ³ + ... + c_{N-1}·θ^N
//
//   The polynomial is intrinsically radial — a function of θ only. To project
//   to pixels relative to the principal point, scale by each axis's pixel
//   focal length INDEPENDENTLY (this matters for non-square pixels and any
//   anamorphic case where f_x ≠ f_y):
//
//       f_x = camera_intrinsic_matrix[0, 0]   (pixel focal length, X axis)
//       f_y = camera_intrinsic_matrix[1, 1]   (pixel focal length, Y axis)
//       c_x = camera_intrinsic_matrix[0, 2]   (principal point X)
//       c_y = camera_intrinsic_matrix[1, 2]   (principal point Y)
//
//       r_pixels_x = r_normalized · f_x
//       r_pixels_y = r_normalized · f_y
//       u          = unit_direction.x · r_pixels_x + c_x
//       v          = unit_direction.y · r_pixels_y + c_y
//
//   where unit_direction is the unit vector pointing from the optical axis to
//   the projected ray in the image plane (cos φ, sin φ for azimuth φ).
//
// COEFFICIENT ORDER
//   coefficients[i] is the coefficient of θ^(i+1):
//     index 0 → θ¹      (the linear/leading term)
//     index 1 → θ²
//     index 2 → θ³
//     ...
//   Both odd and even powers are allowed. This is the only structural
//   difference from OpenCVFisheye, which restricts to θ³, θ⁵, θ⁷, θ⁹
//   with an implicit leading θ¹ coefficient of 1.0 — i.e. OpenCVFisheye can
//   be expressed in this model as [1, 0, k₁, 0, k₂, 0, k₃, 0, k₄].
//
// STRICT NORMALIZATION REQUIREMENTS
//   This model uses STRICT semantics:
//
//   1. LensData.focal_length_mm MUST be set on the parent LensData entry. The
//      polynomial coefficients are normalized assuming a specific physical
//      focal length; consumers that need to relate to physical sensor
//      geometry require this value.
//
//   2. The polynomial MUST be normalized so that c₀ ≈ 1.0 for a paraxial
//      (small-θ) ray. Concretely, if a calibration produces coefficients
//      in meters/radian^n, divide all coefficients by the focal length in
//      meters before writing them here. Consumers MAY reject coefficients
//      whose c₀ is outside a reasonable tolerance (e.g. [0.8, 1.2]) as
//      malformed.
//
//   3. Reference projections under this normalization:
//        Equidistant fisheye:  r_normalized = θ            → [1, 0, 0, ...]
//        Rectilinear pinhole:  r_normalized = tan(θ)       → [1, 0, 1/3, 0, 2/15, ...]
//        Stereographic:        r_normalized = 2·tan(θ/2)   → [1, 0, 1/12, 0, 1/120, ...]
//
// NUMBER OF COEFFICIENTS
//   Variable. Typical ranges:
//     - 4 terms: mild fisheye, narrow-to-medium FOV
//     - 6 terms: wide-angle / fisheye
//     - 8+ terms: extreme wide-angle or non-symmetric optics
//   Producers SHOULD report only as many terms as the calibration
//   actually fitted; do not pad with trailing zeros.
//
// INVERSION
//   Forward (θ → r_normalized) is closed-form polynomial evaluation.
//   Inverse (r_normalized → θ) requires numerical iteration; Newton's
//   method on the polynomial converges in <10 iterations for any
//   physically reasonable lens.
message GenericPolynomial {
    repeated float coefficients = 1;
}

message IMUData {
    // Sample timestamp on the same camera clock as FrameMetadata.start_timestamp_us.
    // Unit: microseconds. Optional; when omitted (single-entry case), the sample
    // applies to the whole frame. For multi-sample entries (the normal case for
    // IMU at 200–1000 Hz) this MUST be set on every sample.
    optional double sample_timestamp_us = 1;
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

// Unit quaternion (w + xi + yj + zk) using the HAMILTON convention:
//   - Right-handed coordinate system.
//   - Multiplication: i·j = k, j·k = i, k·i = j, and i² = j² = k² = ijk = -1.
//     This is NOT the JPL convention (which uses i·j = -k) found in some IMU
//     vendor literature; producers that natively use JPL must convert before
//     writing here.
//   - The four components MUST satisfy w² + x² + y² + z² = 1 (within float
//     precision); consumers MAY renormalize on read.
//   - Storage order is (w, x, y, z) as the four float fields below; this is
//     independent of the multiplication convention but listed for clarity.
message Quaternion {
    float w = 1; // Quaternion component W (real / scalar part)
    float x = 2; // Quaternion component X (i-axis imaginary part)
    float y = 3; // Quaternion component Y (j-axis imaginary part)
    float z = 4; // Quaternion component Z (k-axis imaginary part)
}

// Per-sample camera orientation as a quaternion.
//
// This is the camera's orientation in some inertial / world frame, derived from
// sensor fusion of raw IMU samples — the same role as the output of a complementary
// or Kalman filter, or the camera's own onboard sensor fusion. Producers should fill
// this when the camera reports a fused orientation. When only raw IMU is available,
// leave this field empty and the consumer will integrate from `FrameMetadata.imu`.
//
// For cameras that emit TWO related quaternion streams (a body-orientation stream and
// an image-orientation in-camera transform), this field carries the COMBINED product
// (body_orientation · image_orientation). The image-orientation transform alone, if
// separately useful (e.g. for re-expressing gravity in the encoded-frame coordinate
// system, or for horizon-lock integration), is reported under EISData.data.quaternion
// — see EISData docs.
//
// Sample cadence: typically denser than one per frame (commonly one quaternion per
// gyro sample, ~200 Hz). The consumer interpolates between samples by timestamp.
message QuaternionData {
    // Sample timestamp on the same camera clock as FrameMetadata.start_timestamp_us.
    // Unit: microseconds. Optional; when omitted (single-entry case), the sample
    // applies to the whole frame. For multi-sample entries this MUST be set on
    // every sample.
    optional double sample_timestamp_us = 1;
    Quaternion quat = 2;
}

// UNIFIED STABILIZER SIGN CONVENTION (applies to both LensOISData and IBISData)
//
// Both fields report the DISPLACEMENT OF IMAGE CONTENT on the sensor caused by
// the stabilizer at this sample's time, in the sensor-native pixel coordinate
// frame (+X right, +Y down, origin at the top-left of the sensor active area;
// axes do NOT rotate with ClipMetadata.rotation_degrees).
//
// Consumer formula is the same for both:
//
//       uv_source = uv_output + ois_shift_pixels + ibis_shift_pixels
//
// where each per-axis pixel shift is derived as
//
//       shift_pixels_x = shift_x_nm · frame_width  / (crop_width  · pixel_pitch_x_nm)
//       shift_pixels_y = shift_y_nm · frame_height / (crop_height · pixel_pitch_y_nm)
//
// PRODUCER SIGN RULES (because OIS and IBIS physically move different things):
//   - OIS: the lens group moves the IMAGE. If the image content shifted by +N nm
//     on the sensor, write +N.
//   - IBIS: the SENSOR moves. If the sensor displaced by +N nm in some direction,
//     the image content effectively appears at -N nm in the sensor-local frame.
//     Encoders MUST flip the sign before writing IBIS shifts here: write -N when
//     the sensor moved +N.
//
// The reported value is the EFFECTIVE IMAGE-PLANE DISPLACEMENT at the sensor
// surface, in nanometers. For OIS specifically, this is NOT the mechanical
// position of the IS lens group: manufacturers whose IS group has image-plane
// magnification β ≠ 1 must multiply the actuator displacement by β before
// reporting it here, so the single pixel-conversion formula above applies
// uniformly regardless of lens design.

message LensOISData {
    // Sample timestamp on the same camera clock as FrameMetadata.start_timestamp_us.
    // Unit: microseconds. Optional; when omitted (single-entry case), the sample
    // applies to the whole frame. For multi-sample entries this MUST be set on
    // every sample.
    optional double sample_timestamp_us = 1;
    // Image-plane displacement caused by OIS, in nanometers, in sensor-native
    // coordinates. Positive = content moved in +X direction.
    float shift_x_nm = 2;
    // Same, Y axis.
    float shift_y_nm = 3;
}

message IBISData {
    // Sample timestamp on the same camera clock as FrameMetadata.start_timestamp_us.
    // Unit: microseconds. Optional; when omitted (single-entry case), the sample
    // applies to the whole frame. For multi-sample entries this MUST be set on
    // every sample.
    optional double sample_timestamp_us = 1;
    // Image-plane displacement caused by IBIS, in nanometers, in sensor-native
    // coordinates. Positive = content moved in +X direction. NOTE: this is the
    // displacement of the IMAGE CONTENT, not the sensor — i.e. it's the negative
    // of the sensor's mechanical displacement. See the unified-sign-convention
    // block above LensOISData for the producer rule.
    float shift_x_nm = 2;
    // Same, Y axis.
    float shift_y_nm = 3;
    // Sensor rotation about the optical axis (Z), in degrees, pivoting about
    // the principal point. Positive = counterclockwise viewed from the lens
    // toward the sensor.
    //
    // PIVOT COORDINATE FRAME: the rotation is applied in sensor-native pixel
    // coordinates (the same frame as shift_x_nm / shift_y_nm — origin at the
    // top-left of the sensor active area, +X right, +Y down). The pivot
    // therefore must be expressed in the same frame, NOT in output-pixel
    // coordinates. Convert the principal point from camera_intrinsic_matrix
    // (which is in output-pixel units, per LensData docs) using the per-frame
    // crop:
    //
    //     pivot_sensor_px_x = FrameMetadata.crop_x
    //                       + (camera_intrinsic_matrix[0, 2] / frame_width)
    //                         · FrameMetadata.crop_width
    //     pivot_sensor_px_y = FrameMetadata.crop_y
    //                       + (camera_intrinsic_matrix[1, 2] / frame_height)
    //                         · FrameMetadata.crop_height
    //
    // (where frame_width/frame_height are ClipMetadata.frame_{width,height}).
    // Producers that don't expose camera_intrinsic_matrix should approximate
    // the pivot as the center of the per-frame crop rectangle.
    //
    // PHYSICAL MODEL: the IBIS mechanism has exactly three mechanical degrees
    // of freedom — translation X, translation Y, and this rotation about the
    // optical axis. The sensor does not pitch or yaw (physically tilting the
    // sensor would defocus the image). The X/Y shift pair captures ALL linear
    // corrective output, including correction for camera yaw/pitch shake (both
    // manifest at the sensor as horizontal/vertical image translation by
    // approximately f·tan(θ)).
    float roll_angle_degrees = 4;
}

// Per-sample electronic-image-stabilization data — describes a transform the camera
// applied INTERNALLY between sensor read-out and the encoded pixels. The consumer
// applies the inverse to recover the sensor-native frame before doing its own
// stabilization on top.
//
// Distinct from QuaternionData (which carries camera ORIENTATION in world frame,
// not an in-camera transform applied to pixels).
//
// VARIANTS:
//
//   - `quaternion`: a rotation the camera applied to the captured pixels before
//     encoding (e.g. the image-orientation half of a body-orientation +
//     image-orientation quaternion pair, used to re-express data into the encoded
//     frame and for horizon-lock integration).
//
//   - `mesh_warp`: a 2D mesh describing a spatial warp the camera applied as part
//     of in-camera electronic stabilization. The consumer numerically inverts the
//     mesh to recover the sensor-native frame. See MeshWarpData for layout.
//
//   - `matrix_4x4`: a 4×4 affine transform the camera applied. Preliminary — no
//     grounded producer yet.
//
// MULTIPLE ENTRIES PER FRAME:
//   Multiple EISData samples per frame are permitted; use sample_timestamp_us for
//   per-sample timing (aligned with the row-midpoint timeline for rolling shutter).
//   When only one entry exists, the transform applies uniformly to the whole frame
//   and the timestamp may be omitted.
message EISData {
    // Sample timestamp on the same camera clock as FrameMetadata.start_timestamp_us.
    // Unit: microseconds. Optional; when omitted (single-entry case), the transform
    // applies to the whole frame.
    optional double sample_timestamp_us = 1;

    // Exactly one variant is present. Each describes a different kind of in-camera
    // applied transform.
    oneof data {
        // Rotation the camera applied to the captured pixels before encoding.
        // Consumer applies the inverse to undo it.
        Quaternion   quaternion = 2;

        // 2D mesh warp the camera applied. See MeshWarpData.
        MeshWarpData mesh_warp  = 3;

        // 4×4 affine transform the camera applied. Preliminary — no grounded
        // producer yet.
        Matrix4x4    matrix_4x4 = 4;
    }
}

// 4×4 affine transform. Row-major: values[0..4] = first row, etc.
message Matrix4x4 {
    repeated float values = 1; // 16 floats, row-major
}

// 2D mesh describing an in-camera spatial warp the camera applied between sensor
// read-out and the encoded frame. Used by the EISData.mesh_warp variant.
//
// DIRECTION:
//   The mesh is the FORWARD direction — it describes what the camera DID to map
//   from sensor-native positions to encoded-frame positions. The consumer
//   numerically inverts (Newton / Nelder-Mead on the interpolated mesh) to map
//   encoded positions back to sensor positions for sampling.
//
// COORDINATE FRAME:
//   Both grid anchors and warped output positions are in SENSOR-pixel coordinates,
//   measured relative to FrameMetadata.crop_x, crop_y (the per-frame capture-area
//   origin). Axes match LensOISData / IBISData: +X right, +Y down, sensor-native
//   (do NOT rotate with ClipMetadata.rotation_degrees).
//
// LAYOUT:
//   Grid anchors are uniformly spaced over the rectangle [0, region_width] ×
//   [0, region_height] in sensor-pixel coordinates:
//       anchor(i, j) = ( i · region_width  / (grid_width  - 1),
//                        j · region_height / (grid_height - 1) )
//
//   For each anchor in row-major order (index k = j · grid_width + i), the pair
//   (warped_xy[2k], warped_xy[2k + 1]) gives the (x, y) position the camera
//   mapped that anchor TO under its in-camera EIS warp. Typically the warped
//   position is the anchor itself plus a small displacement (sub-pixel to a few
//   pixels) representing the per-frame stabilization correction.
//
//   Total warped_xy length = 2 · grid_width · grid_height.
//
// INTERPOLATION:
//   Between grid anchors, the consumer interpolates the warped positions. Cubic
//   spline (Catmull-Rom or natural cubic) is recommended for smoothness; bilinear
//   is acceptable for low-precision use cases.
message MeshWarpData {
    // Grid resolution. Typically 9 × 9.
    uint32 grid_width  = 1;
    uint32 grid_height = 2;

    // Rectangular region the grid covers, in sensor pixels relative to crop origin.
    float region_width  = 3;
    float region_height = 4;

    // Warped target position for each anchor. 2 · grid_width · grid_height floats,
    // row-major, two per anchor (x, y).
    repeated float warped_xy = 5;
}

// Per-sample GPS / GNSS position fix.
//
// SAMPLE CADENCE: typically much lower than IMU — 1 Hz for generic GNSS chips,
// up to ~10–18 Hz for high-rate sport-camera receivers. Most video frames at
// 1 Hz GPS rates contain no GPSData entries; producers emit them only on the
// frames during whose interval the receiver fired. Multi-sample entries within
// a single frame are permitted and MUST set sample_timestamp_us on every sample.
//
// COORDINATE / UNIT CONVENTIONS:
//   - latitude_degrees  ∈ [−90, +90],   WGS-84, N positive / S negative.
//   - longitude_degrees ∈ [−180, +180], WGS-84, E positive / W negative.
//     Producers whose native form is DMS plus N/S/E/W hemisphere char MUST
//     convert to signed decimal degrees before writing.
//   - altitude_m in meters. The reference surface (WGS-84 ellipsoid vs MSL via
//     internal geoid model) is producer-defined — this schema does not
//     distinguish them. Most consumer GNSS chips output MSL via a built-in
//     geoid model; raw RTK altitude is usually ellipsoid. Consumers that need
//     a specific reference SHOULD verify against the producer's documented
//     behavior.
//   - speed_mps in m/s (SI). Producers whose native unit is km/h MUST divide by 3.6.
//   - track_degrees ∈ [0, 360), 0° = true north, 90° = east (clockwise).
//   - velocity_*_mps decompose ground velocity in the ENU (East-North-Up)
//     local-tangent-plane frame, m/s. Optional and independent of speed_mps /
//     track_degrees: producers MAY emit either, both, or neither. When both
//     are present they MUST be self-consistent:
//        speed_mps   ≈ √(velocity_east_mps² + velocity_north_mps²)
//        track_degrees ≈ ((atan2(velocity_east_mps, velocity_north_mps) · 180/π) + 360) mod 360
//      (the +360 / mod 360 wrap is required because atan2 returns (−180°, +180°]
//      but track_degrees is defined on [0, 360); westerly headings would
//      otherwise come out negative.)
//   - *_accuracy_* fields are 1-σ standard deviations as estimated by the
//     receiver (typically from satellite geometry + signal-to-noise — NOT a
//     fixed multiple of σ). Producers SHOULD omit these unless their hardware
//     provides them; setting them to zero would be misinterpreted as "perfect
//     accuracy" rather than "unknown accuracy".
message GPSData {
    // Sample timestamp on the same camera clock as FrameMetadata.start_timestamp_us.
    // Unit: microseconds. Optional; when omitted (single-entry case), the sample
    // applies to the whole frame. For multi-sample entries this MUST be set on
    // every sample. Producers that have only an absolute UTC time (no camera-
    // clock alignment) SHOULD leave this unset and rely on unix_timestamp_s.
    optional double sample_timestamp_us = 1;

    // Absolute time of the fix in Unix-epoch seconds (UTC), if known. Producers
    // that natively use GPS-epoch time MUST subtract the GPS↔UTC leap-second
    // offset (~18 s as of 2025) before writing — UTC is the universal convention
    // here. Independent of
    // sample_timestamp_us: producers MAY emit either, both, or (rarely) neither;
    // when both are present they refer to the same physical instant.
    optional double unix_timestamp_s = 2;

    // True iff the receiver has a valid position fix at this sample. Defaults
    // to false (proto3 scalar default), which is the safe interpretation for
    // producers that omit it. Producers SHOULD also set fix_type when their
    // hardware reports finer detail — consumers MAY treat fix_type ∈
    // {Fix2D, Fix3D, RTK} as implying is_acquired = true even when the bool
    // is unset.
    bool is_acquired = 3;

    // Finer fix-quality classification, when the producer hardware reports it.
    // Optional — is_acquired alone is enough for the basic valid/invalid split.
    optional FixType fix_type = 4;

    enum FixType {
        Unknown = 0; // Producer did not classify the fix
        NoFix   = 1; // Receiver has no usable fix this sample
        Fix2D   = 2; // Horizontal position valid; altitude unreliable
        Fix3D   = 3; // Horizontal position + altitude valid
        RTK     = 4; // Real-Time Kinematic — centimeter-class accuracy (survey-grade)
    }

    // WGS-84 latitude in degrees, N positive / S negative.
    double latitude_degrees  = 5;
    // WGS-84 longitude in degrees, E positive / W negative.
    double longitude_degrees = 6;

    // Altitude in meters. Reference surface (ellipsoid vs MSL) is producer-
    // defined — see the COORDINATE / UNIT CONVENTIONS block above.
    optional float altitude_m = 7;

    // Ground speed in m/s.
    optional float speed_mps     = 8;
    // Ground course / heading in degrees, 0° = true north, increasing clockwise.
    optional float track_degrees = 9;

    // 1-σ uncertainty estimates as reported by the receiver, in the corresponding
    // native units. Producers SHOULD omit these when their hardware doesn't
    // supply them — zero is NOT a valid placeholder for "unknown".
    optional float horizontal_accuracy_m = 10;
    optional float vertical_accuracy_m   = 11;
    optional float speed_accuracy_mps    = 12;

    // Decomposed ground velocity in the ENU (East-North-Up) local-tangent-plane
    // frame, m/s. Optional alternative to speed_mps + track_degrees; many GNSS-
    // only formats emit the polar form instead. See the self-consistency
    // requirement in the COORDINATE / UNIT CONVENTIONS block when both
    // representations are present.
    optional float velocity_east_mps  = 13;
    optional float velocity_north_mps = 14;
    optional float velocity_up_mps    = 15;
}
```
{% endcode %}
