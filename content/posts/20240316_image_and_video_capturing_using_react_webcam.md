--- 
draft : false
date : 2024-03-16T14:37:18+01:00
title : "Image and Video Capturing With React-Webcam"
tags : ["React", "webcam"]
showTableOfContents : true
type: "post"
---

Video cameras, known as webcams, are useful for capturing live images or movies for your online applications. It is acknowledged to be helpful for related tasks including streaming, instant messaging, video conferencing, and others. Webcams have recently developed into essential tools for both the social and professional spheres.

A small JavaScript package called React-Webcam receives over 113k downloads per week on npm and 1.3k GitHub stars. It supports webcams that are built into or physically attached to mobile and laptop devices. The library has a number of flexible functions that make application integration straightforward.

In this post, we’ll examine how to integrate react-webcam into your program and build a quick web application that uses a webcam to offer the following features:
* Select a camera
* Taking Pictures
* Streamlining Images
* Recording video

## Set up React Project
Use the below instructions to launch the React application to get started.
```
npx create-react-app webcam-app
```

## Install react-webcam

Here, we install `react-webcam` dependencies.
```
cd webcam-app
npm i react-webcam
npm start
```
Next, we adjust App.css to give the application some fundamental styling.

```css
.Container {
  align-items: center;
  min-height: 100vh;
  margin: 0 10px;
  display: flex;
  flex-direction: column;
  justify-content: center;
}

button {
  margin-top: 10px;
  padding: 10px 40px;
  border: none;
  background-color: rgb(6, 191, 233);
  color: #fff;
  border-radius: 25px;
  cursor: pointer;
}

img {
  width: 400px;
  object-fit: cover;
  height: 400px;
}

.App {
  display: flex;
  align-items: center;
  width: 150vw;
  justify-content: center;
  background-color: #262b36;
}
```

## Create the webcam component

We will import the `webcam` component into the `App.js` component in this section. You can as well create your own components under the component folder and import it to the `App.js` just to make your work look neat.

```jsx
import "./App.css";
import Webcam from "react-webcam";


function App() {
  return (
    <div className="App">
      <Webcam />
    </div>
  );
}

export default App;
```
We’ll save, and if we go back to our web app, our screen will display a prompt, and you will select allow.

![image1.webp](/images/2024mars18_1.webp)

By clicking the Allow a window will come up with your device camera capturing you or whatever that’s in the background.

![image2.webp](/images/2024mars18_2.webp)

## React-webcam Props

You can use a variety of props from the `react-webcam` component in your application. We’ll discuss a couple of them below:

### ScreenshotFormat

The screenshot format prop has a string type and the value image/webp by default.

```jsx
<Webcam screenshotFormat='image/webp' />
```

This gives the type of format to save our screenshots.

### VideoConstraints

A sort of object called `videoConstraints` is responsible for managing a video’s “width,” “height,” “aspect ratio,” and “facing mode” attributes. The aspect ratio parameter can flip the camera from portrait to landscape orientation.

```jsx
const videoConstraints = {
aspectRatio: 0.6666666667,
  facingMode: "user",
  width: { min: 480 },
  height: { min: 720 },
 };

...
<Webcam width={480} height={720} videoConstraints={videoConstraints} />
```
![image3.webp](/images/2024mars18_3.webp)

### Mirrored

The webcam’s output is mirrored when the `mirrored` prop, a boolean with a default value of `false`, is used.

```jsx
<Webcam mirrored={false} />
```
![image4.webp](/images/2024mars18_4.webp)

Likewise, when the `mirrored` prop was set to `true`.
```jsx
<Webcam mirrored={true} />
```
![image5.webp](/images/2024mars18_5.webp)

### Audio

The boolean `audio` prop has the default value of `false`. The microphone on the computer is managed by this.
```jsx
<Webcam audio={true} />
```
We will get a prompt asking us to provide access to the computer’s microphone after adding the `audio` prop and changing it to true on the camera component.

![image6.webp](/images/2024mars18_6.webp)

## Image Capturing

In this section, we’ll increase the webcam’s functionality by fusing two or more accessories so that the program can take a live photo. The current webcam image will be returned as a base64-encoded string by the getScreenshot method, which we will use.

The following step is to create a `WebcamImage.js` component, then paste the next code block there:
```jsx
import Webcam from "react-webcam";
import React, { useState, useRef, useCallback } from "react";

function WebcamImage() {
  const webcamRef = useRef(null);
  const [img, setImg] = useState(null);

const capture = useCallback(() => {
    const imageSrc = webcamRef.current.getScreenshot();
    setImg(imageSrc);
  }, [webcamRef]);

  const videoConstraints = {
    width: 390,
    height: 390,
    facingMode: "user",
  };

  return (
    <div className="Container">
      {img === null ? (
        <>
          <Webcam
            screenshotFormat="image/jpeg"
            videoConstraints={videoConstraints}
            audio={false}
            height={500}
            width={500}
            ref={webcamRef}
            mirrored={true}
          />
          <button onClick={capture}>Capture photo</button>
        </>
      ) : (
        <>
          <img src={img} alt="screenshot" />
          <button onClick={() => setImg(null)}>Recapture</button>
        </>
      )}
    </div>
  );
}

export default WebcamImage;
```

Next, we add the following code block to `App.js` to make changes:
```jsx
import "./App.css";
import WebcamImage from "./WebcamImage";

function App() {
  return (
    <div className="App">
      <WebcamImage />
    </div>
  );
}

export default App;
```
![image7.webp](/images/2024mars18_7.webp)

## Video Capturing

The capability of the `react-webcam` will be expanded in this section by recording a live video and saving it to your PC.

The following step is to create a `WebcamVideo.js` component and paste the following code block:
```jsx
const WebcamStreamCapture = () => {
  const webcamRef = React.useRef(null);
  const mediaRecorderRef = React.useRef(null);
  const [capturing, setCapturing] = React.useState(false);
  const [recordedChunks, setRecordedChunks] = React.useState([]);

  const handleStartCaptureClick = React.useCallback(() => {
    setCapturing(true);
    mediaRecorderRef.current = new MediaRecorder(webcamRef.current.stream, {
      mimeType: "video/webm"
    });
    mediaRecorderRef.current.addEventListener(
      "dataavailable",
      handleDataAvailable
    );
    mediaRecorderRef.current.start();
  }, [webcamRef, setCapturing, mediaRecorderRef]);

  const handleDataAvailable = React.useCallback(
    ({ data }) => {
      if (data.size > 0) {
        setRecordedChunks((prev) => prev.concat(data));
      }
    },
    [setRecordedChunks]
  );

  const handleStopCaptureClick = React.useCallback(() => {
    mediaRecorderRef.current.stop();
    setCapturing(false);
  }, [mediaRecorderRef, webcamRef, setCapturing]);

  const handleDownload = React.useCallback(() => {
    if (recordedChunks.length) {
      const blob = new Blob(recordedChunks, {
        type: "video/webm"
      });
      const url = URL.createObjectURL(blob);
      const a = document.createElement("a");
      document.body.appendChild(a);
      a.style = "display: none";
      a.href = url;
      a.download = "react-webcam-stream-capture.webm";
      a.click();
      window.URL.revokeObjectURL(url);
      setRecordedChunks([]);
    }
  }, [recordedChunks]);

  return (
    <>
      <Webcam audio={false} ref={webcamRef} />
      {capturing ? (
        <button onClick={handleStopCaptureClick}>Stop Capture</button>
      ) : (
        <button onClick={handleStartCaptureClick}>Start Capture</button>
      )}
      {recordedChunks.length > 0 && (
        <button onClick={handleDownload}>Download</button>
      )}
    </>
  );
};
```
After that, we edit `App.js` by pasting the following code block:
```jsx
import "./App.css";
import WebcamVideo from "./WebcamVideo";

function App() {
  return (
    <div className="App">
      <WebcamVideo/>
    </div>
  );
}

export default App;
```
![image7.gif](/images/2024mars18_7.gif)

Output

![image8.gif](/images/2024mars18_8.gif)