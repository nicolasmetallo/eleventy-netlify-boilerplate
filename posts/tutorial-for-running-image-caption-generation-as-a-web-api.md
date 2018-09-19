---
title: Tutorial for running Image Caption Generation as a Web API
date: 2018-09-19T23:14:45.896Z
summary: >-
  This repository contains an implementation of typical image caption generation
  based on neural network (i.e. CNN + RNN). The model first extracts the image
  feature by CNN and then generates captions by RNN. CNN is ResNet50 and RNN is
  a standard LSTM.
tags:
  - image caption
  - generation
  - CNN
  - RNN
  - API
---
# Image Caption Generation as a Web API

**Rev. 1.0** 

**Author:** Nicolas Metallo 

**Date:** May 6th, 2017

## Step by Step Guide
#### Install OpenCV
- Using the previous EC2 Instance we launched ami-b1e2c4a6 we will launch our image caption generation
- You can install OpenCV from the Ubuntu or Debian repository. 
```sh
$ sudo apt-get install libopencv-dev python-opencv
```
- For more info, visit this link or follow these instructions.
- Test it by typing in the command-line
```sh
$ python
$ Import cv2
```

#### Clone GIT repository and setup Web API
- Clone GIT repository on your data/ folder
```sh
$ cd /data/
$ git clone https://github.com/apple2373/chainer-caption
```
- Install chainer version 1.19.0
```sh
$ pip install chainer==1.19.0
```
- Download installer from Anaconda website and run it
```sh
$ cd /data/
$ mkdir anaconda
$ cd anaconda
$ wget https://repo.continuum.io/archive/Anaconda2-4.3.1-Linux-x86_64.sh
$ bash Anaconda2-4.3.1-Linux-x86_64.sh 
```
- Go back to the GIT repository we cloned and download models and other preprocessed files
```sh
$ cd /data/chainer-caption
$ bash download.sh
```
- You can now generate the captions running
```sh
$ python sample_code_beam.py \
	--rnn-model ./data/caption_en_model40.model \
	--cnn-model ./data/ResNet50.model \
	--vocab ./data/MSCOCO/mscoco_caption_train2014_processed_dic.json \
	--gpu -1 \
	--img ./sample_imgs/COCO_val2014_000000185546.jpg \
```
- To run the caption generation module as a Web API do the following
```sh 
$ cd webapi
	python server.py --rnn-model ../data/caption_en_model40.model \
	--cnn-model ../data/ResNet50.model \
	--vocab ../data/MSCOCO/mscoco_caption_train2014_processed_dic.json \
	--gpu -1 \
```
To make a request to our model use
```sh
$ curl -X POST -F image=@cat1.jpg http://54.205.220.105:8090/predict
```
