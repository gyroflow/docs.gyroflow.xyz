---
description: Import Gyroflow Projects into Apple's Final Cut Pro.
---

# 🎬 Final Cut Pro X

## About Gyroflow Toolbox

**Gyroflow Toolbox** allows you to import Gyroflow Projects directly into Apple's Final Cut Pro. This allows you to take the stablised data from Gyroflow and use it within Final Cut Pro as an effect, so that you don't have to export a ProRes file (for example), from Gyroflow - avoiding any round-tripping, and unnessary render files.

It also allows you to manipulate and keyframe parameters such as FOV, Smoothness, Lens Correction, Horizon Lock/Roll, Position Offset and Rotation directly within Final Cut Pro.

Gyroflow Toolbox is powered by the same Gyroflow Core Engine that the hero Gyroflow application uses, so you get the same awesome performance in Final Cut Pro as you do in the hero Gyroflow application.

You can even use this in conjunction with [BRAW Toolbox](https://brawtoolbox.io/) (also on the App Store), to stabilise **Blackmagic RAW files**!

To get started, simply drop your video clip from the Final Cut Pro Browser or Finder to the drop zone in the Final Cut Pro Inspector. You can also drop a Gyroflow Project.

**Sony**, **GoPro (Hero 8+)**, **DJI**, **Blackmagic RAW** and **Insta360** will be automatically synced when imported. Other cameras, such as **RED**, will require launching Gyroflow to synchronise.

**GoPro**, **DJI** and **Insta360** will automatically select a Lens Profile. For all other cameras, you may need to manually select a Lens Profile, however, we'll try and GUESS a good match.

<figure><img src="../.gitbook/assets/App Store Image 2 - v2.png" alt=""><figcaption></figcaption></figure>

## Download

**Gyroflow Toolbox** is a one-time payment of **9.99**.

This is generally **9.99** in your local currency (i.e. **AUD9.99**, **USD9.99**, **CAD9.99**).

However, if your country doesn't have an equivalent of 9.99 (i.e. Rp169,000 in Indonesia), it will default to a **USD$9.99 equivalent** (calculated by Apple).

There is currently no free trial, and it is only be available on the Mac App Store as a **one-time payment**.

Alternatively, as Gyroflow Toolbox is **open source**, you can also [build it yourself via GitHub](https://github.com/latenitefilms/GyroflowToolbox).\
\
You can learn more and purchase on the Mac App Store [here](https://apps.apple.com/au/app/gyroflow-toolbox/id1667462993?mt=12).

## Limitations

**You should always put the Gyroflow Toolbox effect on a clip inside a Compound Clip.**

This is because you should only ever apply the Gyroflow Toolbox effect to an **entire clip** - the clip cannot be trimmed. Due to limitations in Final Cut Pro's FxPlug4 API - we currently can't determine the source start timecode of a clip. Because of this, the Gyroflow Toolbox effect should only be applied to a clip where the start of the clip hasn't been trimmed in the timeline (i.e. the clip you have in the timeline should show the first frame of the source clip). If you need to trim the start of this clip, you can use the full clip within a Compound Clip, then trim the Compound Clip as required. We have been in contact with the Final Cut Pro team about this, and there's currently no other better workaround or solution. Discussed in [issue 8](https://github.com/latenitefilms/GyroflowToolbox/issues/8).

## Documentation & Support

You can find the official Gyroflow Toolbox website here:

[https://gyroflowtoolbox.io](https://gyroflowtoolbox.io)
