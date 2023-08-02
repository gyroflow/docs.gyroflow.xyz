---
description: Gyroflow has a CLI interface for usage in scripts and automating workflows.
---

# 🏗 Command line (CLI)

You can also use Gyroflow as a command line program, in order to automate some tasks in scripts.

Most common usage will be to provide a video file and render the stabilized video. You can simply call `Gyroflow.exe YOUR_FILE.mp4` to do that in case of sync-less files, or also add `--preset YOUR_PRESET.gyroflow` to apply any settings and/or lens profile you want.&#x20;

CLI can also render from a project file.

One of more interesting CLI features is a **watch folder**, which can be used to automatically stabilize all new footage copied to that specified folder. You can simply run `Gyroflow.exe --watch C:\MY_FOOTAGE\` and it automatically stabilize any new video files that appear in that folder.&#x20;

All other options are pretty self-explanatory.

```
Usage: gyroflow [<input...>] [-f] [-j <parallel-renders>] [-d <when-done>] [-p <out-params>] [--export-project <export-project>] [--preset <preset>] [--open <open>] [--watch <watch>] [-g <gyro-file>]

Gyroflow v1.4.2
Video stabilization using gyroscope data

Positional Arguments:
  input             input files: videos, project files, lens profiles, presets

Options:
  -f, --overwrite   overwrite if output file exists, default: false
  -j, --parallel-renders
                    number of parallel renders, default: 1
  -d, --when-done   when done: 1 - shut down; 2 - reboot; 3 - sleep; 4 -
                    hibernate; 5 - logout
  -p, --out-params  output parameters, eg. "{ 'codec': 'H.265/HEVC', 'bitrate':
                    150, 'use_gpu': true, 'audio': true }"
  --export-project  export project file instead of rendering: 1 - default
                    project, 2 - with gyro data, 3 - with processed gyro data
  --preset          preset (file or content directly), eg. "{ 'version': 2,
                    'stabilization': { 'fov': 1.5 } }"
  --open            open file in the GUI (video or project)
  --watch           watch folder for automated processing
  -g, --gyro-file   gyro file path
  --help            display usage information
```
