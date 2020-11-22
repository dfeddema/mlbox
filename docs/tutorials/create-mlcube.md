# Tutorial Create an MLCube 
<div style="text-align:center"><span style="color:black; font-family:Georgia; font-size:2em;">Interested in getting started with MLCube? Follow the instructions in this tutorial, or watch the video below.</span></div>
[![Watch the video](http://img.youtube.com/vi/hQRBLW6giRc/0.jpg)](https://www.youtube.com/embed/hQRBLW6giRc)

## Step 1: CREATE A DOCKER IMAGE FOR YOUR MODEL 
``` 
FROM python:3.6
MAINTAINER MLPerf MLCube Working Group

# Remove all stopped containers: docker rm $(docker ps -a -q)
# Remove containers like none:none: docker rmi $(docker images | grep none | awk '{print $3}')

COPY requirements.txt /requirements.txt
RUN pip3 install --no-cache-dir -r /requirements.txt

COPY mnist.py /workspace/mnist.py
ENTRYPOINT ["python3", "/workspace/mnist.py"]
```
## Step 2: PUSH YOUR DOCKER IMAGE TO A PUBLIC REPO 
```
# After you have created a repository on https://hub.docker.com or another public repository 
docker push yourusername/yourimagename 
```
## Step 3: GET MLCUBE TEMPLATES
```  
# Clone MLCube Templates
git clone https://github.com/sergey-serebryakov/mlbox/tree/feature/mlbox-template/examples/template 
```
## Step 3: USE TEMPLATE FILES TO SET UP MLCUBE
```
cd ./platforms
# Edit the docker.yaml file specifiing the platform name you want (docker or kubernetes or ssh)
```  
```
# Platform configuration files define where and how runners run MLBoxes. This configuration file defines a Docker
# runtime for MLBoxes. One field need to be updated here - `container.image`. This platform file defines local docker
# execution environment.
# MLCommons-Box Docker runner uses image name to either `pull` or `build` a docker image. The rule is the following:
#   - If the following file exists (`build/Dockerfile`), Docker image will be built.
#   - Else, docker runner will pull a docker image with the specified name.
# Users provide platform files using `--platform` command line argument.
schema_type: mlcommons_box_platform
schema_version: 0.1.0

platform:
  name: "docker"
  version: ">=18.01"
container:
  # image: "mlperf/mlbox_mnist:0.0.2"
```
## Step 4: TEST YOUR MLCUBE
