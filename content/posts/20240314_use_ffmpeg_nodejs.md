--- 
draft : false
date : 2024-03-14T14:37:18+01:00
title : "How to use FFmpeg in Node.js"
tags : ["Nodejs", "ffmpeg"]
showTableOfContents : true
type: "post"
---

This tutorial will show you how to use the FFmpeg from a Node.js app.

FFmpeg is a versatile open-source tool for encoding video files and performing basic editing operations such as stitching, cropping, and resizing. You probably know it can run from the terminal, but did you know it can also be invoked from an application in Node.js, for example, to automate video processing?

## Installing dependencies

Make sure you install the `ffmpeg-static` and `ffmpeg-fluent` NPM packages first. The `ffmpeg-static` package provides a static build of FFmpeg, so you don't need to worry about installing it manually on the machine where you will deploy your application, and `ffmpeg-fluent` gives us a simple interface to run FFmpeg commands from JavaScript.
```
npm i ffmpeg-static fluent-ffmpeg
```

## Fluent-ffmpeg examples
1. Extracting audio from a video
In order to extract audio from a video file and store it as an MP3, we can use the following code:
```javascript
const ffmpegStatic = require('ffmpeg-static');
const ffmpeg = require('fluent-ffmpeg');

// Tell fluent-ffmpeg where it can find FFmpeg
ffmpeg.setFfmpegPath(ffmpegStatic);

// Run FFmpeg
ffmpeg()

  // Input file
  .input('video.mp4')

  // Audio bit rate
  .outputOptions('-ab', '192k')

  // Output file
  .saveToFile('audio.mp3')

  // Log the percentage of work completed
  .on('progress', (progress) => {
    if (progress.percent) {
      console.log(`Processing: ${Math.floor(progress.percent)}% done`);
    }
  })

  // The callback that is run when FFmpeg is finished
  .on('end', () => {
    console.log('FFmpeg has finished.');
  })

  // The callback that is run when FFmpeg encountered an error
  .on('error', (error) => {
    console.error(error);
```

2. Changing the resolution of a video file

Here's how to resize a video while maintaining its aspect ratio:

```javascript
const ffmpegStatic = require('ffmpeg-static');
const ffmpeg = require('fluent-ffmpeg');

// Tell fluent-ffmpeg where it can find FFmpeg
ffmpeg.setFfmpegPath(ffmpegStatic);

// Run FFmpeg
ffmpeg()

  // Input file
  .input('video.mp4')

  // Scale the video to 720 pixels in height. The -2 means FFmpeg should figure out the
  // exact size of the other dimension. In other words, to make the video 720 pixels wide
  // and make FFmpeg calculate its height, use scale=720:-2 instead.
  .outputOptions('-vf','scale=-2:720')

  // Output file
  .saveToFile('video.mp4')

  // Log the percentage of work completed
  .on('progress', (progress) => {
    if (progress.percent) {
      console.log(`Processing: ${Math.floor(progress.percent)}% done`);
    }
  })

  // The callback that is run when FFmpeg is finished
  .on('end', () => {
    console.log('FFmpeg has finished.');
  })

  // The callback that is run when FFmpeg encountered an error
  .on('error', (error) => {
    console.error(error);
  });
```

3. Removing the audio stream from a video

This is how you can remove the audio track from a video without re-encoding. This operation is quite fast, because it doesn't have to process the video. It just copies the video stream but not the audio:

```javascript
const ffmpegStatic = require('ffmpeg-static');
const ffmpeg = require('fluent-ffmpeg');

// Tell fluent-ffmpeg where it can find FFmpeg
ffmpeg.setFfmpegPath(ffmpegStatic);

// Run FFmpeg
ffmpeg()

  // Input file
  .input('video.mp4')

  // Tell FFmpeg to ignore the audio track
  .noAudio()

  // Copy the video without re-encoding
  .outputOptions('-codec', 'copy')

  // Output file
  .saveToFile('video.mp4')

  // Log the percentage of work completed
  .on('progress', (progress) => {
    if (progress.percent) {
      console.log(`Processing: ${Math.floor(progress.percent)}% done`);
    }
  })

  // The callback that is run when FFmpeg is finished
  .on('end', () => {
    console.log('FFmpeg has finished.');
  })

  // The callback that is run when FFmpeg encountered an error
  .on('error', (error) => {
    console.error(error);
  });
```

4. Extracting images from a video

Here's how to grab the frames from a video and save them as PNG:

```javascript
const ffmpegStatic = require('ffmpeg-static');
const ffmpeg = require('fluent-ffmpeg');

// Tell fluent-ffmpeg where it can find FFmpeg
ffmpeg.setFfmpegPath(ffmpegStatic);

// Run FFmpeg
ffmpeg()

  // Input file
  .input('video.mp4')

  // Optional: Extract the frames at this frame rate
  // .fps(10)

  // Output file format. Frames are stored as frame-001.png, frame-002.png, frame-003.png, etc.
  .saveToFile('frame-%03d.png')

  // Log the percentage of work completed
  .on('progress', (progress) => {
    if (progress.percent) {
      console.log(`Processing: ${Math.floor(progress.percent)}% done`);
    }
  })

  // The callback that is run when FFmpeg is finished
  .on('end', () => {
    console.log('FFmpeg has finished.');
  })

  // The callback that is run when FFmpeg encountered an error
  .on('error', (error) => {
    console.error(error);
  });
```

5. Stitch images into a video file

Like the previous example, but the other way around:

```javascript
const ffmpegStatic = require('ffmpeg-static');
const ffmpeg = require('fluent-ffmpeg');

// Tell fluent-ffmpeg where it can find FFmpeg
ffmpeg.setFfmpegPath(ffmpegStatic);

// Run FFmpeg
ffmpeg()

  // FFmpeg expects your frames to be named like frame-001.png, frame-002.png, etc.
  .input('frame-%03d.png')

  // Tell FFmpeg to import the frames at 10 fps
  .inputOptions('-framerate', '10')

  // Use the x264 video codec
  .videoCodec('libx264')

  // Use YUV color space with 4:2:0 chroma subsampling for maximum compatibility with
  // video players
  .outputOptions('-pix_fmt', 'yuv420p')

  // Output file
  .saveToFile('video.mp4')

  // Log the percentage of work completed
  .on('progress', (progress) => {
    if (progress.percent) {
      console.log(`Processing: ${Math.floor(progress.percent)}% done`);
    }
  })

  // The callback that is run when FFmpeg is finished
  .on('end', () => {
    console.log('FFmpeg has finished.');
  })

  // The callback that is run when FFmpeg encountered an error
  .on('error', (error) => {
    console.error(error);
  });
  ```