# 📸 Common filming tips and issues

## Shutter speed, ND filters and motion blur

For any electronic stabilization it's best to avoid motion blur. This means that you have to use high shutter speed and avoid ND filters if the lighting conditions allow it.

Motion blur can't be removed in post, so if you capture it in camera, then you stabilize it later, that "motion" will remain in the original direction, while your video will have a different (stabilized) motion. This creates an ugly-looking effect and in some cases ruins the footage. This also applies to the internal Hypersmooth stabilization.

Here's an example of bad motion blur after stabilization:

{% embed url="https://youtu.be/0Clu7SaMTZw" %}

You might not notice this effect in your original footage, because motion blur follows the motion you're seeing so it looks natural. Once you stabilize the footage, the frame motion is changed (stabilized) but the motion blur remains in the original direction, and this creates a dissonance in your brain (because it no longer matches the motion)

If you use high shutter speed to capture sharp frames, you can the freely move them around to stabilize without risking these artifacts. You can then add motion blur in post, after stabilization, in your video editor. A great plugin for motion blur is [ReelSmart Motion Blur](https://revisionfx.com/products/rsmb/). You can also use built-in tools eg. in DaVinci Resolve or in Adobe After Effects (Pixel Motion Blur)

### 180° shutter speed rule

A common rule in filmmaking is to use 180° shutter speed (i.e. twice the frame rate, e.g. 1/60 when shooting 30 fps). However **that rule doesn't work when using any kind of digital stabilization.**

Shooting with 180° shutter speed is good when your camera is physically stabilized, on a 3-axis gimbal, a crane, slider etc. This is commonly used in filmmaking because movie sets do have the cameras physically stabilized, the shots are planned and the camera motion is predictable.

This rule doesn't work for scenarios like FPV drones, mountain biking, running or basically any action-camera type of shots, where the camera is shaking a lot. It can only work if you fly very smoothly and the stabilization is needed only to correct minor movements and not big shakes. A great example of an FPV video with motion blur where strong stabilization is not needed is [here](https://www.youtube.com/watch?v=VjvmY-0mk5s).

Using 90° or even 45° shutter will be much better choice if you want to use any post-stabilization.
