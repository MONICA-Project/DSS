# Monica Decision Support System 
<!-- Short description of the project. -->
**NB! The DSS is not Open Source, but there is a ready made [docker container](https://hub.docker.com/repository/docker/monicaproject/dss) that can be used for testing**

The Decision Support System (DSS) is characterised as the intelligence layer to the MONICA platform, which can solve problems and make interactive decisions by analysing massive streams of data in real time. DSS receives inputs from cameras, microphones and IoT sensors (such as wristbands, environmental sensors) in JSON form and depends on the input, it applies the appropriate intervention strategies and feeds COP with results/alerts.

![DSS](https://github.com/MONICA-Project/DSS/raw/master/operation.png) 

## Prerequisite
All the communication in and out from the DSS is using the MQTT protocol. Therefore to run the DSS the
* MQTT Broker (For instance [Eclipse Mosquitto](https://mosquitto.org/))


## Getting Started
Retrieve the docker image and start it
```bash
docker pull monicaproject/dss
docker run -d --name dss monicaproject/dss
```
The last commmand returns the container id, use this id to attach to the container.
```bash
docker exec -it id-returned-from-last-command  /bin/bash
```
Then install your favorite editor, in my case, nano:
```bash
apt-get update
apt-get install nano
```
### Configuring the MQTT Broker
The user has to adjust the parameters of the mqtt server on which the DSS will connect to to start receiving and sending data. For achieving that, he has to reach the input folder by typing “cd input”, to open the mqttServerParameters json file by typing nano mqttServerParameters. json” and to declare the ip and port of the mqtt server to the fields “mqtt_hostname” (line 2) and “mqtt_port” (line 3) .

![mqtt](https://github.com/MONICA-Project/DSS/raw/master/mqtt.png) 

## The DSS json Configurations files
The user has also to determine the name of the topics, from which DSS will receive and send data. Depends on the functionality there is the appropriate json file in the input folder. He has to open it and declare the input and output datastreams. In addition, he has to adjust the limits if it is needed.  
### fightdetection.json
It includes the datastreams come from the fight detection module. The user has to declare in the fields “Topic” and “Output” the names of input and output datastreams relatively. In addition, in the field “Limit”, he has to determine the limit of fight detection confidence. If this limit has been exceeded, DSS will start sending alert messages.   
![fight detectinm](https://github.com/MONICA-Project/DSS/raw/master/fight.png) 

### flowanalysis.json
It includes the datastreams come from flow detection module. The user has to declare in the fields “Topic” and “Output” the names of input and output datastreams relatively.
![flow analysis](https://github.com/MONICA-Project/DSS/raw/master/flow,png.png) 

### gatecounting.json
It includes the datastreams come from gate counting module. The user has to declare in the fields “Topic” and “Output” the names of input and output datastreams relatively.
![gate counting](https://github.com/MONICA-Project/DSS/raw/master/gate.png) 

### cameras.json
It includes the datastreams come from cameras. The user has to declare in the fields “Topic” and “Output” the names of input and output datastreams relatively. The user has also to declare the limits of the number of people per square meter before DSS starts sending warning, alert, high alert, so he has to adjust the fields “First_Limit”, "Second_Limit" and "Third_Limit" respectively.
![cameras](https://github.com/MONICA-Project/DSS/raw/master/cameras.png) 

### objdetection.json
It includes the datastreams come from object detection module. The user has to declare in the fields “Topic” and “Output” the names of input and output datastreams relatively.
![Object detection](https://github.com/MONICA-Project/DSS/raw/master/objectdetection.png) 

### microphones.json
It includes the datastreams come from SLMs. The user has to declare in the fields “Topic” and “Output” the names of input and output datastreams relatively. The user should also, declare to the field “Alert” → “Output” the name of the datastream on which DSS will start sending alert messages. Finally, he ought to determine in the field “Type” the type of the SLM and in the case that there are more than one SLMs, the name and the transfer function of the other in the field “Transfer function”.
![microphones](https://github.com/MONICA-Project/DSS/raw/master/microphones.png) 

### Adding datastreams to existing json files
In the case that the user needs to add new input datastreams, it can easily be done by adding a same structure of the json format, declaring the new input datastream and separating the two structures by a comma.
![multiple](https://github.com/MONICA-Project/DSS/raw/master/objectdet2.png) 

## Development
Edit the configuration files, detach from the console and restart the docker container. The DSS will start with the new configuration.

To save the configuration create an image of the docker container that you edited:
```bash
docker container commit dss mydss:latest
```
This can now be pushed to for instance docker hub.

## Functionality
Depending on the input of the DSS, it can execute many functionalities.
### Crowd Heatmaps
If it receives the crowd heat map from cameras, it can export alert messages in the case that the number of people per square meter exceeds a limit.
![heatmaps](https://github.com/MONICA-Project/DSS/raw/master/functionality1.png) 
### Soundlevel meters
If it receives the sound level data from SLMs (types LAeq, LCeq), it can export alert messages in the case that the volume of the sound exceeds a limit. Furthermore, it can export the moving average of the sound per 15 minutes.
![SLM](https://github.com/MONICA-Project/DSS/raw/master/func2.png) 
### Fight detection 
If it receives data from the fight detection module, it can export alert messages in the case of the confidence exceeds a limit.
![fight](https://github.com/MONICA-Project/DSS/raw/master/func3.png)
### Flow detection 
If it receives data from the flow detection module, it can export alert messages in the case that the direction is the opposite of that it should be.
![flow](https://github.com/MONICA-Project/DSS/raw/master/func4.png)

### Gate counting
If it receives data from the gate counting module it can export the number of entries and exits.
![flow](https://github.com/MONICA-Project/DSS/raw/master/func5.png)

### Object detection 
If it receives data from the object detection module it can export alert messages in the case of unwanted detection.
![Object Detection](https://github.com/MONICA-Project/DSS/raw/master/func6.png)

## Usage
DSS is highly adaptable and configurable, using JSON configuration files. The code base is written in a generic, pilot-agnostic way and has been tested in a lot of pilots, some of them are Fete des Lumieres, Woodstower et c.

## Affiliation
![MONICA](https://github.com/MONICA-Project/template/raw/master/monica.png)  
This work is supported by the European Commission through the [MONICA H2020 PROJECT](https://www.monica-project.eu) under grant agreement No 732350.
