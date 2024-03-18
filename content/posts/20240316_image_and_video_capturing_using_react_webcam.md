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

