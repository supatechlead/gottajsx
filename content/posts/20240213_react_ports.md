--- 
draft : false
date : 2024-02-13T14:37:18+01:00
title : "Specify a Port for React Project"
tags : ["React"]
showTableOfContents : true
type: "post"
---

## Using package.json

An option is to modify the `scripts` part of `package.json` from:
```json
"start": "react-scripts start"
```

to

Linux (tested on Ubuntu 14.04/16.04) and MacOS (tested by aswin-s on MacOS Sierra 10.12.4):
```json
"start": "PORT=3006 react-scripts start"
```
or (may be) more general solution 
```json
"start": "export PORT=3006 react-scripts start"
```
## Using .env file

Create a file with name `.env` in the main directory besides `package.json` and set `PORT` variable to desired port number.

For example:

> .env
```
PORT=4200
```
You can find the documentation for this action here: https://create-react-app.dev/docs/advanced-configuration