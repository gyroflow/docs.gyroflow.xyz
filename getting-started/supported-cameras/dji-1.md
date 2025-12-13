# Xtra

Xtra cameras contain the gyro data embedded in the video file.

{% hint style="warning" %}
**Important!** In-camera stabilization (EIS/MotionMaster) needs to be **disabled**, and FOV needs to be set to **Wide.** Otherwise your camera **will not** record gyro data and won't be usable in Gyroflow.\
\
Ultrawide, Normal or "Natural Wide" lens modes are **not supported.**
{% endhint %}

## Supported models

<table><thead><tr><th width="142">Model</th><th width="119.5714111328125">Gyro data</th><th width="124">Lens profile</th><th width="159">Synchronization</th><th>Remarks</th></tr></thead><tbody><tr><td>Edge<br>Edge Pro</td><td>✅ </td><td>✅ Official</td><td>✅ Not needed</td><td></td></tr><tr><td>Muse</td><td>❌</td><td>❌</td><td>❌</td><td></td></tr><tr><td>Sphra360</td><td>-</td><td>-</td><td>-</td><td>360 cameras are not supported</td></tr></tbody></table>

## Split recording

Xtra cameras have a file size limit of 4 GB. If the recording is longer, it will be split into multiple parts. If you want to stabilize such split video, you'll have to merge the parts before stabilization.

Gyroflow can do that for you. Just drag & drop all your files in the sequence to Gyroflow to merge.

You can find more information on the [**🎞 File joiner**](../file-joiner.md) page.



## Shutter speed, ND filters and motion blur

You should avoid motion blur when recording, read more why in the [📸 **Common filming tips and issues**](../common-filming-tips-and-issues.md).



