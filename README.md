## NOTES 
Forked from https://github.com/openai/gpt-2 on 01-24-2020. Most of the code here, aside my changes and fixes, are from the original repository.

This project is based 
upon https://www.linuxjournal.com/content/ai-wizard-words

The following steps were taken from an Ubuntu 19.10 host with a GeForce GTX 1080 GPU while experimenting at home.

This is a work in progress - 01-25-2020

## Install the Nvidia Container Runtime
https://nvidia.github.io/nvidia-container-runtime/
```
curl -s -L https://nvidia.github.io/nvidia-container-runtime/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-container-runtime/$distribution/nvidia-container-runtime.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-container-runtime.list
sudo apt-get update
sudo apt install nvidia-container-runtime

sudo vi /etc/docker/daemon.json
{
    "runtimes": {
        "nvidia": {
            "path": "/usr/bin/nvidia-container-runtime",
            "runtimeArgs": []
        }
    }
}

sudo systemctl restart docker

```
## Build and Run Docker Image
```
cd [repo_clone_directory]
docker build -t openai/gpt-2-gpu:$(date +%F) -f Dockerfile.gpu .

docker images

docker run -it --rm --runtime=nvidia openai/gpt-2-gpu:$(date +%F) /bin/bash

root@[container_id]:/gpt-2# nvidia-smi
Sun Jan 26 17:42:46 2020       
+-----------------------------------------------------------------------------+
| NVIDIA-SMI 430.50       Driver Version: 430.50       CUDA Version: 10.1     |
|-------------------------------+----------------------+----------------------+
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap|         Memory-Usage | GPU-Util  Compute M. |
|===============================+======================+======================|
|   0  GeForce GTX 1080    Off  | 00000000:01:00.0  On |                  N/A |
| 38%   34C    P0    44W / 180W |   1042MiB /  8117MiB |      0%      Default |
+-------------------------------+----------------------+----------------------+
                                                                               
+-----------------------------------------------------------------------------+
| Processes:                                                       GPU Memory |
|  GPU       PID   Type   Process name                             Usage      |
|=============================================================================|
+-----------------------------------------------------------------------------+


root@[container_id]:/gpt-2# src/interactive_conditional_samples.py 

Model prompt >>> [insert any text to begin text predicting]

```

## Using the CPU, instead of GPU, Docker Container Image

```
docker build -t openai/gpt-2-cpu:$(date +%F) -f Dockerfile.cpu .

docker run -it --rm openai/gpt-2-cpu:$(date +%F) /bin/bash

```


