---
description: >-
  In some cases, you may want to have more control over the encoding parameters.
  This section describes the advanced features of the Export settings panel.
---

# 🎬 Exporting

### Custom encoder options

Gyroflow uses **ffmpeg** for the rendering process. There are a lot of different encoders, parameters and cases, depending on the file type, operating system and your graphic card. For most cases, the defaults are fine, but if you want to pass an additional setting to the encoder, you can use the **Custom encoder options** field and enter the parameters as you would to the **ffmpeg** command line. For example: `-preset hq -profile rext`.&#x20;

The list of accepted parameters is available by clicking the **Info** icon (**Show available options**), next to the text field. You can google the parameter names + ffmpeg for specific explanations.

Note that Gyroflow doesn't use the ffmpeg command line (CLI), but rather the C API directly, so not all options that can be passed to the ffmpeg CLI, can be used here. This field is for the **video encoder parameters only**.

Additional supported options are: **-hwaccel\_device**, **-qscale** and **-pix\_fmt**.

### Metadata comment

Text written to the comment field of the exported file's metadata.

### Keyframe distance

When using H.264 or H.265, you can specify the keyframe distance (default 1 s) - it's the interval for every **I-frame** (a fully encoded frame independent of other frames). For detailed explanation what it is read [**this Wikipedia article**](https://en.wikipedia.org/wiki/Video\_compression\_picture\_types). It has nothing to do with parameter keyframing in Gyroflow, it's a video encoder parameter.

### Preserve other tracks

Some video files contain additional tracks, like timecode, subtitles, motion data etc. By default, Gyroflow discards these tracks and only renders one video and one audio track. In order to preserve them and copy directly to the stabilized file, you can check this option.&#x20;

Since Gyroflow 1.7.0, this option copies the camera's metadata tracks and the file metadata as they are, so apps that read them recognize the exported video. For example, DJI Mimo shows its dashboard overlays (see [DJI](../getting-started/supported-cameras/dji.md#exporting-for-dji-mimo)). If some tracks can't be copied, the export finishes with a warning that lists them.

Since 1.7.0, this option also no longer forces the .mov format and copying the audio. The audio uses the selected **Audio codec**, and the output stays .mp4, unless the audio codec is **Copy** (then it's .mov).

You can't use the trim range with this option (i.e. you need to render the full file), because Gyroflow doesn't know how to cut arbitrary binary tracks.

### Use black frames outside trim range and keep original file duration

When using the trim range, the rendered file will only be the selected trim section of the original file. However in some cases, it is desirable to render only a fragment of the file, but retain full original length of the file. A common case is when using a video editor and having the edit already done. If you then replace the clip with a shorter one, the edit will fall apart. But if you replace the clip with one with the exact same duration, everything will work fine.

When you check this option, Gyroflow will render black frames outside the trim range, and actual stabilized video inside the trim range. This allows the video codec to compress the black frames, so the file size will still be small, while retaining the full original video duration. This can also be useful when combined with the **Preserve other tracks** option.

### Export trim ranges as separate videos

With several trim ranges, they're exported as one video by default. With this option, every trim range is exported as a separate video.

### Audio codec

By default, Gyroflow renders the audio as AAC with the same bitrate as the input file (up to 320 kbps). You can choose a different format in **Audio codec**: AAC, PCM (16-bit or 24-bit, little or big endian), or **Copy**.

**Copy** (since Gyroflow 1.7.0) keeps the original audio without re-encoding it. If the output format can't store the original audio codec, AAC is used instead.

### Interpolation method

How the pixels of the source frame are sampled: **Bilinear**, **Bicubic**, **Lanczos4** (default) or one of the **EWA** methods (RobidouxSharp, Robidoux, Mitchell, Catmull-Rom). Bilinear is the fastest and the softest. The EWA methods take into account how much of the source image each output pixel covers, so they avoid aliasing where the image is shrunk (e.g. with a smaller output size), but they're slower.

### Export fps

Gyroflow always exports the video file with the same frame rate as the original video file. There is currently no option to change the frame rate.

### Device for rendering

Some computers or laptops have more than one GPU and usually one is more powerful that the other (e.g. integrated GPU vs. the dedicated GPU). You can choose the preferred device used for **video encoding** here.&#x20;

There's also a second selector in the **Advanced** section which controls the device used for the **stabilization process** itself. These processes are separate and you can process on one GPU and render on the other GPU.

Some devices support more encoding formats and some devices don't support GPU video encoding at all, so make sure you select the correct (and more powerful) device here.

### Preserve export settings and path

**Preserve export settings** keeps the output size and bitrate for all files you open, instead of taking them from each video. **Preserve export path** does the same for the output folder.

### Experimental encoders

**Use experimental Vulkan encoder** (Windows and Linux, for H.264, H.265 and AV1) and **Use experimental D3D12 encoder** (Windows, for H.265 and AV1) use the GPU's hardware encoder through these generic APIs, instead of the vendor's own API (NVENC, AMF or Quick Sync). Try them if the regular GPU encoding doesn't work on your system.

### Project types

When exporting a project file, you have 3 options:

* **Project file** - This is the smallest project file which can be used when your video file contains internal motion data. The motion data will always be in the original video file, so there's no need to save it separately in the project. This project contains all stabilization parameters and the file path of the video.
* **Project file (including gyro data)** - If your motion data is from external source or is in a separate file (like .gcsv), then this project file will embed the entire motion data inside the project file, so there's no need to keep the separate gyro file around. This can be useful when you're using a GoPro as a logger, or when you want to export a project file for the video editor plugins. The default shortcut to save project file (**Ctrl+S**) is saving with gyro data, as it's the safe default for all cases.
* **Project file (including processed gyro data)** - This option is similar to the previous one, but it also contains additional motion data processed by Gyroflow - it is useful in combination with external scripts like [**Gyroflow-to-CSV**](https://github.com/EmberLightVFX/Gyroflow-to-CSV). Video editor plugins don't need this option, the `(including gyro data)` one is enough.

The first two are in the menu of the arrow on the **Export** button, and the third one in `Motion data -> Export`. All three are also available as the **Export mode** of the [⏩ Render queue](render-queue.md#export-mode).
