--- 
draft : false
date : 2024-03-15T14:37:18+01:00
title : "Using react-webcam in a Function Component"
tags : ["React", "webcam"]
showTableOfContents : true
type: "post"
---

## Installing the package

You can install the `react-webcam` with one of the following commands based on your package manager.

```shell
//With Yarn
Yarn add react-webcam
//With npm
npm install react-webcam
```
## Referencing the package

You can reference the package as follows:
```jsx
import Webcam from 'react-webcam';
```

## Writing the Function component

### Starting and Stoping the webcam feed

If we need to toggle the webcam with a button click or a similar event, then we’ll have to render the webcam element conditionally. We can achieve this with the help of a boolean state variable. See the code snippet below:
```jsx
<div className="camView">
{
  isShowVideo &&
  <Webcam audio={false} ref={videoElement} videoConstraints=        {videoConstraints} />
}
</div>
```
For an instance, we can implement an event handler binding to the click event of a button, and then update the state variable to toggle the conditional rendering of the webcam element. However, in my example I used two buttons for starting and stopping the cam respectively.

We’ll have to use the `useState` and `useRef` hooks for this implementation. We need the `useState` hook for the boolean state variable which we use for the conditional rendering of the webcam. The `useRef` hook is used to get hold of the webcam element inside the event handler to stop the camera feed.

Finally, let’s have a look at the code

```jsx
import React, { useRef, useState } from 'react';
import Webcam from 'react-webcam';

function WebcamSample() {

    const [isShowVideo, setIsShowVideo] = useState(false);
    const videoElement = useRef(null);
    
    const videoConstraints = {
        width: 640,
        height: 480,
        facingMode: "user"
    }

    const startCam = () => {
        setIsShowVideo(true);
    }

    const stopCam = () => {
        let stream = videoElement.current.stream;
        const tracks = stream.getTracks();
        tracks.forEach(track => track.stop());
        setIsShowVideo(false);
    }

    return (
        <div>
            <div className="camView">
                {isShowVideo &&
                    <Webcam audio={false} ref={videoElement} videoConstraints={videoConstraints} />
                }
            </div>
            <button onClick={startCam}>Start Video</button>
            <button onClick={stopCam}>Stop Video</button>
        </div>
    );
};

export default WebcamSample;
```