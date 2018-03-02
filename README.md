# MAX-Example

## Description
Keras is a deep learning library that you can use in conjunction with Tensorflow and several other deep learning libraries. This is Keras model Inception V3, with weights pre-trained on ImageNet. Since its first introduction, Inception has been one of the best performing family of models on the ImageNet dataset 

## Published Docker Image:
```
dockerhub link
```

## Try Out
| Demo Link| 
| ------------- |
| http://abc.com:3000/ | 


## Model 
| Domain | Industry | Framework | Datasets | Data Format | Links | 
| ------------- | --------  | -------- | --------- | --------- | -------------- | 
| Vision | General | Tensorflow and Theano| ImageNet | channels_first, channels_last| https://arxiv.org/abs/1512.00567 |

## Build and Run

### Pre-requisite

* `docker`: The Docker command-line interface (https://www.docker.com/)

* `S3 CLI`: The [command-line interface](https://aws.amazon.com/cli/) to configure your Object Storage

* The minimum recommended capacity for this model is 4GB Memory and 2 CPUs.

*

## Steps

## Deploy the Model 

To build and deploy the model to a REST API using Docker, follow these steps:

### 1. Clone the repo

Clone the `mae-skeleton-keras` repository locally. In a terminal, run the following command:

```
$ git clone https://https://github.com/IBM/MAX.git
```

Change directory into the repository base folder: `$ cd MAX`.

### 2. Build the Model Docker image

To build the docker image locally, run: 

```
$ docker build -t mae-keras -f Dockerfile .
```

The docker image uses the [nvidia/cuda base image](https://hub.docker.com/r/nvidia/cuda/) so will need to pull that from Dockerhub, which may take some time. _Note_ that currently this docker image is CPU only (we will support GPU later).

### 3. Run the Model server

To run the docker image, which automatically starts the model serving API, run:

```
$ docker run -it -v $PWD:$PWD -w $PWD -v $HOME/.keras/models:/root/.keras/models -p 5000:5000 mae-keras
```

When run for the first time, the model files will automatically be downloaded.
```
