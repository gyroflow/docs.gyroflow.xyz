---
description: >-
  Gyroflow engine is also available as an OpenFX plugin to allow you to
  stabilize videos inside a video editor like DaVinci Resolve
---

# DaVinci Resolve plugin (OpenFX)

## Supported hosts

1. DaVinci Resolve
2. MAGIX Vegas
3. Possibly other OpenFX-compatible hosts, however they were not tested

## Installation

{% tabs %}
{% tab title="Windows" %}
## Installing the OpenFX plugin

1. Download the latest version from [https://gyroflow.xyz/download#plugins](https://gyroflow.xyz/download#plugins)
2. Extract `Gyroflow-ofx-windows.zip` somewhere
3. Create the OFX folder: `C:\Program Files\Common Files\OFX\Plugins`.
4. Copy `Gyroflow.ofx.bundle` folder to `C:\Program Files\Common Files\OFX\Plugins`



<figure><img src="../../.gitbook/assets/openfx_install_windows.png" alt=""><figcaption><p>Properly installed plugin on Windows</p></figcaption></figure>



## Hardware acceleration

Gyroflow OFX plugin supports OpenCL and CUDA acceleration on Windows



## Latest development build

If you want to check out latest features and fixes, you can download the latest dev build from [https://nightly.link/gyroflow/gyroflow-ofx/workflows/build/main/gyroflow-ofx-windows.zip](https://nightly.link/gyroflow/gyroflow-ofx/workflows/build/main/gyroflow-ofx-windows.zip)

The list of all nightly builds is available on [GitHub actions](https://github.com/gyroflow/gyroflow-ofx/actions)
{% endtab %}

{% tab title="macOS" %}
## Installing the OpenFX plugin

1. Download the latest version from [https://gyroflow.xyz/download#plugins](https://gyroflow.xyz/download#plugins)&#x20;
2. Create the OFX folder: `/Library/OFX/Plugins`.&#x20;
   1. You can do that in Finder or in the Terminal: `sudo mkdir -p /Library/OFX/Plugins ; open /Library/OFX/Plugins`.
3. Run `Gyroflow-ofx-macosx.dmg`
4. Copy `Gyroflow.ofx.bundle` folder to `/Library/OFX/Plugins/`



{% hint style="info" %}
Make sure the OFX folder is in the root `/Library` folder, not your user `/Users/you/Library`
{% endhint %}



<figure><img src="../../.gitbook/assets/openfx_install_macos.jpg" alt=""><figcaption><p>Properly installed plugin on macOS</p></figcaption></figure>



## Hardware acceleration

Gyroflow OFX plugin supports Metal acceleration on macOS



## Latest development build

If you want to check out latest features and fixes, you can download the latest dev build from [https://nightly.link/gyroflow/gyroflow-ofx/workflows/build/main/gyroflow-ofx-macosx.zip](https://nightly.link/gyroflow/gyroflow-ofx/workflows/build/main/gyroflow-ofx-macosx.zip)

The list of all nightly builds is available on [GitHub actions](https://github.com/gyroflow/gyroflow-ofx/actions)
{% endtab %}

{% tab title="Linux" %}
## Installing the OpenFX plugin

1. Download the latest version from [https://gyroflow.xyz/download#plugins](https://gyroflow.xyz/download#plugins)&#x20;
2. Create the OFX folder: `/usr/OFX/Plugins`.&#x20;
   1. You can do that in the Terminal: `sudo mkdir -p /usr/OFX/Plugins && sudo chown $USER /usr/OFX/Plugins`.
3. Extract`Gyroflow-ofx-linux.zip` to `/usr/OFX/Plugins`



<figure><img src="../../.gitbook/assets/openfx_install_linux.png" alt=""><figcaption><p>Properly installed plugin on Linux</p></figcaption></figure>



## Hardware acceleration

Gyroflow OFX plugin supports OpenCL and CUDA acceleration on Linux



## Latest development build

If you want to check out newest features and fixes, you can download the latest dev build from [https://nightly.link/gyroflow/gyroflow-ofx/workflows/build/main/gyroflow-ofx-linux.zip](https://nightly.link/gyroflow/gyroflow-ofx/workflows/build/main/gyroflow-ofx-linux.zip)

The list of all nightly builds is available on [GitHub actions](https://github.com/gyroflow/gyroflow-ofx/actions)
{% endtab %}
{% endtabs %}



