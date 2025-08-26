---
description: Gyroflow has a CLI interface for usage in scripts and automating workflows.
---

# 🏗️ Command Line (CLI)

You can also use Gyroflow as a command line program, in order to automate some tasks in scripts.

Most common usage will be to provide a video file and render the stabilized video. You can simply call `Gyroflow.exe YOUR_FILE.mp4` to do that in case of sync-less files, or also add `--preset YOUR_PRESET.gyroflow` to apply any settings and/or lens profile you want.

CLI can also render from a project file.

One of more interesting CLI features is a **watch folder**, which can be used to automatically stabilize all new footage copied to that specified folder. You can simply run `Gyroflow.exe --watch C:\MY_FOOTAGE\` and it automatically stabilize any new video files that appear in that folder.

All other options are pretty self-explanatory.

```
Usage: gyroflow [<input...>] [-f] [-j <parallel-renders>] [-d <when-done>] [-p <out-params>] [-t <suffix>] [-s <sync-params>] [--stdout-progress] [--export-project <export-project>] [--export-metadata <export-metadata>] [--export-metadata-fields <export-metadata-fields>] [--export-stmap <export-stmap>] [--preset <preset>] [--open <open>] [--watch <watch>] [-g <gyro-file>] [-b <processing-device>] [-r <rendering-device>] [--no-gpu-decoding] [--version]

Gyroflow v1.6.2
Video stabilization using gyroscope data

Positional Arguments:
  input             input files: videos, project files, lens profiles, presets

Options:
  -f, --overwrite   overwrite if output file exists, default: false
  -j, --parallel-renders
                    number of parallel renders, default: 1
  -d, --when-done   when done: 1 - shut down; 2 - reboot; 3 - sleep; 4 -
                    hibernate; 5 - logout
  -p, --out-params  output parameters, eg. "{{ 'codec': 'H.265/HEVC',
                    'bitrate': 150, 'use_gpu': true, 'audio': true }}"
  -t, --suffix      default output suffix, eg. "_stabilized"
  -s, --sync-params synchronization parameters, eg. "{{ 'search_size': 3,
                    'processing_resolution': 720 }}"
  --stdout-progress print progress to stdout in addition to progress bars
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
                    'euler_angles': true }}, 'zooming': {{ 'minimal_fovs':
                    true, 'fovs': true, 'focal_length': true }} }}"
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
