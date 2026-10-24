---
description: >-
  Gyroflow engine is also available as an Adobe plugin to allow you to stabilize
  videos inside Adobe Premiere or Adobe After Effects.
---

# 🎞️ Adobe Premiere / After Effects

## Installation

The Gyroflow Adobe Plugin works on Mac and Windows.

{% hint style="success" %}
**The easiest way** to install or update the plugin is from the Gyroflow app: in the **Video editor plugins** panel at the bottom of the right side, below **Advanced**, click **Install** (or **Update**) next to **Adobe**. The panel shows up when Adobe Premiere Pro or After Effects is installed.
{% endhint %}

To install it manually:

{% tabs %}
{% tab title="Mac" %}
### Installing the Adobe plugin

1. Download the latest version from [https://gyroflow.xyz/download#plugins](https://gyroflow.xyz/download#plugins)&#x20;
2. Run `Gyroflow-Adobe-macos.dmg`
3. Copy `Gyroflow.plugin` folder to `/Library/Application Support/Adobe/Common/Plug-ins/7.0/MediaCore/`
{% endtab %}

{% tab title="Windows" %}
### Installing the Adobe plugin

1. Download the latest version from [https://gyroflow.xyz/download#plugins](https://gyroflow.xyz/download#plugins)
2. Extract `Gyroflow-Adobe-windows.zip` somewhere
3. Copy `Gyroflow-Adobe-windows.aex` file to `C:\Program Files\Adobe\Common\Plug-ins\7.0\MediaCore\`
{% endtab %}
{% endtabs %}

## ✨ Features

* Automatic loading of the project file, or gyro data from video file
* Support for speed ramping and time remapping
* Support for different aspect ratios and output sizes
* In After Effects, you can create a 3D camera based on the gyro data
* The plugin uses the clip trim range to calculate stabilization, so it doesn't consider data outside of your clip
* Plugin loads the default preset if you created one in the app
* Pasted or duplicated effects, the pieces of a cut clip and effects applied with a preset each have their own stabilization
* An effect copied to another clip loads that clip's project and keeps your adjustments
* The effect follows relinked or moved media, also when the project is opened on another computer
* Premiere Pro subclips are stabilized with the right part of the motion data
* In After Effects, the stabilization updates when keyframes are moved, eased or removed in the timeline, and after an undo

## Source Code

You can find the source code for this Adobe plugin on GitHub [here](https://github.com/gyroflow/gyroflow-plugins).

## Nightly Builds

Development moves fast and there are new features added very often, check them out by downloading the nightly build:

* [Nightly Adobe Build for Mac](https://nightly.link/gyroflow/gyroflow-plugins/workflows/release/main/Gyroflow-Adobe-macos.zip)
* [Nightly Adobe Build for Windows](https://nightly.link/gyroflow/gyroflow-plugins/workflows/release/main/Gyroflow-Adobe-windows.zip)
* [Nightly Gyroflow Build](https://gyroflow.xyz/devbuild/?autodownload)

The list of all nightly builds is available on [GitHub actions](https://github.com/gyroflow/gyroflow-plugins/actions).
