---
description: >-
  Accurate synchronization is the most important step in Gyroflow to get good
  results.
---

# ⌛ Synchronization

## What is synchronization?

Synchronization in Gyroflow is the process of adding sync points - the time offset between motion data and video data.&#x20;

Most cameras record the motion data outside of the video pipeline and this causes time difference between capturing actual pixels and capturing camera position. In order to compensate for that, we have to figure out that time offset so Gyroflow can know exactly where the camera was at a particular pixel of the video.

{% hint style="info" %}
Synchronization is needed for all cameras except GoPro Hero 8+, DJI, Insta360, Sony and BRAW.
{% endhint %}

{% hint style="warning" %}
When using internal gyro data, the time offset will never be larger than 1 second (1000 ms). Usually even less than ±200 ms.

When using external gyro data, the offset can be arbitrary and it's the time between starting recording of the motion data and video file.
{% endhint %}

Synchronization works by analyzing the video visually, using an optical flow algorithm to recreate the camera motion from pixels. Once that motion is calculated, it tries to match the motion data from file with the motion data from optical flow. It then adds a sync point with the value (time offset) that makes these two motions overlap.

Estimating motion from optical flow is not accurate and depends on a lot of factors, so sometimes this algorithm will fail and produce bad sync point. It is therefore important for you to learn how to detect that and correct it.

Before you attempt to sync, you also need to have proper [**🔀 IMU orientation and rotation**](../../advanced-usage/imu-orientation-and-rotation.md) set. It is usually correct by default, but keep that in mind. Autosync can't work if your motion data doesn't correspond to the video motion. Sometimes you can determine the IMU orientation during the syncing (an example of that is in the Examples section on this page).

## Sync points

A sync point is a time offset by which the motion data should be shifted in either positive or negative direction, to align properly with the video data. Each sync point has a value (time offset in milliseconds). If you have multiple sync points on the timeline, the time offset is interpolated between them.

{% hint style="danger" %}
**Very important!** All sync points should have similar values. Sometimes the values will be linearly increasing or decreasing, and sometimes they will be very similar, but in general you can't have multiple sync points with contradicting values. For example, 3 sync points with values 10, -50, 70 is definitely wrong, but 3 sync points with values 10, 12, 16 is plausible. 10.5, 11, 10.7 is also plausible because they are very close to each other.

More examples:\
**BAD:** 0.5ms, -100ms, -150ms, 20ms\
**BAD:** -120ms, -130ms, 25ms, -150ms (the 25ms is the contradicting one)\
**BAD:** 1500ms, 120ms, 45ms, 5ms\
\
**GOOD:** -1400ms, -1405ms, -1407ms, -1408ms\
**GOOD:**  49.5ms, 50ms, 50.5ms, 51ms\
**GOOD:**  20.4ms, 20.4ms, 20.2ms, 20.0ms
{% endhint %}

### Sync point colors

All sync points have a color ranging from green to red. In general, green means a good sync point, and red means bad sync point. If you have multiple green points and one red - delete the red one.

It may also happen that the red sync point is actually correct, because you have one correct point and 4 bad points.

The color is based on line fit on the time offsets, so it indicates how linear all the points are.

**This is not a definitive indicator!** It may help troubleshoot but if your video plays correctly, even red sync point can be actually fine. This is especially important with RunCam cameras where this indicator doesn't work at all and should be ignored.

The most common usage will be to use "**Auto sync here**" to create a sync point, and right click + "**Delete sync point**" to remove it. Adjusting sync point value manually can be done by clicking on it and using the slider that appears below it (or the value field next to the slider), but should generally be avoided and autosync should be used instead.

You can also right click on the sync point and choose "Zoom in and loop", to create a trim range around the sync point and zoom in the chart. You can then adjust its value and watch the video until it's synchronized correctly.&#x20;

## Getting good sync points

The synchronization algorithm works best in places where there is significant motion in all directions. If you have these places in the video, they are preferred to pick.

The algorithm doesn't work in places where there is no motion, or very little, because it doesn't have enough information to estimate camera movement visually at these places.

If you're not getting good sync points, make sure the **Sync search size** parameter covers possible offset range. Like mentioned before for internal gyro data it should be within 1s, and for external gyro data it will be the time you waited between recording gyro and video.

Usually 2-3 sync points are enough and the recommended amount for good quality cameras is 3. In certain challenging situations you will need more sync points, maybe 10 or even more.

## How to check if my synchronization is accurate?

The best way to check if a sync point is accurate it to just play the video at that time. You can create a trim range around the sync point to loop the video on it. If the video is stabilized then you're good. If not, then zoom in on the chart and make sure the lighter lines overlap the darker lines. If they don't, remove that sync point. If you don't see lighter lines, right click there and choose "Auto sync here".

Keep in mind that other sync points can affect this sync point. For example, if your video is 10s long and you have a correct sync point of 5 ms in the middle, but then have a sync point with -7000 ms, then the entire data will be stretched and won't be correct even at that one 5 ms sync point.

If you want to rely on the sync point colors, you'll need to have more than 2, because 2 sync points are always linear, and the colors show how linear all the offsets are.

## Examples

TODO

## Figuring out the offset manually

If everything else fails, there's also an option to figure out the offset manually. You can right click on the timeline and **Add manual sync point here**, and then you can click on the point and adjust the value using the slider below it, or the value field on the right of the slider.

By moving the slider or entering a different value, you'll see the gyro data moving left or right on the chart. You can then try to match it with the optical flow, or (if optical flow failed) you can change the value in intervals and observe the video visually until it's stable.

The default range of the slider is -VALUE to +VALUE, so the more value (time offset) the point has, the slider will have greater range. By default it starts with 15ms, but you can enter any higher value in the value field, and the range of the slider will adjust.

TODO: example with optical flow

TODO: example video without optical flow

## Settings that affect the synchronization algorithm

If you edit any of the following settings, you have to run the autosync again, because it depends on these values: **Motion data low pass filter, rotation, bias, IMU orientation, integration method and rolling shutter correction**.&#x20;

Remove all sync points after changing these settings and run the autosync again.

## Synchronizing external gyro source

When using external gyro source, the most important parameter is the **Sync search size.** It is the possible range of a sync point, i.e. no sync point can be larger than this value.

The second parameter is **Rough gyro offset.** It's an initial guess (by you) of the offset. For example, if you know that you started recording gyro data, waited 30 seconds and then you started recording video, the rough gyro offset would be 30s.

If you know that you started recording both at the same time, leave **Rough gyro offset** at 0s.

These two parameters work together. The scanned range is from `rough_offset - search_size` to `rough_offset + search_size`. For example:

| Rough gyro offset | Sync search size | Scanned range |
| ----------------- | ---------------- | ------------- |
| 0s                | 5s               | -5s to 5s     |
| 15s               | 2s               |  13s to 17s   |
| -30s              | 10s              | -40s to -20s  |

Read more on the [**🌀 Using external gyro source**](../../advanced-usage/using-external-gyro-source/) page.
