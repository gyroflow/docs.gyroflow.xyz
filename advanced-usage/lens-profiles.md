# 🔭 Lens Profiles

## Output projection

Since Gyroflow 1.7.0, you can choose how the stabilized image is projected. Click the value of **Output projection** in the lens information table, in the **Lens profile** panel:

* **Rectilinear** (default) - straight lines stay straight. A lens that sees more than 180° can't be shown whole, so its edges are cropped.
* **Stereographic** - fisheye that keeps shapes natural but bends straight lines. Shows the whole picture of a lens wider than 180°.
* **Equidistant** - fisheye where the distance from the center is proportional to the angle. Shows the whole picture of a lens wider than 180°.
* **Equisolid angle** - fisheye that keeps areas in proportion. Shows the whole picture of a lens wider than 180°.
* **Orthographic** - fisheye that compresses the edges strongly. Shows at most 180°.
* **Automatic** - rectilinear, or stereographic when the lens sees more than 180°.

The fisheye projections are useful for very wide lenses, like the Insta360 X4/X5 lenses or fisheye adapters, where a rectilinear image would have to crop a lot of the picture.

## No distortion, focal length only

If your video is already undistorted (it was recorded with a rectilinear lens, or the distortion was corrected in camera), you don't need a lens profile. Check **No distortion, focal length only** in the **Lens profile** panel and enter the **Focal length (35 mm equiv.)**. That's the focal length with the same field of view on a full frame (36x24 mm) sensor, so the focal length of the lens multiplied by the crop factor of the sensor. Gyroflow then uses a lens without any distortion, of that focal length.

This option is shown when the file doesn't have a lens profile, and in `Lens profile -> Advanced` when the loaded lens profile is unofficial. Unchecking it puts back the previous lens profile.

Files that record the focal length of every frame already work this way without a lens profile, so the option isn't shown for them.

## Zoom lenses

If the file records the focal length of every frame, Gyroflow follows it, so you can zoom during the shot. That's the case for Sony, and since Gyroflow 1.7.0 also for Canon (with supported lenses), Blackmagic, RED, Nikon and Z CAM. Since 1.7.0, the focal length is also interpolated between frames, so zooming clips may be framed slightly differently than in older versions.

### Stabilize focal length

You can enable **Stabilize focal length** in the Stabilization panel to smooth out fast zooms and zoom "pumping". Fast zooms are spread out by cropping ahead of a zoom-in and after a zoom-out, while zooms slower than **Max zoom speed** pass through unchanged. Lower values give a smoother zoom and more crop.

The **FL** button on the left side of the timeline chart shows the original and the smoothed focal length.

### Lens metadata delay

Some lenses report the focal length a few frames later than the picture (e.g. some Tamron zooms on Sony). It depends on the lens, not on the frame rate. If the picture jumps or wobbles during zooms, set **Lens metadata delay** in `Lens profile -> Advanced`, or click **Analyze** next to it to measure it from the zooms in the clip. The clip needs a zoom recorded in the lens metadata for the analysis to work.

This option is available for Sony files.

## Lens breathing compensation

Many lenses change the field of view slightly when focusing (this is called focus breathing). Since Gyroflow 1.7.0, Gyroflow can keep the field of view constant while focusing, using the lens breathing data recorded by the camera. That's the case for supported Sony lenses on cameras like the α7 IV, α7C II, α7R V, α6700 and ZV-E1.

When the file contains this data, **Lens breathing compensation** shows up in the **Lens profile** panel, and it's enabled by default.

## Lens is under water

Water bends the light at the lens port, so a lens has a narrower field of view under water. Enable `Lens profile -> Advanced -> Lens is under water` for footage filmed under water, and the lens profile is adjusted for it (refraction coefficient 1.33). Since Gyroflow 1.7.0, the adjustment is exact across the whole frame.

You can [🔑 keyframe](keyframes.md) it (right-click the checkbox), for clips that go in and out of the water.

## Asymmetrical lens

By default, Gyroflow assumes the optical center of the lens is in the middle of the frame. If it isn't (e.g. a lens that isn't centered on the sensor), enable **Lens is asymmetrical** in the [🏁 Lens calibrator](../getting-started/lens-calibration.md) before calibrating. The lens profile then keeps the calibrated optical center (`"asymmetrical": true` in the file).

## Lens parameters

`Lens profile -> Advanced` shows the parameters of the loaded lens profile: **Pixel focal length**, **Focal center** and **Distortion coefficients**. You can edit them to fine-tune a profile without calibrating again.

## Export STMap

**Export STMap** in `Lens profile -> Advanced` writes the lens undistortion and redistortion as STMaps (`-undistort-` and `-redistort-` `.exr` files) to the selected folder, for use in compositing apps like Nuke or Fusion. If the file records lens data for every frame (e.g. a zoom lens), you can export one STMap per frame. The [CLI](command-line-cli.md) can do the same with `--export-stmap`.

## Lens profile file

Lens profiles are `.json` files. Most fields are filled in by the [🏁 Lens calibrator](../getting-started/lens-calibration.md), but some features are only available by editing the file by hand. The examples below come from profiles in the lens profile database.

### Synchronization settings

`sync_settings` stores the synchronization settings for the camera. They're applied when the lens profile is loaded. With `"do_autosync": true`, **Auto sync** runs automatically when the video, the motion data and the lens profile are loaded, if the file has no sync points yet and its motion data isn't accurately timed. It also runs in the [⏩ Render queue](render-queue.md).

```json
"sync_settings": {
    "do_autosync": true,
    "initial_offset": 0,
    "initial_offset_inv": false,
    "search_size": 0.5,
    "max_sync_points": 5,
    "every_nth_frame": 1,
    "time_per_syncpoint": 0.5
}
```

The keys match the [⌛ Synchronization](synchronization.md) options. `initial_offset` (rough gyro offset), `search_size` and `time_per_syncpoint` are in seconds. `of_method`, `offset_method` and `pose_method` are the index of the option in the list, starting from 0.

### Custom sync point timestamps

By default, **Auto sync** spreads the sync points evenly. `custom_sync_pattern` in `sync_settings` places them at specific times instead. It's one pattern, or a list of them:

```json
"custom_sync_pattern": [
    { "start": "899", "interval": "1001", "gap": "0.6s" },
    { "start": "450", "interval": "1001" }
]
```

* `start` - the first sync point.
* `interval` - the distance between the sync points (default: the whole clip, so only one point).
* `gap` - adds two sync points instead of one, this far apart, centered on each position.

Plain numbers are frames, and you can also use seconds (`"0.6s"`) or milliseconds (`"600ms"`).

### Compatible settings

One calibration can be shared by several camera modes. `compatible_settings` lists them, and Gyroflow creates a copy of the lens profile for each:

```json
"compatible_settings": [
    { "width": 1920, "height": 1440, "fps": 23.976, "identifier": "gopro-hero10black-narrow-1920x1440@23976-no-eis" },
    { "width": 1920, "height": 1440, "fps": 25.0,   "identifier": "gopro-hero10black-narrow-1920x1440@25000-no-eis" }
]
```

If `width` and `height` have the same aspect ratio as the calibration, the calibration is scaled to them. If the aspect ratio is different, it's treated as a sensor crop. Each entry can also override other fields, like `frame_readout_time`, `crop`, `digital_lens`, `focal_length`, `output_dimension` or `sync_settings`.

### Crop

For a camera mode that uses only part of the calibrated image and scales it to the same resolution, set `crop` to that part, e.g. `"crop": 0.8` for 80% of the width. The focal length of the calibration is divided by it.

### Interpolations

A zoom lens changes its distortion along the zoom. `interpolations` holds calibrations at several focal lengths, and Gyroflow blends between them using the focal length recorded in the video file:

```json
"interpolations": {
    "14": { "camera_matrix": [[1541.53, 0, 1942.44], [0, 1542.31, 1087.03], [0, 0, 1]], "distortion_coeffs": [0.312, 0.109, -0.034, 0.092], "focal_length": 14 },
    "16": { "camera_matrix": [[1710.70, 0, 1927.51], [0, 1712.35, 1088.55], [0, 0, 1]], "distortion_coeffs": [0.318, 0.127, -0.048, 0.113], "focal_length": 16 }
}
```

The keys are focal lengths in millimetres. Instead of `camera_matrix` and `distortion_coeffs`, an entry can also point to another lens profile from the database with `"identifier"`.

Without `interpolations`, on cameras that record the focal length in millimetres (Blackmagic, RED, Nikon, Z CAM), a single calibration follows the zoom by scaling its focal length, and keeps the distortion of the calibration.

### Other fields

* `frame_readout_time` (ms) and `frame_readout_direction` - the [rolling shutter correction](../getting-started/basic-usage/stabilization.md#rolling-shutter-correction) applied with the profile.
* `global_shutter` - `true` for global shutter cameras, which need no rolling shutter correction.
* `gyro_lpf` - the motion data low-pass filter (Hz) applied with the profile.
* `input_horizontal_stretch` and `input_vertical_stretch` - the [input stretch](../getting-started/lens-calibration.md#input-stretch), e.g. for anamorphic lenses.
* `focal_length` (mm) and `crop_factor` - the lens focal length and the sensor crop factor.
* `output_dimension` - the default output size for this camera.

### Lens models

`distortion_model` is the lens model of the calibration. When it's missing, it's `opencv_fisheye`.

* `opencv_fisheye`, `opencv_standard` and `generic_polynomial` - the models you can choose in the Lens calibrator, see [Distortion model and calibration method](../getting-started/lens-calibration.md#distortion-model-and-calibration-method).
* `poly3`, `poly5` and `ptlens` - the lens models used by the lensfun database.
* `sony`, `insta360` and `gopro` - the models of the lens data recorded by these cameras. You don't need to set them yourself.

`digital_lens` describes digital processing done in the camera after the lens, which Gyroflow reverses first: `gopro_superview`, `gopro6_superview` and `gopro_hyperview` for older GoPro cameras (also available as **Digital lens** in the Lens calibrator), and `gopro_warp` for the lens data recorded by GoPro HERO11 and newer.
