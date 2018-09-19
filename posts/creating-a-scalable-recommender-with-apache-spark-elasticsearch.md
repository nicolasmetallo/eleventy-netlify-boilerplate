---
title: Creating a Scalable Recommender with Apache Spark & Elasticsearch
date: 2018-09-05T01:36:41.133Z
summary: >-
  This repo contains a Jupyter notebook illustrating how to use Spark for
  training a collaborative filtering recommendation model from rating data
  stored in Elasticsearch, saving the model factors to Elasticsearch, and then
  using Elasticsearch to serve real-time recommendations using the model. 
tags:
  - elasticsearch
  - spark
  - recommender systems
  - python
  - jupyter notebook
---
I'll be following this [repo](https://github.com/IBM/elasticsearch-spark-recommender) from IBM to build a recommender system.

## Installation

<img src=https://github.com/IBM/elasticsearch-spark-recommender/raw/master/doc/source/images/architecture.png "Architecture" height=50% width=50%>

### 1. Clone the IBM repository

In a terminal, clone the repository

```
$ git clone https://github.com/IBM/elasticsearch-spark-recommender.git
```

#### 2. Setup elasticsearch & elasticsearch vector scoring plugin

For Linux/Mac, use

```
$ wget https://artifacts.elastic.co/downloads/elasticsearch/elasticsearch-5.3.0.tar.gz
$ tar xfz elasticsearch-5.3.0.tar.gz
```

Change directory to the newly unzipped folder

```
$ cd elasticsearch-5.3.0
```

And install the elasticsearch vector scoring plugin to get raw vectors as scores. If you do not JAVA in your computer, you can install it going to their website.

```
$ ./bin/elasticsearch-plugin install https://github.com/MLnick/elasticsearch-vector-scoring/releases/download/v5.3.0/elasticsearch-vector-scoring-5.3.0.zip
```

Next, start Elasticsearch (do this in a separate terminal window in order to keep it up and running):

```
$ ./bin/elasticsearch
```

And finally, run the elasticsearch Python client. If you are having problems with Python and your Mac during installation, reinstall it using brew by doing `brew install python@2`

```
$ pip install elasticsearch
```

#### 3. Download the Elasticsearch Spark connector

```
$ wget http://download.elastic.co/hadoop/elasticsearch-hadoop-5.3.0.zip
$ unzip elasticsearch-hadoop-5.3.0.zip
```

#### 4. Download Apache Spark

```
$ cd ..
$ wget http://apache.dattatec.com/spark/spark-2.3.1/spark-2.3.1-bin-hadoop2.7.tgz
$ tar xfz spark-2.3.1-bin-hadoop2.7.tgz
```

If you don't have Numpy installed, run

```
$ pip install numpy
```

#### 5. Download the data

Run the following commands from the base directory of the Code Pattern repository (IBM repository from step 1):

```
$ cd data
$ wget http://files.grouplens.org/datasets/movielens/ml-latest-small.zip
$ unzip ml-latest-small.zip
```

#### 6. Launch the notebook

To run the notebook you will need to launch a PySpark session within a Jupyter notebook. If you don't have Jupyter installed, you can install it by running the command:

```
$ pip install jupyter
```

Run the following command to launch your PySpark notebook server locally.

```
$ PYSPARK_DRIVER_PYTHON="jupyter" PYSPARK_DRIVER_PYTHON_OPTS="notebook" ../spark-2.3.1-bin-hadoop2.7/bin/pyspark --driver-memory 4g --driver-class-path ../../elasticsearch-hadoop-5.3.0/dist/elasticsearch-spark-20_2.11-5.3.0.jar
```
This should open a browser window with the Code Pattern folder contents displayed. Click on the notebooks subfolder and then click on the ```elasticsearch-spark-recommender.ipynb``` file to launch the notebook.


![alt text](https://github.com/IBM/elasticsearch-spark-recommender/raw/master/doc/source/images/launch-notebook.png "jupyter notebook")
