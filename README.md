# machine_learning

machine learning source code and datasets

## setup Ubuntu 24.04 for Machine Learning with GPU support

Computer: i7-12700K + 32GB + 1TB SSD + Nvidia RTX 5060 Ti 16GB + Nvidia RTX 4060Ti 16GB
OS: Ubuntu 24.04 LTS

* Step1: Install Nvidia driver: nvidia-driver-570-open  (as of 5/24/2025, this is the only version works); run command nvidia-smi to verify

* Step2: Install docker following https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

* Step3: Install the NVIDIA Container Toolkit https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#with-apt-ubuntu-debian and config it https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#configuring-docker

now, the machine learning environment with GPU support has setup, we can run docker container like ollama, tensorflow or PyTorch

## run PyTorch docker container with GPU support

Step1: Find the PyTorch tag match the Cuda version from the driver
       run nvidai-smi to check the driver's cuda version
       find the tag match the version from https://hub.docker.com/r/pytorch/pytorch/tags 

Step2: start the docker container with --gpus=all option

       sudo docker run -it --gpus=all --rm pytorch/pytorch:2.7.0-cuda12.8-cudnn9-runtime bash

Step3: verify PyTorch is working with GPU support
       in the commandline opened in step2, run below command


       echo -e 'import torch \nprint("GPU count:", torch.cuda.device_count())'|python3

  
  
  expect output like: "GPU count: 2"

## run chatbot with ollama + open WebUI 

* Step1: Start ollama + webui bundle container following https://github.com/open-webui/open-webui?tab=readme-ov-file#installing-open-webui-with-bundled-ollama-support

* Step2: open https://ollama.com/search to find models, open webui http://localhost:3000 and pull the model from ollama, start chat, while chatbot is running, use nvidia-smi to monitor GPU load.
