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

## 1. Deploy the Model 

To build and deploy the model to a REST API using Docker, follow these steps:

### 1.1 Clone the repo

Clone the `mae-skeleton-keras` repository locally. In a terminal, run the following command:

```
$ git clone https://https://github.com/IBM/MAX.git
```

Change directory into the repository base folder: `$ cd MAX`.

### 1.2 Build the Model Docker image

To build the docker image locally, run: 

```
$ docker build -t mae-keras -f Dockerfile .
```

The docker image uses the [nvidia/cuda base image](https://hub.docker.com/r/nvidia/cuda/) so will need to pull that from Dockerhub, which may take some time. _Note_ that currently this docker image is CPU only (we will support GPU later).

### 1.3 Run the Model server

To run the docker image, which automatically starts the model serving API, run:

```
$ docker run -it -v $PWD:$PWD -w $PWD -v $HOME/.keras/models:/root/.keras/models -p 5000:5000 mae-keras
```

When run for the first time, the model files will automatically be downloaded.

### 1.4 Test the API

The API server automatically generates an interactive Swagger documentation page. Go to `http://localhost:5000` to load it. From there you can explore the API and also create test requests.

Use the `model/predict` endpoint to load a test image (you can use one of the test images from the `assets` folder) and get predicted labels for the image from the API.

You can also test it on the command line, for example:

```
$ curl -F "image=@assets/dog.jpg" -XPOST http://127.0.0.1:5000/model/predict
```

You should see a JSON response like that below:

```json
{"status": "ok", "predictions": [{"label": "beagle", "probability": 0.9201778173446655, "label_id": "n02088364"}, {"label": "Walker_hound", "probability": 0.010086667723953724, "label_id": "n02089867"}, {"label": "English_foxhound", "probability": 0.009787781164050102, "label_id": "n02089973"}, {"label": "bluetick", "probability": 0.006095303222537041, "label_id": "n02088632"}, {"label": "Eskimo_dog", "probability": 0.0025898281019181013, "label_id": "n02109961"}]}
```

# Development

To run the Flask API app in debug mode, edit `app.py` to set the last line to:

```python
app.run(debug=True, host='0.0.0.0')
```

You will then need to rebuild the docker image (following [step 2](#2-build-the-model-docker-image) and [step 3](#3-run-the-model-server)).

