# machine_learning

machine learning source code and datasets

## 5 simple steps to setup a LLM chatbot on local computer

Computer: i7-12700K + 32GB + 1TB SSD + Nvidia RTX 5060 Ti 16GB + Nvidia RTX 4060Ti 16GB
OS: Ubuntu 24.04 LTS

* Step1: Install Nvidia driver: nvidia-driver-570-open  (as of 5/24/2025, this is the only version works); run command nvidia-smi to verify

* Step2: Install docker following https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository

* Step3: Install the NVIDIA Container Toolkit https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#with-apt-ubuntu-debian and config it https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html#configuring-docker

until here, the machine learning environment with GPU support has setup, we can run ollama or other framework like tensorflow or PyTorch

* Step4: Start ollama + webui bundle container following https://github.com/open-webui/open-webui?tab=readme-ov-file#installing-open-webui-with-bundled-ollama-support

* Step5: open https://ollama.com/search to find models, open webui http://localhost:3000 and pull the model from ollama, start chat, while chatbot is running, use nvidia-smi to monitor GPU load.

to start a pytorch docker, find the tags match the Cuda version from the driver https://hub.docker.com/r/pytorch/pytorch/tags 
