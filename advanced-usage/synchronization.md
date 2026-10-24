# ⌛ Synchronization

This page describes the options of the Synchronization panel. Read [⌛ **Basic usage -> Synchronization**](../getting-started/basic-usage/synchronization.md) first, to learn how the synchronization works and how to check it. Figuring out the offset manually is described there as well.

## Main options

* **Rough gyro offset** - your initial guess of the offset, see [Synchronizing external gyro source](../getting-started/basic-usage/synchronization.md#synchronizing-external-gyro-source). The small checkbox next to it also analyzes the negative offset. It doubles the calculation time, so use it only for the first sync point, until you know the offset.
* **Sync search size** - the range around the rough offset that is searched. The small checkbox next to it (shown for the **Visual features** and **rs-sync** offset methods) first estimates the offset with the faster **Essential matrix** method, and then refines it. It's checked automatically when the search size is larger than 10 s.
* **Max sync points** - how many sync points **Auto sync** adds. They're spread evenly over the trim range.

The small checkbox next to the **Auto sync** button enables the experimental automatic sync point selection: instead of spreading the points evenly, Gyroflow picks the moments with the most usable motion in the motion data.

## Advanced

* **Analyze every n-th frame** - analyzes only every n-th frame. It's useful for high frame rate footage (e.g. 240 fps), where consecutive frames barely differ, and it makes the analysis faster. This setting isn't remembered between sessions.
* **Time to analyze per sync point** - how much of the video is analyzed around each sync point (default 1.5 s). Longer gives the algorithm more motion to match, but takes longer.
* **Processing resolution** - the video is scaled down to this height before the analysis (default 720p). Higher resolutions can track finer detail, but are slower.
* **Optical flow method** - how the motion of the pixels between frames is measured:
  * **OpenCV (DIS)** (default) - dense optical flow.
  * **OpenCV (PyrLK)** - tracks individual corner points.
  * **AKAZE** - matches features between frames. It may be more accurate, but it's much slower. Use it only if the OpenCV methods don't give good results.
* **Pose method** - how the camera rotation between frames is calculated from the optical flow: **findEssentialMat** (default), **Almeida**, **EightPoint** or **findHomography**. This rotation is the lighter lines on the chart.
* **Offset method** - how the offset is found:
  * **rs-sync** (default) - a synchronization algorithm that takes the rolling shutter into account. Make sure the [rolling shutter correction](../getting-started/basic-usage/stabilization.md#rolling-shutter-correction) is set correctly before syncing.
  * **Visual features** - undistorts the optical flow points with the motion data at each candidate offset, and picks the offset where the video moves the least.
  * **Essential matrix** - compares the camera rotation calculated from the optical flow with the rotation from the motion data.
* **Low pass filter** - filters the motion measured from the video (the lighter lines on the chart), which can help when it's noisy.
* **Show detected features** and **Show optical flow** - draw the tracked points and their motion over the preview, where the video was analyzed.

## Synchronization settings in lens profiles and presets

A lens profile or a [settings preset](settings-presets.md) can carry its own synchronization settings, and can run **Auto sync** automatically when the file is loaded. This is useful for cameras that always need synchronization, e.g. in the render queue. See [Synchronization settings](lens-profiles.md#synchronization-settings) in the lens profile file reference.
