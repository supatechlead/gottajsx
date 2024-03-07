--- 
draft : false
date : 2024-02-26T14:37:18+01:00
title : "How to Split a Video Into Multiple Parts With FFmpeg"
tags : ["video", "ffmpeg"]
showTableOfContents : true
type: "post"
---

Video splitting is an important technique for controlling the size and flow of videos. Depending on the use case, we can split a video file based on time, number of frames, or size of chunks.

In a Linux environment, we can split a video into multiple parts using tools like FFmpeg. FFmpeg provides efficient and versatile ways to process multimedia files and is available on a wide range of operating systems like Linux, macOS, and Windows.

In this tutorial, we’ll learn to split a video into multiple parts with FFmpeg.

## Environment Setup
Before moving forward, let’s make sure that we have all the prerequisites ready, including FFmpeg and a sample video.

### FFmpeg Installation
To begin with, we verify that we have FFmpeg installed on our system:

```bash
ffmpeg -version
```
In case it’s not already available, we can install FFmpeg via the local package manager:

```bash
sudo apt install ffmpeg
```
In addition to FFmpeg, we’ll also need a sample video file to split it into multiple parts.

###  Sample Video

Let’s get a sample video file using the `wget` command:
```bash
wget https://sample-videos.com/video123/mp4/480/big_buck_bunny_480p_5mb.mp4
```
We’ll use the downloaded video file to learn about splitting using FFmpeg.

## Splitting a Video at Regular Time Intervals

In general, we split a video into multiple parts to extract smaller chunks of that video.
To begin with, we’ll split a video into multiple parts, each lasting the exact same amount of time:
```bash
ffmpeg -i big_buck_bunny_480p_5mb.mp4 -acodec copy -f segment -segment_time 10 -vcodec copy -reset_timestamps 1 -map 0 output_time_%d.mp4
```

This command takes an input video file, `big_buck_bunny_480p_5mb.mp4`, and splits it into 10-second parts.

Let’s understand each option:

* `-i` specifies the input file, in our case, `big_buck_bunny_480p_5mb.mp4`
* `-acodec copy` sets the audio codec for the output to copy, which means the audio stream will be copied without re-encoding
* `-f segment` sets the format to segment
* `-segment_time 10` specifies the duration of each segment to 10 seconds
* `-vcodec copy` sets the video codec for the output to copy, which means the video stream will be copied without re-encoding
* `-reset_timestamps 1` resets timestamps for each segment and creates segments with continuous timestamps
* `-map 0` maps all the streams from input to the output
* `output_time_%d.mp4` defines the naming pattern for the output files, where `%d` in the naming pattern is a placeholder for a numeric index

As a result of this command, we get three (3) segments of the input video, `big_buck_bunny_480p_5mb.mp4`, with an approximate duration of 10 seconds each.

However, it’s common that not all of the resulting segments are exactly 10 seconds. The reason behind this is the complex nature of a video file. Multiple factors are at play here, including keyframe alignment, audio-video synchronization, and variable frame rate.

Therefore, re-encoding the video before the segmentation may serve as a better approach enabling more accurate control over the segment duration.

## Splitting by Re-Encoding

Re-encoding a video may result in an increase or decrease in size, depending on the quality of the input video and the codec libraries used for re-encoding.

To begin with, we’ll re-encode the video via FFmpeg followed by splitting it into multiple parts:
```
ffmpeg -i big_buck_bunny_480p_5mb.mp4 -c:v libx264 -c:a aac -strict experimental -b:a 192k -force_key_frames "expr:gte(t,n_forced*10)" -f segment -segment_time 10 -reset_timestamps 1 -map 0 output_part_%d.mp4
```
Firstly, this command re-encodes the given input video with audio and video codecs. Secondly, it forces keyframes at 10-second intervals. Lastly, it segments the video into parts, each lasting for 10 seconds.

Let’s understand the new parameters used in this command:

* `-c:v libx264` sets the video codec to libx264
* `-c:a aac` sets the audio codec to AAC
* `-strict experimental` is needed when using the AAC audio codec
* `-b:a 192k` sets the audio bitrate to 192 kbps
* `-force_key_frames` forces keyframes at regular intervals set by the following expression
* `“expr:gte(t,n_forced*10)”` forces a keyframe every 10 seconds (time t)

With this command, the last split part may have a shorter duration than the specified 10 seconds.

## Splitting a Video at Regular Keyframe Intervals

Similar to splitting at a regular time interval, splitting at regular keyframe intervals can be tricky. Thus, we’ll split the video file by re-encoding it:
```
ffmpeg -i big_buck_bunny_480p_5mb.mp4 -c:v libx264 -c:a aac -strict experimental -b:a 192k -force_key_frames "expr:gte(n,200*trunc(t/200))"  -f segment -segment_frames $(seq 200 200 600 | paste -sd "," -) -reset_timestamps 1 -map 0 output_partf_%d.mp4
```

This command effectively splits the input video file, `big_buck_bunny_480p_5mb.mp4`, at every 200 keyframes.

The parameter `-force_key_frames “expr:gte(n,200*trunc(t/200))”` ensures that a keyframe is forced at every 200 frames. Let’s take a close look at the expression:

* `gte(n,200*trunc(t/200))` expression evaluates whether the current frame number `n` is greater than or equal to the calculated keyframe index
* `200 * trunc(t/200)` determines the keyframe index, where `t` represents the time in seconds

With the `-segment_frames` option, we can specify the frame numbers at which to split the video. In the above command, we used `seq` to generate a sequence of numbers from 200 to 600 in increments of 200 and the paste command to join the numbers with a comma separator.

We can also manually specify the frame numbers (i.e., 200, 400, 600) instead of generating them using `seq` in the above command, achieving the same result.

Moreover, we can force keyframes by any frame index that is a factor of the target keyframe interval, in this case, 200.

## Extracting the Duration of a Video

Since video duration in chronological time can be inconsistent, we can use both seconds and frames as our video measurements.

### Chronological Units

First, let’s find the duration of one of the first part we split using FFprobe:
```
ffprobe -v error -show_entries format=duration -of default=noprint_wrappers=1:nokey=1 output_part_1.mp4 
10.000000
```

Let’s break down this command to understand each parameter:

* `-v error` sets the verbosity level to error
* `-show_entries format=duration` specifies the information to be displayed, in our case, the duration of the multimedia file
* `-of default=noprint_wrappers=1:nokey=1` specifies the output format
* `output_part_1.mp4` is the name of the input multimedia file

Similarly, we can check the duration of each of the split parts with ffprobe.

### Frames

Now, let’s find the total number of frames in one of the keyframe-split videos using ffprobe:
```
ffprobe -v error -select_streams v:0 -show_entries stream=nb_frames -of default=nokey=1:noprint_wrappers=1 output_partf_0.mp4 
200
```

Thus, the first split part has 200 frames. In this command, `-select_streams v:0` specifies and selects the video stream with index 0. The `-show_entries stream=nb_frames` option specifies the information to be displayed, i.e., the total number of frames available in an input video file.

Similarly, we can use `ffprobe` to find the total number of frames in each of the split videos by changing the input file name.