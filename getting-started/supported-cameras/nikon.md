---
description: Gyroflow plays Nikon RAW video files, but Nikon cameras don't record gyro data.
---

# Nikon

Nikon cameras don't record gyro data, so they need an [external gyro source](../../advanced-usage/using-external-gyro-source/), or the motion measured from the video with [Optical correction](../../advanced-usage/input-data.md#optical-correction).

Since Gyroflow 1.7.0, Gyroflow can open Nikon's RAW video files directly: **N-RAW** (.nev) and **R3D NE** (.r3d), recorded for example by the Nikon ZR. It also reads their recording metadata, like the lens focal length and the shooting settings.

## RAW playback

Gyroflow opens **.nev** and **.r3d** files directly, so there's no need for transcoding. Like R3D files from RED cameras, they're decoded with the RED SDK, which Gyroflow downloads the first time you open such a file.

## Lens profile

The camera doesn't record the lens distortion, so you have to provide a lens profile. You can either find it in the list of community-provided lens profiles, or calibrate your lens by following [🏁 this guide](../lens-calibration.md).

## Zoom lenses

The camera records the focal length of every frame, and Gyroflow follows it, so you can zoom during the shot. If you want to smooth out fast zooms, enable **Stabilize focal length** in the Stabilization panel. Learn more in [🔭 **Advanced usage -> Lens profiles**](../../advanced-usage/lens-profiles.md#zoom-lenses).

## Rolling shutter correction

The frame readout time isn't recorded in the file, so you have to set it in the Stabilization panel. Read more in [🔥 **Stabilization -> Rolling shutter correction**](../basic-usage/stabilization.md#rolling-shutter-correction). It's best to save it in your lens profile.

## Exporting

Exporting these files works the same way as with RED files: Gyroflow first converts them to ProRes with _REDline_ (available with the REDCINE-X installation). Read more on the [RED](red.md#exporting) page.

It's recommended to use one of the [video editor plugins](../../video-editor-plugins/general-plugin-workflow.md) instead, to keep all RAW color controls.

## Shutter speed, ND filters and motion blur

You should avoid motion blur when recording if you want to stabilize in post, read more why in the [📸 **Common filming tips and issues**](../common-filming-tips-and-issues.md).
