## NOTES 
Forked from https://github.com/openai/gpt-2

This project is planned to be based 
upon https://www.linuxjournal.com/content/ai-wizard-words

## Work in Progress - 01-25-2020 ##

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

docker run -it --runtime=nvidia openai/gpt-2-gpu:$(date +%F) /bin/bash

root@[container_id]:/gpt-2# nvidia-smi

```

