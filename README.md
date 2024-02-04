# Introduction

A set of command lines examples to help debugging video streaming files like mp4, ts ,fmp4 in Dash, HLS, or MSS, with or without DRM.

# The tools

* [FFprobe](https://ffmpeg.org/ffprobe.html) - A generic tool for media streaming debug
* [Mediainfo](https://mediaarea.net/en/MediaInfo) - A generic tool for media streaming info
* [TSDuck](https://tsduck.io/#cmdlist) - A tool more specific for TS (MPEG-2 Part 1)
* [Bento4](https://www.bento4.com/) - A tool more specific for MP4 (MPEG-4 Part 14)


## Overview of the streams

```bash
ffprobe -i file.extension

# trace will output like a parser, it's useful for learning also check container structure such as fragmented mp4, mpegts programs...
ffprobe -v trace file.extension

mediainfo file.extension

tsdump file.ts
tsanalyze file.ts
tstables file.ts
# .. tsXXXX

mp4info file.mp4
mp4dump  # this is useful for fragmented mp4
# mp4XXXXX
```

## Specific info from the streams

```bash
# -select_streams v is filtering only video streams
ffprobe -loglevel panic -select_streams v -show_entries "stream=start_pts,start_time,avg_frame_rate,r_frame_rate,codec_time_base" file.extension

# logging/printing HDR metada information from FFmpeg
# source: https://www.reddit.com/r/ffmpeg/comments/qidrc8/comment/i9lrj9u/?utm_source=reddit&utm_medium=web2x&context=3
ffprobe -loglevel quiet -read_intervals "%+#2" -select_streams v:0 -show_entries side_data "input.mkv" | egrep -m 1 -A 10 'Mastering display metadata' | grep -v 'Mastering display metadata' >/tmp/variables.txt
ffprobe -loglevel quiet -read_intervals "%+#2" -select_streams v:0 -show_entries side_data "input.mkv" | egrep -m 1 -A 2 'Content light level metadata' | grep -v 'Content light level metadata' >>/tmp/variables.txt
cat /tmp/variables.txt
```
## Specific info from some frames

```bash
ffprobe -loglevel panic -select_streams v -show_entries "frame=pkt_pts,pkt_pts_time,pkt_duration,best_effort_timestamp,best_effort_timestamp_time" -read_intervals %+#5
```

## Codec/Container parsing-like info from the stream

```bash

mediainfo --Details=1 file.extension

```

## Video/Audio/ClosedCaptions/Container/Codec ffmpeg samples

https://samples.ffmpeg.org/

