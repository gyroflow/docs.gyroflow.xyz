# 🔥 Stabilization

This page describes the advanced options of the Stabilization panel. For the basics, see [🔥 **Basic usage -> Stabilization**](../getting-started/basic-usage/stabilization.md).

## Smoothing algorithms

* **No smoothing** - keeps the camera motion as it was recorded. Lens and rolling shutter correction still apply.
* **Default** - smooths the motion depending on its speed: slow motion is smoothed a lot, while fast motion (like a quick pan or a flip) is followed more closely, so the video doesn't have to zoom in as much. Use it for almost everything.
* **Plain 3D** - smooths all motion equally, with the **Smoothness** time in seconds (default 0.25 s). Fast motion makes it zoom in a lot.
* **Fixed camera** - keeps the camera pointed in one fixed direction, set with the **Roll angle**, **Pitch angle** and **Yaw angle**, like on a tripod. When the camera points away from that direction, you'll see the background (black) instead of the video.

### Default algorithm options

**Smoothness** is the main slider. The amount of smoothing also depends on the lens: the same rotation moves the picture more on a narrow lens, so a narrow lens gets less smoothing than a wide one at the same value.

In the **Advanced** section below the slider:

* **Per axis** - separate **Pitch smoothness**, **Yaw smoothness** and **Roll smoothness** instead of one slider.
* **Only within trim range** (on by default) - the motion outside the trim range doesn't affect the smoothing inside it. For example, a crash right after the end of the trim range won't pull the smoothed motion before it.
* **Max smoothness** - the smoothing time for slow motion (default 1 s). Higher values make slow motion smoother.
* **Max smoothness at high velocity** - the smoothing time for fast motion (default 0.1 s).

All of these sliders can be [🔑 keyframed](keyframes.md).

## Horizon lock

### Lock pitch angle

Since Gyroflow 1.7.0, **Lock horizon** can also hold the camera tilt (up/down) at a fixed angle. Check **Lock pitch angle** and set the angle with **Pitch angle correction**. It can be [🔑 keyframed](keyframes.md), like the roll angle.

Pitch locking isn't available when **Use gravity vectors** is enabled, because the gravity vectors only level the roll.

### Automatic horizon lock

Since Gyroflow 1.7.0, **Automatic lock** lets the horizon lean into turns, like in a banked turn, instead of keeping it level all the time. When the camera rolls faster than a threshold, the horizon tilts with it, proportionally to the roll speed. Enabling it sets the lock amount to 100%.

* **Turn threshold** - the roll speed (in °/s) above which the horizon starts to tilt.
* **Turn smoothness** - the smoothing time of the turn detection (in ms).
* **Turn multiplier** - how much the horizon tilts: the tilt is the multiplier times the roll speed (°/s → °).
* **Limit tilt acceleration** - limits how fast the tilt can speed up or slow down, with **Tilt acceleration limit** (in °/s²). It prevents jerky horizon movements.

### Gravity vectors

Some cameras (e.g. GoPro) record the direction of gravity, as computed by the camera. In that case, **Use gravity vectors** shows up, and the horizon is levelled using that data instead of Gyroflow's own estimate. It's not always better, so compare both on your footage.

### Integration method

When the motion data uses the camera's built-in orientation (**Integration method** set to **None** in the Motion data panel) and the file also has the raw gyroscope data, the horizon lock has its own **Integration method**. It's used only to keep track of the horizon, while the rest of the stabilization keeps using the camera's orientation.

## Zooming

### Static zoom: zoom level

With **Static zoom**, **Zoom level** can be:

* **Automatic** - as much zoom as the video needs, up to the zoom limit.
* **Manual** - since Gyroflow 1.7.0, exactly the zoom you set in **Zoom**, in every video, like in-camera stabilization, so you know the framing while shooting. The smoothing is lowered only where the frame wouldn't fit otherwise. If even that isn't enough, the video is zoomed in further and a warning is shown. You can then increase **Zoom limit iterations** in `Stabilization -> Advanced`, or the zoom.

### Zoom limit

The zoom limit is calculated approximately. If you need more accuracy, increase **Zoom limit iterations** in `Stabilization -> Advanced`. More iterations give a more accurate limit, but are slower to calculate.

Since Gyroflow 1.7.0, with **Static zoom**, the zoom limit lowers the smoothing only around the moments that don't fit, instead of in the whole clip. The old behavior is available in `Stabilization -> Advanced -> Zoom limit lowers the smoothing of the whole clip`. Projects saved with older versions open with it enabled, so they look the same.

### Advanced -> Zooming method

With **Dynamic zooming**, the zoom follows how much each frame needs, smoothed with the **Zooming speed**:

* **Envelope follower** (default) - never zooms in less than a frame needs, and eases in and out of each zoom with the zooming speed as the time constant.
* **Gaussian filter** - takes the largest zoom needed within the zooming speed window around each frame, and blurs it with a Gaussian curve. When the zooming speed is [🔑 keyframed](keyframes.md), it may show black borders, so Gyroflow switches to **Envelope follower** then.

### Advanced -> Additional 3D rotation

Rotates the output view by a **Pitch**, **Yaw** and **Roll** angle, after the stabilization. You can use it to reframe the video, e.g. to keep the subject in frame when exporting a vertical video from a horizontal one. It can be [🔑 keyframed](keyframes.md).

It replaces the older **Zooming center offset**, which is no longer shown.

## Lens correction strength

The **Lens correction** slider controls how much of the lens distortion is removed. At 100%, the lens distortion is fully corrected and straight lines are straight. Lower values add the original lens distortion back, which can be useful to keep the look of a wide lens.

Since Gyroflow 1.7.0, values below 100% are exact and change the image evenly. In older versions, most of the change happened close to 100%, so the same value may look different in older projects.

## Video speed

**Video speed** in `Stabilization -> Advanced` changes the speed of the exported video, from 10% to 1000%. You can [🔑 keyframe](keyframes.md) it to create speed ramps. The export keeps the frame rate of the original file and skips or repeats frames. Audio isn't exported when the speed is changed.

The three small checkboxes above the slider (all on by default) adapt the stabilization to the new speed:

* **Link with smoothing** - the smoothing is scaled with the speed, so the exported video looks as smooth as at 100%.
* **Link with zooming speed** - the zooming speed is scaled with the speed.
* **Link with zooming limit** - the zoom limit is raised with the speed, by a quarter of the speed change (at most 1.8 times). For example, at 200% speed, a 130% zoom limit becomes 162.5%.
