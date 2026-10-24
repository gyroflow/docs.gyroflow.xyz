# 💻 Other

## Full screen mode

Press **F** or **F11** to enlarge the video. The first press hides the side panels, the second press switches to a real full screen and also hides the timeline, and the third press goes back. **Esc** exits the full screen mode. Double-clicking the video also toggles the first mode.

## Stabilization overview

The **Toggle stabilization overview** button below the video (or **D**) zooms out the preview, so you can see the whole stabilized frame and what the stabilization is doing. It only affects the preview.

**Ctrl + click** on that button while the overview is on (Command on macOS), or **V**, switches to the **split view**: the overview and the final framing are shown side by side (or one above the other, for vertical videos). The split view is remembered.

## Advanced settings

These settings are in the **Advanced** panel on the right.

* **Preview resolution** - the resolution of the preview. Lower it if the preview isn't smooth. It doesn't affect the export.
* **Background mode** - what's shown where the stabilized frame doesn't cover the output (e.g. with **No zooming**, or FOV over 1):
  * **Solid color** - the **Render background** color.
  * **Repeat edge pixels** and **Mirror edge pixels** - fill the area with the edge of the video.
  * **Margin with feather** - blends the edges of the video into a fill taken from further inside the frame, set with **Margin** and **Feather**. Both can be [🔑 keyframed](keyframes.md).
* **Render background** - the background color, as `#RRGGBB`, or `#AARRGGBB` with transparency. See [Transparent videos](other.md#transparent-videos).
* **Theme**, **UI scaling** and **Language** - the look of the app.
* **Safe area guide** - when FOV is over 1, shows a rectangle of the area FOV 1 would show. It's only a guide in the preview, it doesn't affect the export.
* **Use GPU decoding** - decodes the video on the GPU (hardware decoding). Disable it if a video doesn't load or shows corrupted frames.
* **Format for R3D conversion**, **Colors for R3D conversion**, **Gamma curve for R3D conversion**, **Color space for R3D conversion** and **Additional REDline params** - the settings of the ProRes file made with _REDline_ when exporting R3D files. They show up only when REDCINE-X is installed. See [RED -> Exporting](../getting-started/supported-cameras/red.md#exporting).
* **Preview pipeline** - how the preview is processed:
  * **Zero-copy Qt RHI** (default) - the fastest, the video never leaves the GPU.
  * **Zero-copy OpenCL** - like the default, but processed with OpenCL.
  * **OpenCL/wgpu/CPU** - copies the frames between the GPU and the CPU. It's slower, but try it if the preview doesn't work.
* **Device for video processing** - the GPU (or **CPU only**) used for the stabilization, both in the preview and in the export. The GPU used for encoding is set separately in `Export settings -> Advanced -> Device for rendering`.
* **Default file suffix** - added to the name of the exported files (default `_stabilized`).
* **Notification sounds** - plays a sound with success and error messages, e.g. when an export is finished.
* **Reset all settings to default** - clears all settings (your favorite lens profiles are kept). Restart Gyroflow afterwards.
* **Open data folder** - opens the folder with Gyroflow's settings, logs and downloaded lens profiles.

## Transparent videos

Set **Render background** to a color with transparency, e.g. `#00000000`, and export to a format that supports it: **PNG sequence**, **EXR sequence**, or **ProRes 4444** / **4444 XQ** encoded on the CPU. The areas outside the stabilized frame are then transparent.

## Render queue

The render queue is saved automatically. If Gyroflow is closed (or crashes) with unfinished items, they're restored the next time you start it. See [⏩ Render queue](render-queue.md).

## Sleep while rendering

While exporting or analyzing a video, Gyroflow prevents the computer from going to sleep (on phones and tablets, the screen stays on).
