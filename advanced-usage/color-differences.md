# 🎨 Color differences

**`TODO:`**` ``Cleanup this page`

color range is either Full/JPEG (0-255) or Limited/MPEG (16-235), and is generally in metadata&#x20;

Gyroflow tries really hard to maintain all original video parameters, but there is a huge mess with color range metadata across video players, editors, and tools&#x20;

in Resolve, you can go to clip attributes and change Color range to Full or Video and it may look different in different players or video editors, you can search online about color range issues it will depend on your exact case and environment so you'll have to figure out a workflow that works for you

you have to test the colors on all stages of your editing and make the changes as necessary your original file from gopro can also be interpreted in 2 different ways there is no rule to this, your file may look the same or different at all stages: original file, processed in gyroflow, playing in VLC, playing in resolve, exported from resolve and played in VLC, uploaded to youtube etc&#x20;

in addition to the color range, there's also a gamma curve, which also makes a difference at different stages and for example on youtube&#x20;

google a common problem with colors looking different on the exported file, and in the uploaded video on youtube (not related to gyroflow)

but there is such a mess that there are multiple workarounds and different rules that it's impossible to be certain across applications

[https://cinelerra-gg.org/download/CinelerraGG\_Manual/Color\_Space\_Color\_Range\_Aff.html](https://cinelerra-gg.org/download/CinelerraGG\_Manual/Color\_Space\_Color\_Range\_Aff.html)\
\
also there are bugs, eg video encoder on Intel macOS basically ignores the requested color range

[https://referencehometheater.com/2014/commentary/rgb-full-vs-limited/](https://referencehometheater.com/2014/commentary/rgb-full-vs-limited/)\
\
[https://coloraggio.github.io/davinci-resolve-manuals/12.5\_Manual/6.pdf](https://coloraggio.github.io/davinci-resolve-manuals/12.5\_Manual/6.pdf) "Data Levels Settings and Conversions"\
\
it's best to use the OpenFX plugin to avoid this issue
