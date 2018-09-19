---
title: Image Recognition with NVIDIA Digits in Amazon ECS Docker Containers
date: 2018-09-19T23:18:33.573Z
summary: >-
  Step by Step tutorial to run NVIDIA Digits in an Amazon EC2 instance and
  set-up an image recognition pipeline.
tags:
  - nvidia
  - digits
  - amazon
  - ec2
  - docker
---
# Image Recognition Using GPUs in Amazon ECS Docker Containers

**Rev. 1.0** 
**Author:** Nicolas Metallo 
**Date:** March 28th, 2017.

## Step by Step Guide
#### Amazon EC2 Instance
- Log-in with your credentials to http://aws.amazon.com and go to the EC2 Dashboard
- Make sure you are in U.S. East N. Virginia and select “Launch Instance”
- Now we choose “Community AMIs” and look for ami-b1e2c4a6
- Set storage to 60 Gb and set ports 22 (ssh), 5000 and 8000 open to anywhere
- Create a new key {your-key}.pem
- Go back to you EC2 dashboard and copy the public IP
- Log into your machine through ssh using the Terminal
```sh
$ ssh -i {your-key}.pem ubuntu:{your-ip}
```

#### Update NVIDIA Drivers and CUDA
- Check if everything is working
```sh
$ nvidia-docker run --rm nvidia/cuda nvidia-smi
```
- You should see a response like this (the driver will be 364 as set in the AMI)
- Reboot
- We uninstall our previous NVIDIA drivers and Reboot again
```sh 
$ apt remove --purge nvidia*
```
- Download “CUDA 8*.run” installer to the /data folder
```sh 
$ wget -P /data/ https://developer.nvidia.com/compute/cuda/8.0/Prod2/local_installers/cuda_8.0.61_375.26_linux-run
```
- Install CUDA 8 with the following parameters (around ~20 mins)
```sh 
$ cd /data/
$ chmod +x cuda*
$ ./cuda* -silent
```
- Reboot
- Install “nvidia-docker” again
```sh
$ wget -P /tmp https://github.com/NVIDIA/nvidia-docker/releases/download/v1.0.1/nvidia-docker_1.0.1-1_amd64.deb
sudo dpkg -i /tmp/nvidia-docker*.deb && rm /tmp/nvidia-docker*.deb
```
- Let’s check again if everything is working alright
```sh 
$ nvidia-docker run --rm nvidia/cuda nvidia-smi
```
Download the Stanford Dog Dataset:
```sh
$ cd /opt/
$ mkdir dataset-stanford
$ cd dataset-stanford
$ wget http://vision.stanford.edu/aditya86/ImageNetDogs/images.tar
$ wget http://vision.stanford.edu/aditya86/ImageNetDogs/annotation.tar
$ tar -xvf images.tar
$ tar -xvf annotation.tar
```
- Run “NVIDIA DIGITS 5” mapping the folder /opt to /data in order to use our uploaded datasets
```sh
$ nvidia-docker run --name digits -d -p 5000:5000 -v /opt/:/data/ nvidia/digits
```
- Browse to {my ip address}:5000 and log into NVIDIA DIGITS


#### Using NVIDIA DIGITS 5
- First let’s create a new dataset. Go to ‘New dataset’-’Images’-’Classification’
- Set the path to your dataset under ‘Training Images’ and leave the rest of the options as it is. Set Group and Dataset names
- Create the dataset
- We can explore the dataset (train_db)
- Let’s create now the classification model by going to ‘New Model’-’Images’-’Classification’
- Outlook of how settings should look like. We set number of epochs (iterations), base learning rate (lower for pretrained models).
- Run a cURL command to get a response from our model for a single-image image classification. Pay attention to our server path, jobs_id, imag_file.

#### Run NVIDIA GPU REST Engine (GRE)
- Go to you /data folder and clone NVIDIA git repository
```sh
$ cd /data/
$ git clone https://github.com/NVIDIA/gpu-rest-engine
```
- Now modify lines 74-78 from file Dockerfile.inference_server and refer them to the files you copied before
```sh
sudo nano Dockerfile.inference_server 
```
- Modify to account only for p2.xlarge (NVIDIA Tesla K80) and speed up the installation
```sh
$ ENV CUDA_ARCH_BIN "30 35 50 52 60" to ENV CUDA_ARCH_BIN "37"
```
From
```sh
$ CMD ["inference", \
	     "/caffe/models/bvlc_reference_caffenet/deploy.prototxt", \
	     "/caffe/models/bvlc_reference_caffenet/bvlc_reference_caffenet.caffemodel", \
	     "/caffe/data/ilsvrc12/imagenet_mean.binaryproto", \
	     "/caffe/data/ilsvrc12/synset_words.txt"]
```
To
```sh
$ CMD ["inference", \
	     "/opt/models/dogs_googlenet/deploy.prototxt", \
	     "/opt/models/dogs_googlenet/dogs_googlenet.caffemodel", \
	     "/opt/models/dogs_googlenet/mean.binaryproto", \
	     "/opt/models/dogs_googlenet/labels.txt"]
```
- Press Control + O to save it and then Control + X to exit
- Run the build command (this will take a while)
```sh
$ docker build -t inference_server -f Dockerfile.inference_server .
```
- Execute the following command and wait a few seconds for the initialization of the Caffe classifiers:
```sh
$ nvidia-docker run --name=server --net=host --rm inference_server
```
Test if everything is working by classifying one image
```sh
$ curl -XPOST --data-binary @images/1.jpg http://54.205.220.105:5000/api/classify
```
