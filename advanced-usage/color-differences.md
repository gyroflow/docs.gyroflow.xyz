# 🎨 Color differences

If the colors look different after exporting from Gyroflow, in most cases the pixels didn't change: a player or video editor just interprets the color range or the gamma of the file differently.

## What Gyroflow does

Gyroflow processes the video directly in its original YUV format, without converting the colors. The exported file gets the same color metadata as the original: the color range, the color matrix, the transfer curve (gamma) and the color primaries.

A few encoders don't store all of it:

* GPU encoding on macOS (VideoToolbox) doesn't write the transfer curve.
* ProRes encoded on the GPU on macOS from 8-bit 4:2:0 video is always limited range (Gyroflow converts the pixels to match, to work around an encoder bug).
* The H.264/H.265 GPU encoder on Intel Macs ignores the requested color range.

## Color range

The color range is either **Full** (0-255, also called JPEG or Data levels) or **Limited** (16-235, also called MPEG or Video levels), and it's stored in the file metadata. Players, video editors and websites don't always read it the same way:

* Limited range shown as full looks washed out (grey blacks, dull whites).
* Full range shown as limited looks too contrasty (crushed blacks, clipped whites).

In DaVinci Resolve, you can change it in `Clip Attributes -> Data Levels` (Auto, Video or Full).

## Gamma

The transfer curve (gamma) can also be interpreted differently, for example by QuickTime on macOS, or after uploading to YouTube. This happens with any video, not only with files from Gyroflow.

## How to find the cause

Your file can look the same or different at every stage: the original file, the export from Gyroflow, played in VLC, imported in your video editor, exported from the editor, uploaded to YouTube etc. Compare them in the same player, and change one thing at a time. There's no single rule, so find a workflow that works for your setup.

Read more about color range:

* [Color Space and Color Range](https://cinelerra-gg.org/download/CinelerraGG\_Manual/Color\_Space\_Color\_Range\_Aff.html) (Cinelerra-GG manual)
* [RGB Full vs. Limited](https://referencehometheater.com/2014/commentary/rgb-full-vs-limited/)
* [Data Levels Settings and Conversions](https://coloraggio.github.io/davinci-resolve-manuals/12.5\_Manual/6.pdf) (DaVinci Resolve manual)

## Avoiding it altogether

Use one of the [video editor plugins](../video-editor-plugins/general-plugin-workflow.md). The editor then reads the original file and applies the stabilization itself, so there's no extra file whose colors could be interpreted differently.
