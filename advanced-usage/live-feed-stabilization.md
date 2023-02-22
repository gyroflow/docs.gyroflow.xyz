# 📺 Live feed stabilization

Stabilization of a live (real-time) feed is something Gyroflow would like to support eventually. It's not available yet, mostly due to lack of standards for sending gyro data.

Some discussion about live stabilization is available at [#dev-live-stabilization](https://discord.com/channels/797044698682228736/1055194134622765077) channel on [Discord](https://discord.com/invite/BBJ2UVAr2D).

1. Currently Gyroflow is not designed to do live stabilization
2. There is no standard IMU data transmission, so that is most important part to figure out or standardize or have any examples if you want to push this topic further
3. For testing, we can try with synthetic "live" data, ie. take a standard GoPro file with gyro data, and stream it from another app, stream the IMU data and pretend it's live. Then work on supporting that in Gyroflow
4. There is no timeline on when that would be supported, mainly due to not enough concrete data. Everyone's setup is different so I don't have anything to work on at this moment. This has to be figured out
5. One potential solution is to create an OBS plugin which does the stabilization using [`gyroflow_core`](../technical-details/gyroflow-core.md)``
6. Another one is to open RTSP stream in the Gyroflow app (`mdk-sdk` already supports that) and render it in the UI with zero-copy Qt-RHI, and use the true full screen mode (F11 key twice) - this way it could be streamed further over HDMI (acting like a monitor but connected to HDMI capture card)

### Contributions are welcome!
