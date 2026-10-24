---
description: Gyroflow has a CLI interface for usage in scripts and automating workflows.
---

# 🏗️ Command Line (CLI)

You can also use Gyroflow as a command line program, in order to automate some tasks in scripts.

Most common usage will be to provide a video file and render the stabilized video. You can simply call `Gyroflow.exe YOUR_FILE.mp4` to do that in case of sync-less files, or also add `--preset YOUR_PRESET.gyroflow` to apply any settings and/or lens profile you want.

CLI can also render from a project file.

One of more interesting CLI features is a **watch folder**, which can be used to automatically stabilize all new footage copied to that specified folder. You can simply run `Gyroflow.exe --watch C:\MY_FOOTAGE\` and it automatically stabilize any new video files that appear in that folder. Since Gyroflow 1.7.0, files that are still being copied are processed only when they're complete, and one bad file no longer stops the queue.

The output settings are passed with `-p`. Run `Gyroflow.exe --help` to see all the keys and their values. For example, to export AV1 and keep the original audio: `-p "{ 'codec': 'AV1', 'audio_codec': 'Copy' }"`.

{% hint style="info" %}
The examples in the help text show the braces doubled (`{{ }}`), but you have to type them once (`{ }`).
{% endhint %}

### Optical correction

Since Gyroflow 1.7.0, the CLI can also measure the camera motion from the video before rendering, like [**Optical correction**](input-data.md#optical-correction) in the app:

* `--optical-correction 50` - corrects the motion data with the motion measured from the video, with this strength in %. Files without motion data get all of their motion measured from the video.
* `--ignore-file-motion` - ignores the motion data in the files, and measures all of the motion from the video.
* `--optical-per-frame` - corrects the motion data once per frame instead of per band of rows, e.g. for a global shutter camera, or when the rolling shutter time is unknown.
* `--optical-tracking dis` - uses the DIS dense optical flow instead of KLT (the default), for low texture like sky or water. It's slower.

The last three options enable `--optical-correction` as well.

### Exit codes

Since Gyroflow 1.7.0, the CLI returns an exit code you can check in your scripts:

* `0` - success
* `1` - render failure
* `2` - invalid arguments

### All options

```
Usage: gyroflow [<input...>] [-f] [-j <parallel-renders>] [-d <when-done>] [-p <out-params>] [-t <suffix>] [-s <sync-params>] [--stdout-progress] [--optical-correction <optical-correction>] [--ignore-file-motion] [--optical-per-frame] [--optical-tracking <optical-tracking>] [--export-project <export-project>] [--export-metadata <export-metadata>] [--export-metadata-fields <export-metadata-fields>] [--export-stmap <export-stmap>] [--preset <preset>] [--open <open>] [--watch <watch>] [-g <gyro-file>] [-b <processing-device>] [-r <rendering-device>] [--no-gpu-decoding] [--version]

Gyroflow v1.7.0
Video stabilization using gyroscope data
Exit codes: 0 - success, 1 - render failure, 2 - invalid arguments

Positional Arguments:
  input             input files: videos, project files, lens profiles, presets

Options:
  -f, --overwrite   overwrite if output file exists, default: false
  -j, --parallel-renders
                    number of parallel renders, default: 1
  -d, --when-done   when done: 1 - shut down; 2 - reboot; 3 - sleep; 4 -
                    hibernate; 5 - logout
  -p, --out-params  output parameters. Keys: codec ("H.264/AVC", "H.265/HEVC",
                    "ProRes", "DNxHD", "CineForm", "EXR Sequence", "PNG
                    Sequence", "AV1"), codec_options (eg. "ProRes 422 HQ"),
                    output_path (or output_folder + output_filename, both
                    absolute), output_width, output_height, bitrate (Mbps),
                    use_gpu, audio, pixel_format (eg. "P210LE"), audio_codec
                    ("AAC", "PCM (s16le)", "PCM (s16be)", "PCM (s24le)", "PCM
                    (s24be)", "Copy" - keeps the original audio, encodes AAC if
                    the output format can't store it), interpolation
                    ("Bilinear", "Bicubic", "Lanczos4", "EWA: RobidouxSharp",
                    "EWA: Robidoux", "EWA: Mitchell", "EWA: Catmull-Rom"),
                    encoder_options, keyframe_distance, preserve_other_tracks,
                    pad_with_black, export_trims_separately, metadata ({{
                    'comment': "..." }}). Eg. "{{ 'codec': 'ProRes',
                    'codec_options': 'ProRes 422 HQ', 'pixel_format': 'P210LE',
                    'output_path': '/path/out.mov' }}"
  -t, --suffix      default output suffix, eg. "_stabilized"
  -s, --sync-params synchronization parameters, eg. "{{ 'search_size': 3,
                    'processing_resolution': 720 }}"
  --stdout-progress print progress to stdout in addition to progress bars
  --optical-correction
                    analyze the video optically before rendering and correct the
                    motion data with it (Motion data -> Optical correction),
                    with this strength in %, eg. 50. Files without motion data
                    get all of their motion measured from the video
  --ignore-file-motion
                    ignore the motion data in the files and measure all of the
                    motion from the video (implies --optical-correction)
  --optical-per-frame
                    correct the motion data once per frame instead of per band
                    of rows, eg. for a global shutter or a rolling shutter time
                    that's unknown (implies --optical-correction)
  --optical-tracking
                    how the optical analysis follows the points: "klt" (default,
                    corners) or "dis" (dense optical flow, for low texture like
                    sky or water; slower) (implies --optical-correction)
  --export-project  export project file instead of rendering: 1 - default
                    project, 2 - with gyro data, 3 - with processed gyro data, 4
                    - video + project file
  --export-metadata export metadata instead of rendering. <type>:<path>, where
                    type is 1 - full metadata, 2 - parsed metadata, 3 - camera
                    data. Eg. "3:camera.json"
  --export-metadata-fields
                    fields to include in the exported camera metadata file.
                    Defaults to all: "{{ 'original': {{ 'gyroscope': true,
                    'accelerometer': true, 'quaternion': true, 'euler_angles':
                    true }}, 'stabilized': {{ 'quaternion': true,
                    'euler_angles': true }}, 'zooming': {{ 'minimal_fovs': true,
                    'fovs': true, 'focal_length': true }} }}"
  --export-stmap    export STmap instead of rendering. <type>:<folder_path>,
                    where type is 1 - single frame, 2 - all frames. Eg.
                    "1:C:/stmaps/"
  --preset          preset (file or content directly), eg. "{{ 'version': 2,
                    'stabilization': {{ 'fov': 1.5 }} }}"
  --open            open file in the GUI (video or project)
  --watch           watch folder for automated processing
  -g, --gyro-file   gyro file path
  -b, --processing-device
                    processing device index. By default it uses the one set in
                    the GUI
  -r, --rendering-device
                    rendering device, specify the GPU type. Eg. "nvidia",
                    "intel", "amd", "apple m"
  --no-gpu-decoding don't use gpu video decoding, default: false
  --version         print app version
  --help, help      display usage information
```
