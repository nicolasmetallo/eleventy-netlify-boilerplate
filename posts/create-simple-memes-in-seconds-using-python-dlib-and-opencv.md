---
title: 'Create simple memes in seconds using Python, Dlib, and OpenCV'
date: 2018-09-08T23:05:32.968Z
summary: >-
  I put together this simple script that switch faces between two input images
  to generate quick 'Eameo'-type memes.
tags:
  - computer
  - vision
  - face swap
  - dlib
  - python
  - opencv
---
# eameo-faceswap-generator
Inspired by the amazing work done daily by [Eameo](https://twitter.com/EameoOk), I put together this simple script that
switch faces between two input images. You can run the step-by-step ipynb in [Google Colab](https://colab.research.google.com)
or run ```python faceswap.py``` directly.
# Installation
This script supports Python 2.7 and 3.6.
## Clone this repository
````
$ git clone https://github.com/nicolasmetallo/eameo-faceswap-generator.git
````
## Pre-requisites
Run the following to install required modules
````
$ pip install -r requirements.txt
````
# Example Usage
This script requires the user to input three file paths which can be local paths or URLs:
- from_image = 'Path to first image where you will extract the face.'
- to_image = 'Path to second image where you will swap the existing face'
- output_filename = 'Path to output image.'

Now let's take two input images and set ```result.jpg``` as the output file.
```
$ python faceswap.py input1.jpg input2.jpg result.jpg
```
<img src="https://i.imgur.com/Z4oac7a.png">
