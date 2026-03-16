# machine_learning

machine learning source code and datasets

## setup Ubuntu 24.04 for Machine Learning with GPU support

Computer: i7-14700K + 64GB + 1TB SSD + Nvidia RTX 5060 Ti 16GB + Nvidia RTX 4060Ti 16GB
OS: Ubuntu 24.04 LTS

* Step1: Install Nvidia driver: nvidia-driver-580-open; run command nvidia-smi to verify

* Step2: Install docker following https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

* Step3: Install the NVIDIA Container Toolkit https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#with-apt-ubuntu-debian and config it https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#configuring-docker

now, the machine learning environment with GPU support has setup, we can run docker container like ollama, tensorflow or PyTorch

## run PyTorch docker container with GPU support

Step1: Find the PyTorch tag match the Cuda version from the driver
       run nvidai-smi to check the driver's cuda version
       find the tag match the version from https://hub.docker.com/r/pytorch/pytorch/tags 

Step2: start the docker container with --gpus=all option

       sudo docker run -it --gpus=all --rm pytorch/pytorch:2.10.0-cuda13.0-cudnn9-runtime bash

Step3: verify PyTorch is working with GPU support
       in the commandline opened in step2, run below command


       echo -e 'import torch \nprint("GPU count:", torch.cuda.device_count())'|python3

  
  
  expect output like: "GPU count: 2"

## run chatbot with ollama + open WebUI 

* Step1: Start ollama, priotize RTX 5060 Ti 16GB
  
       sudo docker run -d --gpus all   -e CUDA_DEVICE_ORDER="PCI_BUS_ID"   -e CUDA_VISIBLE_DEVICES="0,1"   -e OLLAMA_SCHED_SPREAD=0   -v ollama:/root/.ollama   -p 11434:11434   --name ollama   --restart always   ollama/ollama:latest

* Step2: install Open WebUI, ref https://github.com/open-webui/open-webui?tab=readme-ov-file#installation-with-default-configuration

       sudo docker run -d -p 3000:8080   -e OLLAMA_BASE_URL=http://host.docker.internal:11434   --add-host=host.docker.internal:host-gateway   -v open-webui:/app/backend/data   --name open-webui   --restart always   ghcr.io/open-webui/open-webui:main

* Step3: open https://ollama.com/search to find models, open webui http://localhost:3000 and pull the model from ollama, start chat, while chatbot is running, use nvidia-smi to monitor GPU load.

## run jupyterlab inside the docker container

1. when start docker container, add port forward
   
       sudo docker run -it -p 8888:8888 --gpus=all pytorch/pytorch:2.10.0-cuda13.0-cudnn9-runtime bash

2. install jupyterlab in the container
   
       pip3 install jupyterlab
       
3. set jupyterlab password so it can be accessed remotely 

       jupyter lab password

4. start jupyterlab binding 0.0.0.0

       jupyter lab --allow-root --ip 0.0.0.0

5. access from host machine

       http://localhost:8888/lab

or all in one:

```
#!/bin/bash

# Define the container name
NAME="pytorch-jupyter"

# Remove existing container if it exists
sudo docker rm -f $NAME 2>/dev/null

# Start the container
sudo docker run -d \
  --name $NAME \
  --gpus all \
  --restart always \
  -p 8080:8888 \
  -v pytorch:/root/.pytorch \
  -e CUDA_DEVICE_ORDER="PCI_BUS_ID" \
  -e CUDA_VISIBLE_DEVICES="0" \
  pytorch/pytorch:2.10.0-cuda13.0-cudnn9-runtime \
  /bin/bash -c "pip install jupyterlab --break-system-packages && jupyter lab --ip=0.0.0.0 --port=8888 --no-browser --allow-root --NotebookApp.token='' --NotebookApp.password=''"

echo "Wait 30 seconds for installation, then access at http://localhost:8080"
```

   
