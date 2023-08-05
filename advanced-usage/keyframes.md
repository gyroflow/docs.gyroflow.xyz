---
description: >-
  Gyroflow comes with the ability to keyframe most parameters in order to
  achieve any look you want.
---

# 🔑 Keyframes

Most parameters in Gyroflow can be keyframed - meaning their value can be changed in different parts of the video (i.e. you can use a different smoothness at the start of the video, compared to the end).

## How to use?

### Enabling keyframes

Keyframes can be enabled by right-clicking on the slider and choosing **Enable keyframing.**

<figure><img src="../.gitbook/assets/enable_keyframing.png" alt=""><figcaption></figcaption></figure>

Then, go to a time in the video when you want the value to be different and just change the slider value. This will create a keyframe on the timeline chart.

<figure><img src="../.gitbook/assets/keyframe_example.png" alt=""><figcaption><p>Smoothness keyframe</p></figcaption></figure>

### Keyframe easing

You can right-click the keyframe point to enable or disable easing (it's enabled by default).

<figure><img src="../.gitbook/assets/keyframe_easing_menu.png" alt=""><figcaption></figcaption></figure>

<figure><img src="../.gitbook/assets/keyframe_easing.png" alt=""><figcaption><p>Ease in + out vs. linear keyframe</p></figcaption></figure>

### Removing/disabling keyframes

You can right-click the keyframe point to delete it, or to disable all keyframing for a slider, right-click on the slider and uncheck **Enable keyframing** - this will clear all keyframes on that slider.

## Keyframeable parameters

* Smoothness
* FOV
* Zooming speed
* Zooming center offset
* Horizon lock amount / roll correction
* Lens correction strength
* Max smoothness
* Max smoothness at high velocity
* Pitch/Roll/Yaw smoothness
* Video rotation
* Background margin / feather
* Video speed

## Example

{% embed url="https://gyroflow.xyz/demo/docs/?v=keyframing" %}

## Caveat

When keyframing zooming speed and the speed changes are significant, make sure to switch the zooming algorithm to **Envelope follower.**&#x20;

There is a known bug with the **Gaussian filter** algorithm which may lead to black borders in view when keyframing zooming speed.
