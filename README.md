# MAX-Example

## Description
The model takes an image and detects and localizes people and their parts: face, head-shoulder, body, torso, and legs within the image.

## Published Docker Image:
```
dockerhub link
```

## Applications and Demos
| Application | Demo Link| 
| ------------- | --------  | ----------- |
| Viusal People Comprehension | http://abc.com:3000/ | 


## Models
| Domain | Industry | Framework | Type | Datasets | Supported Methods* | links | 
| ------------- | --------  | -------- | --------- | --------- | -------------- | --------------- |
| Vision | General | Faster RCNN and PyCaffe | GoogLeNet | Pascal VOC | Inference/Test | external link |

Supported methods can be Train, Interence/Test, Update/Transfer, Embeddings, etc

## Build and Run

### Pre-requisite

* `docker`: The Docker command-line interface (https://www.docker.com/)

* `S3 CLI`: The [command-line interface](https://aws.amazon.com/cli/) to configure your Object Storage

* The minimum recommended capacity for this model is 4GB Memory and 2 CPUs.

*

## Steps

### Build the Docker Image:
```
build
```
### Run the Docker Container:
```
up
```
### Shutdown the Docker Container:
```
down
```

## API Method : Inference

### Command
```
curl -F "images=@<your-images>" -XPOST http://<your-server>:55555/recognize?api_key=<a_key>
```
Analyze posted image(s) `<your-images>` and yield results in JSON output. `<your-images>` can be a single image file or a zip file of several image files. `<your-images>` are processed by `/detect_people` API first to producing a set of regions conatining a bounding box and object type (face, head-shoulder, pedestrian, torso and legs) for each region.

For development, you can specify any text for `<a_key>`.
i.e.
```
curl -F "images=@1.jpg" -XPOST http://smithgpu06.pok.ibm.com:55555/recognize?api_key=zzz
```


The Frontend API wraps results from Internal APIs as below:

```
{
    "images": [
        {
            "results": {
            ...
            ...
            ...
                        },
            "Image": "1.jpg"
        }
    ],
    "images_processed": 1
}
```

### Input
| Type | Content | 
| ------------- | --------  |
| Modality | Image(s) |
| JSON Metadata | Optional |

### Output
The above command produces sample JSON results as below:

```
            {     
                 "Objects": {
                    "Timing": {
                        "$": 0.209268808365
                    },
                    "Object": [
                        {
                            "Type": {
                                "$": "pedestrian"
                            },
                            "Location": {
                                "Score": {
                                    "$": 0.843919992447
                                },
                                "Left": {
                                    "$": 0
                                },
                                "Top": {
                                    "$": 0
                                },
                                "Right": {
                                    "$": 525
                                },
                                "Bottom": {
                                    "$": 791
                                }
                            }
                        },
                        {
                            "Type": {
                                "$": "face"
                            },
                            "Location": {
                                "Score": {
                                    "$": 0.999044835567
                                },
                                "Left": {
                                    "$": 129
                                },
                                "Top": {
                                    "$": 147
                                },
                                "Right": {
                                    "$": 392
                                },
                                "Bottom": {
                                    "$": 456
                                }
                            }
                        },
                        {
                            "Type": {
                                "$": "head-shoulder"
                            },
                            "Location": {
                                "Score": {
                                    "$": 0.978021621704
                                },
                                "Left": {
                                    "$": 27
                                },
                                "Top": {
                                    "$": 0
                                },
                                "Right": {
                                    "$": 525
                                },
                                "Bottom": {
                                    "$": 613
                                }
                            }
                        },
                        {
                            "Type": {
                                "$": "torso"
                            },
                            "Location": {
                                "Score": {
                                    "$": 0.979073643684
                                },
                                "Left": {
                                    "$": 48
                                },
                                "Top": {
                                    "$": 387
                                },
                                "Right": {
                                    "$": 515
                                },
                                "Bottom": {
                                    "$": 791
                                }
                            }
                        }
                    ]
                }
            }
```

Each region is defined within a block of JSON as below where `Type` can be `face`, `pedestrian`, `head-shoulder` and `torso` and `Location` is defined by `Left`, `Top`, `Right`, and `Bottom`. The `Score` within `Location` block is the confidence value for the object type and its location. Please note that we are using Coordinate System where the origin is at `Top` and `Left` and `x` is increasing from `Left` to `Right` and `y` is increasing from `Top` to `Bottom`.
```
                        {
                            "Type": {
                                "$": "face"
                            },
                            "Location": {
                                "Score": {
                                    "$": 0.999044835567
                                },
                                "Left": {
                                    "$": 129
                                },
                                "Top": {
                                    "$": 147
                                },
                                "Right": {
                                    "$": 392
                                },
                                "Bottom": {
                                    "$": 456
                                }
                            }
                        }
```


## Publish and Share
You can push your model to IBM Docker Registry for sharing and deployment:

Step 1: Make sure you have login to the registry:
```
docker login --username <your-ibm-email> https://wvfs-docker-local.artifactory.swg-devops.com
```
Step 2: Publish and Share your Model:
```
docker push wvfs-docker-local.artifactory.swg-devops.com/ai-model-vision-example
```

If needed, you can tag your image with a version and then pubilsh it:
```
docker tag wvfs-docker-local.artifactory.swg-devops.com/ai-model-vision-example wvfs-docker-local.artifactory.swg-devops.com/ai-model-vision-example:v0.1
docker push wvfs-docker-local.artifactory.swg-devops.com/ai-model-vision-example:v0.1
```
where we label the default latest version `latest` to `v0.1` and then push it to IBM Docker Registry for Sharing and Deployment.

## References
1. https://github.ibm.com/qfan/Sparse-Complementary-Detector
