# 🔧 Basic usage

## How does it work?

Gyroflow works by using motion data recorded by the camera to recreate camera motion in real world and compensate for the camera movements, therefore creating a new, stabilized world view.

In order to do that, Gyroflow must know exactly how the camera was oriented in space when the video frame was captured. It is **very important** for the motion data to be accurately representing the video frames.

Most cameras record the motion data outside of the video pipeline and this causes time difference between capturing actual pixels and capturing camera position. In order to compensate for that, we have to figure out that time offset so Gyroflow can know exactly where the camera was at a particular pixel of the video.

This is done by the **Synchronization** step in Gyroflow and it's very important to do it accurately. Otherwise Gyroflow can't know how the camera was moving and will create bad results.

{% hint style="warning" %}
**Remember!** If you're not getting a good stabilization, the most important questions you should ask yourself are:\
1\. Is my synchronization (time offset) accurate?\
2\. Is my lens profile accurate?\
3\. Is my [rolling shutter correction](stabilization.md#rolling-shutter-correction) value accurate?
{% endhint %}

In order to move the video around for stabilization, we also have to remove the lens from the equation. All camera lenses create some distortions and when we attempt to move the view, we'll move it with these distortions and it will look bad.&#x20;

Gyroflow needs to know the real world view that is not skewed by the lens distortions, so we can move sort of a "flat" plane around and avoid warping caused by the lens optics.&#x20;

This is done by calibrating the camera lens and using the lens profile in Gyroflow.&#x20;

Once you have accurate lens profile and accurate motion data, Gyroflow can take your video, undistort the lens optics, move the world view around according to the motion data, and then optionally add the lens distortion back. If your synchronization or lens profile is not accurate, that whole process falls apart and produces bad results.

Some cameras (e.g. GoPro 8+, DJI) have official lens profile and accurate timing data built-in, so they just work out of the box. All other cameras need the lens profile and synchronization done by the user.&#x20;

## General UI overview

Gyroflow's user interface is divided into 3 sections:

1. **Left:** Inputs - all input files and input parameters are on the left side
2. **Center:** Timeline and gyro data - below the video you have the main timeline with the gyro data. This is a very important tool, so make sure you learn how to use it.
3. **Right:** Stabilization parameters and outputs

All sliders in Gyroflow can be reset to the default value by double clicking on the label, or right clicking and selecting **Reset value**.

All sliders also have [**🔑 Keyframing**](../../advanced-usage/keyframes.md) available when you right click and select **Enable keyframing.**

All value fields can be adjusted by using _Up_ and _Down_ keys on your keyboard, with Ctrl/Alt/Shift modifiers for different step amount.

### Video information

This section contains the input file and the detected information about it.

You can see the detected camera and lens info, resolution, duration, frame rate, codec, pixel format, audio, rotation and whether the gyro data was detected in that video file.

There are 2 editable fields here: Frame rate and rotation

1. Frame rate - this is the frame rate used for the **motion data** and this should be the real-time frame rate. For most videos, you don't need to change it. However if you record in a VFR mode, where the video is captured at e.g. 120 fps but the file is slowed down to 30 fps - this is where you should edit that field and enter **119.88 fps** (remember to put exact frame rate, for NTSC it's 23.976, 29.97, 47.952, 59.94, 119.88 etc. For PAL it's 25, 50, 100 etc.)
2. Rotation - if you captured the video on the side or upside down, you can edit that field and enter rotation in degrees.

### Lens profile

This section contains the lens profile for your video. Lens profile is very important as it's the base for the stabilization algorithm. You can either pick one of built-in lens profiles, or create your own. In general [fps ](#user-content-fn-1)[^1]doesn't matter for lens profile, as long as it uses the same sensor area.

Some cameras have the lens profile built-in and loaded automatically.

You can also mark lens profile as a favorite by clicking the star icon.

Lens profiles can be scaled, i.e. it's OK to load a lens profile created for 4k video, to a 1080p video, as long as the aspect ratio is the same and the camera uses the same sensor area.

For this reason, search box validates the aspect ratio and fades out lens profiles which don't match the aspect ratio of your video.

In some cameras (like RED), different resolutions always use different part of the sensor, so the lens profile can't be scaled and you need to use one created for the same exact resolution. Have this in mind when troubleshooting stabilization issues.

The search box here can also hold your [⚙ **settings presets**](../../advanced-usage/settings-presets.md) if you place your preset in the `camera_presets` directory. Presets can also be favorited and are searchable.

The search box also understands shortcuts for some common camera names: `bmpcc4k, bmpcc6k, bmpcc, gopro5, gopro6, gopro7, gopro8, gopro9, gopro10, gopro11, session5, a73, a74, a7r3, a7r4, a7s2, a7s3`

To learn how to calibrate your own lens, go to [🏁 **Lens calibration**](../lens-calibration.md)

To learn more about advanced lens profile features, see [🔭 **Advanced usage -> Lens profile**](../../advanced-usage/lens-profiles.md).

### Motion data

This section contains the motion data for the actual stabilization. For most common use cases, it will be automatically loaded from the video file. If you want to use a different motion data source (like external gyro logger, drone blackbox or a different camera), you can load the motion data file here.

For more details about using external motion data file, go to [🌀 **Using external gyro source**](../../advanced-usage/using-external-gyro-source/).

### Once you have all the input files loaded, proceed to the [📈 Timeline and gyro chart](timeline-and-gyro-chart.md)



[^1]: frames per second
