# 💎 Features

* Real-time preview, parameter adjustments and all calculations
* GPU processing and rendering, all algorithms fully multi-threaded
* Rolling shutter correction
* [Optical correction](../advanced-usage/input-data.md#optical-correction): measures the camera motion from the video itself, so footage without motion data can be stabilized, and bad gyro data (like vibrations on hard-mounted FPV cameras) can be corrected
* [Video editor plugins](https://github.com/gyroflow/gyroflow-plugins) (Adobe Premiere/Ae, DaVinci Resolve, Final Cut Pro, Shotcut and more), allowing you to apply stabilization directly in a video editor without transcoding
* Supports full Sony metadata (recording params, automatic lens, support for IBIS, OIS, EIS, Active and Dynamic Active, lens breathing compensation - you can have IBIS enabled in camera and still apply Gyroflow on top)
* Supports already stabilized GoPro videos (captured with Hypersmooth enabled) (Hero 8 and up)
* Uses the lens calibration recorded in GoPro HERO11 and newer videos, including SuperView and HyperView
* Follows the recorded focal length of zoom lenses, with optional focal length stabilization that smooths out fast zooms
* Supports and renders 10-bit videos (up to 16-bit 4:4:4:4 for regular codecs and 32-bit float for OpenEXR - working directly on YUV data to maintain maximum quality)
* Customizable lens correction strength
* Render queue
* Keyframes
* Ability to create custom settings presets
* Visual chart with gyro data (displays gyro, accelerometer, magnetometer, and quaternions, including smoothed quaternions)
* Supports underwater footage (corrects underwater distortions)
* Modern responsive user interface with Dark and Light theme
* Adaptive zoom (dynamic cropping)
* Zoom limit, or a fixed zoom level for every video
* Rectilinear or fisheye output projections (stereographic, equidistant, equisolid angle, orthographic), to show the whole picture of very wide lenses
* Supports image sequences (PNG, OpenEXR, CinemaDNG)
* Based on [telemetry-parser](https://github.com/AdrianEddy/telemetry-parser) - supports all gyro sources out of the box
* Gyro low pass filter, arbitrary rotation (pitch, roll, yaw angles) and orientation
* Multiple gyro integration methods for orientation determination
* Multiple video orientation smoothing algorithms, including horizon levelling (with pitch locking and automatic leaning into turns) and per-axis smoothness adjustment.
* Cross-platform - works on Windows/Linux/Mac/Android/iOS
* Multiple UI languages
* Supports variable and high frame rate videos - all calculations are done on timestamps
* H.264/AVC, H.265/HEVC, AV1, ProRes, DNxHD, CineForm, PNG and OpenEXR outputs, with H.264, H.265 and AV1 fully GPU accelerated (ProRes also accelerated on Apple Silicon)
* Easy lens calibration process, with the OpenCV fisheye, OpenCV standard and generic polynomial models, and a calibration method for lenses that see more than 180°
* Fully zero-copy GPU preview rendering
* Core engine is a separate library without external dependencies (no Qt, no ffmpeg, no OpenCV), which is also used by the video editor plugins
* Automatic updates of lens profile database
* Built-in official lens profiles for GoPro HERO 5-13; Sony; DJI; Insta360 action cameras; RunCam: Thumb series, 5 Orange
* Easy management of the video editor plugins from within the app
* Ability to add an additional 3D rotation (useful for framing vertical videos)

### Supported gyro sources

* [x] GoPro (HERO 5 and later, including .LRV proxy files)
* [x] Sony (most newer models, see the [list](../getting-started/supported-cameras/sony.md#supported-models))
* [x] Insta360 (OneR, OneRS, SMO 4k, Go, GO 2, GO 3, GO 3S, GO Ultra, Caddx Peanut, Ace, Ace Pro, including .lrv proxy files; motion data only: X5, Antigravity A1)
* [x] DJI (Avata, Avata 2, O3/O4 Air Unit, Action 2/4/5/6/Nano, Neo, Neo2; motion data only: Osmo 360, Avata 360)
* [x] Xtra (Edge, Edge Pro)
* [x] Blackmagic RAW (\*.braw)
* [x] RED RAW (V-Raptor, KOMODO) (\*.r3d)
* [x] Canon (C50, C80, C400, R6 Mk3, R5 Mk2, R6 V) (\*.mp4, \*.mov, \*.mxf; motion data only: Cinema RAW Light \*.crm)
* [x] Z CAM (E2-M4 Mk2, E2-S6 Mk2, E2-F6 Mk2, E2-F8 Mk2, E2-M4G Mk2, E2-S6G Mk2, E2N Mk2) (\*.mp4, \*.mov; motion data only: ZRAW \*.zraw)
* [x] HOVERAir
* [x] Freefly (Ember)
* [x] Betaflight blackbox (\*.bfl, \*.bbl, \*.csv)
* [x] ArduPilot logs (\*.bin, \*.log)
* [x] Gyroflow [.gcsv log](https://docs.gyroflow.xyz/app/technical-details/gcsv-format)
* [x] [Gyroflow Protobuf](../technical-details/gyroflow-protobuf.md) (embedded in \*.mp4, \*.mov, or a \*.jsonl sidecar file)
* [x] iOS apps: [`Sensor Logger`](https://apps.apple.com/us/app/sensor-logger/id1531582925), [`G-Field Recorder`](https://apps.apple.com/at/app/g-field-recorder/id1154585693), [`Gyro`](https://apps.apple.com/us/app/gyro-record-device-motion-data/id1161532981)
* [x] Android apps: [`Sensor Logger`](https://play.google.com/store/apps/details?id=com.kelvin.sensorapp\&hl=de_AT\&gl=US), [`Sensor Record`](https://play.google.com/store/apps/details?id=de.martingolpashin.sensor_record), [`OpenCamera Sensors`](https://github.com/MobileRoboticsSkoltech/OpenCamera-Sensors), [`MotionCam Pro`](https://play.google.com/store/apps/details?id=com.motioncam.pro) (including its RAW video \*.mcraw, motion data only)
* [x] Runcam CSV (Runcam 5 Orange, iFlight GOCam GR, Runcam Thumb, Mobius Maxi 4K)
* [x] Hawkeye Firefly X Lite CSV
* [x] XTU (S2Pro, S3Pro)
* [x] WitMotion (WT901SDCL binary and \*.txt)
* [x] Vuze (VuzeXR)
* [x] KanDao (Obsidian Pro, Qoocam EGO)
* [x] [ESP-gyrologger](../advanced-usage/using-external-gyro-source/esp-gyrologger.md)
* [x] [NiYien SenseFlow A1](../advanced-usage/using-external-gyro-source/niyien-a1.md)
* [x] [CAMM format](https://developers.google.com/streetview/publish/camm-spec)
