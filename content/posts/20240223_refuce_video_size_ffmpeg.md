--- 
draft : false
date : 2024-02-23T14:37:18+01:00
title : "How to Reduce a Video's File Size with FFMPEG"
tags : ["video", "ffmpeg"]
showTableOfContents : true
type: "post"
---

In this tutorial, you'll learn how to reduce a video's file size with very little loss in quality using FFMPEG. We'll discuss what FFMPEG is, how to download and install it in both a Windows and Linux environment, and how to compress your video.

## What is FFMPEG?

FFMPEG is an open-source command-line software used for video recording, converting, compression, and streaming across multiple platforms. It's one of the most reputable compression tools out there and highly efficient.

Make sure you have FFMPEG installed on your machine. You can follow the installation instructions below depending on your environment.

## How to Install FFMPEG

### Windows Installation

If you're using a Windows machine and don't have FFMPEG installed, you can download it from the official FFMPEG website. Once the download completes, run the installation file and follow the on-screen instructions.

### Linux Installation

To install FFMPEG on a Ubuntu Linux environment, make sure you have all of the latest packages installed first:
```bash
sudo apt update
```
Then run the FFMPEG software installation command:
```bash
sudo apt install ffmpeg
```

## Check Your Installation

Once installed in either environment, you can run the following command to ensure the installation completed successfully:
```bash
ffmpeg -version
```
## FFMPEG Parameters

Here is a list of the parameters we'll explore with FFMPEG:

* `-i` - Defines our source file for compression. In this case, input.mp4.
* `-b` - Defines the video's bitrate. Bitrate is the amount of information provided per second. The higher the bitrate, the higher the video quality and smoother transition between frames.
* `-vcodec` - Defines the compression and encoding technology. In this case, `libx265`. H.265 is the current standard, as it compresses more of the video at a higher quality than its predecessor, H.264.
* `-crf` - Defines the Constant Rate Factor or the output quality of the destination video. These values depend on the encoder you're using, but a lower value indicates a higher quality output. For example, a value of 0 indicates a losses compression, while a value of 50 indicates the worst possible quality after compression.

## How to Compress Video with FFMPEG

In this hypothetical scenario, we'll reduce our video file size using H.265 compression, also known as High-Efficiency Video Coding, or MPEG-H Part 2.

Open up your command-line tool and browse to the folder where your video is located. In this example, we'll assume your video is named input.mp4 and create a newly compressed version named `output.mp4`:
```bash
ffmpeg -i input.mp4 -vcodec libx265 -crf 28 output.mp4
```

The command console will output the compression results as it progresses through. Once completed, you'll see your current directory with the ability to input another command.

## Other Video Compression & Conversion Examples

### Convert MP4 to WMV

```bash
ffmpeg -i input.mp4 -b 1000k -vcodec wmv2 -acodec wmav2 -crf 19 -filter:v fps=24 output.wmv
```

### Convert MP4 to WebM for Social Media

```bash
ffmpeg -i input.mp4 -c:v libvpx-vp9 -b:v 0.33M -c:a libopus -b:a 96k \ -filter:v scale=960x540 output.webm
```

### Convert H.264 to H.265 for Higher Compression
```bash
ffmpeg -i input.mp4 -vcodec libx265 -crf 28 output.mp4
```

### Resize Video Width & Height

```bash
ffmpeg -i input.mp4 -vf scale=852x480 output.mp4
````
You can also provide specific sizes like 480p, 720p, 1080p, etc.