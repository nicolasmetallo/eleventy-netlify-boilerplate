---
title: Detect emotion and gender in video streams using TensorFlow
date: 2018-09-19T15:57:57.685Z
summary: >-
  Implementation of a simple script to run emotion and gender detection on video
  streams using Python, TensorFlow, and OpenCV.
tags:
  - emotion
  - gender
  - computer vision
  - tensorflow
---
# Implementation of emotion and gender detection using TensorFlow
Please take a look at the [original repository](https://github.com/oarriaga/face_classification) and to their [publication](https://github.com/oarriaga/face_classification/blob/master/report.pdf)

# Real-time demo:
Real-time demo:
![alt text](https://i.imgur.com/wozpPdN.gif "Logo Title Text 1")

## Instructions for Google Colab

### Clone this repo:
```
$ git clone https://github.com/nicolasmetallo/emotion_and_gender_detection.git
```

### Pre-requisites:
Run the following to install the required modules
```
$ pip install -r face_classification/REQUIREMENTS.txt
```

### Install FFMPEG for Google Colab:
```
$ apt install ffmpeg
```

### Set root dir
```
$ cd face_classification/src
```

### Example usage
This script requires four inputs:

- input_video = 'Path to input video'
- path_to_extract = 'Path for the frames extracted from the video'
- path_to_predict = 'Path for the predictions made by our model'
- output_video = 'Path to output video'
```
$ python run_demo_on_video.py {input_video} {path_to_extract} {path_to_predict} {output_video}
```
Example using the demo video in the repo
```
$ python run_demo_on_video.py emotion-gender-demo.mp4 \
      /content/emotion_and_gender_detection/extracted_frames \
      /content/emotion_and_gender_detection/predicted_frames \
      output_video.mp4
```
