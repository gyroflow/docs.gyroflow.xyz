# 💎 Features

* Real time preview, params adjustments and all calculations
* GPU processing and rendering, all algorithms fully multi-threaded
* Rolling shutter correction
* Supports already stabilized GoPro videos (captured with Hypersmooth enabled) (Hero 8 and up)
* Supports and renders 10-bit videos (up to 16-bit 4:4:4:4 for regular codecs and 32-bit float for OpenEXR - works directly on YUV data to keep maximum quality)
* Customizable lens correction strength
* Render queue
* Keyframes
* Ability to create custom settings presets
* [OpenFX plugin](https://github.com/gyroflow/gyroflow-ofx) (eg. for DaVinci Resolve), which allows you to apply stabilization in video editor without any transcoding
* [Gyroflow Toolbox](https://gyroflowtoolbox.io/) - A Final Cut Pro effect which allows you to import a Gyroflow Project without transcoding
* Visual chart with gyro data (can display gyro, accel, magnetometer and quaternions)
* Visual display of smoothed quaternions
* Modern responsive user interface with Dark and Light theme
* Adaptive zoom (dynamic cropping)
* Support for image sequences (PNG, OpenEXR, CinemaDNG)
* Based on [telemetry-parser](https://github.com/AdrianEddy/telemetry-parser) - supports all gyro sources out of the box
* Gyro low pass filter, arbitrary rotation (pitch, roll, yaw angles) and orientation
* Multiple gyro integration methods for orientation determination
* Multiple video orientation smoothing algorithms, including horizon levelling and per-axis smoothness adjustment.
* Cross-platform - works on Windows/Linux/Mac/Android/iOS
* Multiple UI languages
* Supports variable and high frame rate videos, all calculations are done on timestamps
* H.264/AVC, H.265/HEVC, ProRes, DNxHD, CineForm, PNG and OpenEXR outputs, with H.264 and H.265 fully GPU accelerated (ProRes also accelerated on Apple M1 Pro/Max/Ultra)
* Automatic lens calibration process
* Fully zero-copy GPU preview rendering
* Core engine is a separate library without external dependencies (no Qt, no ffmpeg, no OpenCV), and can be used to create OpenFX and Adobe plugins (on the TODO list)
* Automatic updates of lens profile database
* Built-in official lens profiles for GoPro: HERO 6, 7, 8, 9, 10, 11, 12; RunCam: Thumb, ThumbPro, 5 Orange; Insta360: GO 2 in all shooting modes

### Supported gyro sources

* [x] &#x20;GoPro (HERO 5 and later)
* [x] &#x20;Sony (a1, a7c, a7r IV, a7 IV, a7s III, a9 II, FX3, FX6, FX9, RX0 II, RX100 VII, ZV1, ZV-E10, ZV-E1, a6700)
* [x] &#x20;Insta360 (OneR, OneRS, SMO 4k, Go, GO2, GO3, Caddx Peanut, Ace, Ace Pro)
* [x] &#x20;DJI (Avata, O3 Air Unit, Action 2, Action 4)
* [x] &#x20;Blackmagic RAW (\*.braw)
* [x] &#x20;RED RAW (V-Raptor, KOMODO) (\*.r3d)
* [x] &#x20;Betaflight blackbox (\*.bfl, \*.bbl, \*.csv)
* [x] &#x20;ArduPilot logs (\*.bin, \*.log)
* [x] &#x20;Gyroflow [.gcsv log](https://docs.gyroflow.xyz/logging/gcsv/)
* [x] &#x20;iOS apps: [`Sensor Logger`](https://apps.apple.com/us/app/sensor-logger/id1531582925), [`G-Field Recorder`](https://apps.apple.com/at/app/g-field-recorder/id1154585693), [`Gyro`](https://apps.apple.com/us/app/gyro-record-device-motion-data/id1161532981), [`GyroCam`](https://apps.apple.com/us/app/gyrocam-professional-camera/id1614296781)
* [x] &#x20;Android apps: [`Sensor Logger`](https://play.google.com/store/apps/details?id=com.kelvin.sensorapp\&hl=de\_AT\&gl=US), [`Sensor Record`](https://play.google.com/store/apps/details?id=de.martingolpashin.sensor\_record), [`OpenCamera Sensors`](https://github.com/MobileRoboticsSkoltech/OpenCamera-Sensors), [`MotionCam Pro`](https://play.google.com/store/apps/details?id=com.motioncam.pro)
* [x] &#x20;Runcam CSV (Runcam 5 Orange, iFlight GOCam GR, Runcam Thumb, Mobius Maxi 4K)
* [x] &#x20;Hawkeye Firefly X Lite CSV
* [x] &#x20;XTU (S2Pro, S3Pro)
* [x] &#x20;WitMotion (WT901SDCL binary and \*.txt)
* [x] &#x20;Vuze (VuzeXR)
* [x] &#x20;KanDao (Obisidian Pro)
* [x] &#x20;[CAMM format](https://developers.google.com/streetview/publish/camm-spec)
