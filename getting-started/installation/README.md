---
description: >-
  Gyroflow is officially available in Microsoft Store, Apple Store and Google
  Play store. Downloadable binaries are available as well.
---

# ⚙ Installation

{% tabs %}
{% tab title="Windows" %}
## Install from the Microsoft Store:

[![](../../.gitbook/assets/badge\_microsoft\_store.png)](https://apps.microsoft.com/store/detail/gyroflow/9NZG7T0JCG9H)

## Manual install:

1. Download the latest version from [https://gyroflow.xyz/download](https://gyroflow.xyz/download)
2. Extract `Gyroflow-windows64.zip` somewhere
3. Open the extracted folder and run `Gyroflow.exe`
4. If it shows an error about `VCRUNTIME140.dll`, [install VC redist](https://aka.ms/vs/17/release/vc\_redist.x64.exe)



## Minimum system requirements

Windows 10 64-bit (1809 or later)

* If you have Windows "N" install, go to `Settings` -> `Apps` -> `Optional features` -> `Add a feature` -> enable `Media Feature Pack`

{% hint style="info" %}
Windows 7 is not supported, because of Qt 6 requirements
{% endhint %}
{% endtab %}

{% tab title="macOS" %}
## Install from the App Store:

[![](../../.gitbook/assets/badge\_apple\_store.png)](https://apps.apple.com/us/app/gyroflow/id6447994244)

## Beta channel:

Updates are pushed more often to TestFlight. Use this link to join as tester and get most recent updates first: [https://testflight.apple.com/join/e0RI1n5i](https://testflight.apple.com/join/e0RI1n5i)

## Manual install:

1. Download the latest version from [https://gyroflow.xyz/download](https://gyroflow.xyz/download)
2. Run`Gyroflow-mac-universal.dmg`
3. Drag & drop `Gyroflow` app to the Applications folder (or anywhere you want, like on Desktop)

{% hint style="info" %}
You can also install using brew: **`brew install gyroflow`**.&#x20;

To upgrade Gyroflow, run **`brew update`** then **`brew upgrade gyroflow`**
{% endhint %}



## Minimum system requirements

* macOS 10.14 or later

{% hint style="info" %}
Both Intel and Apple Silicon are supported natively.
{% endhint %}



### Hardware acceleration support:

<table><thead><tr><th width="287">Processor</th><th width="85">H.264</th><th width="91">H.265</th><th>ProRes</th></tr></thead><tbody><tr><td>Intel</td><td>✅</td><td>✅</td><td><a href="https://emojipedia.org/cross-mark/">❌</a></td></tr><tr><td>M1 Air</td><td>✅</td><td>✅</td><td><a href="https://emojipedia.org/cross-mark/">❌</a></td></tr><tr><td>M1 Pro/Max/Ultra</td><td>✅</td><td>✅</td><td>✅</td></tr><tr><td>M2 (all)</td><td>✅</td><td>✅</td><td>✅</td></tr></tbody></table>
{% endtab %}

{% tab title="Linux" %}
## Installing Gyroflow

1. Make sure you have latest GPU drivers installed
2. Download `Gyroflow-linux64.tar.gz` from [https://gyroflow.xyz/download](https://gyroflow.xyz/download)
3. Extract the files somewhere and run `./Gyroflow` in the terminal.
4. If it doesn't work, you probably need to install additional dependencies:

`sudo apt install libc++-dev libva2 libvdpau1 libasound2 libxkbcommon0 libpulse0 libvulkan1`

GPU specific packages:

* NVIDIA: `nvidia-opencl-icd nvidia-vaapi-driver nvidia-vdpau-driver nvidia-egl-icd nvidia-vulkan-icd libnvcuvid1 libnvidia-encode1`
* Intel: `intel-media-va-driver i965-va-driver beignet-opencl-icd intel-opencl-icd`
* AMD: `mesa-vdpau-drivers mesa-va-drivers mesa-opencl-icd libegl-mesa0 mesa-vulkan-drivers`

{% hint style="info" %}
`.AppImage`is also available, but it's recommended to run the `.tar.gz` package
{% endhint %}



## Minimum system requirements

* Debian 10+
* Ubuntu 18.10+
* CentOS 8.2+
* openSUSE 15.3+

{% hint style="info" %}
Other distros require glibc 2.28+ (**`ldd --version`** to check)
{% endhint %}
{% endtab %}

{% tab title="Android" %}
## Install from the Google Play Store:

[<img src="../../.gitbook/assets/badge_google_play.png" alt="" data-size="original">](https://play.google.com/store/apps/details?id=xyz.gyroflow)

## Manual install:

Nightly .apk can be downloaded from [https://nightly.link/gyroflow/gyroflow/workflows/release/master/Gyroflow-android.zip](https://nightly.link/gyroflow/gyroflow/workflows/release/master/Gyroflow-android.zip)

## Minimum system requirements

* Android 8
{% endtab %}

{% tab title="iOS" %}
## Install from the App Store:

[![](../../.gitbook/assets/badge\_apple\_store.png)](https://apps.apple.com/us/app/gyroflow/id6447994244)

## Beta channel:

Updates are pushed more often to TestFlight. Use this link to join as tester and get most recent updates first: [https://testflight.apple.com/join/e0RI1n5i](https://testflight.apple.com/join/e0RI1n5i)

## Manual install:

Installing from outside of Apple Store is possible only on jailbroken devices or if you have a developer account and know how to re-sign an .ipa. Nightly .ipa can be downloaded from [https://nightly.link/gyroflow/gyroflow/workflows/release/master/Gyroflow-ios.zip](https://nightly.link/gyroflow/gyroflow/workflows/release/master/Gyroflow-ios.zip)

## Minimum system requirements

* iOS 14
{% endtab %}
{% endtabs %}





## Latest development build

Development moves fast and there are new features added very often, check them out by downloading the nightly build

You can always download the latest build from [https://gyroflow.xyz/devbuild/](https://gyroflow.xyz/devbuild/)

The list of all nightly builds is available on [GitHub actions](https://github.com/gyroflow/gyroflow/actions)



