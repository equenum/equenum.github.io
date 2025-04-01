---
title: Setting up Ollama on Ubuntu Server (Nvidia GPU Support)
date: 2025-03-22 13:00:00 +0400
categories: [homelab]
tags: [homelab, proxmox, server, hardware, nvidia, ollama, llm]
image:
  path: assets/img/posts/common/ollama-thumb.webp
  lqip: assets/img/posts/common/common-thumb-lqip.webp
---

Welcome to this quick guide on how to set up `Ollama` with `Nvidia` GPU Support on a `Ubuntu Server 24.04` virtual machine running in `Proxmox`. This guide assumes that a virtual machine was freshly created, meaning that for existing ones, some of the steps might be optional.

## Prerequisites

- Make sure a virtual machine has already been created, as this step is not covered in this guide.
- Make sure GPU Passthrough is [configured in Proxmox]({% post_url 2025-03-09-proxmox-gpu-passthrough %}).

## Guide

#### 1. Install Qemu guest agent (optional)

Proxmox [Wiki-page](https://pve.proxmox.com/wiki/Qemu-guest-agent).

```shell
# install
apt update
apt install qemu-guest-agent

# enable and start
systemctl enable qemu-guest-agent
systemctl start qemu-guest-agent

# verify
systemctl status qemu-guest-agent
```

#### 2. Install Nvidia drivers

Official [Nvidia Cuda Toolkit Download page](https://developer.nvidia.com/cuda-downloads?target_os=Linux&target_arch=x86_64&Distribution=Ubuntu&target_version=24.04&target_type=deb_network) for Ubuntu Server 24.04, (x86_64, network installer type).

Install the drivers (proprietary or open kernel module flavor):

```shell
# proprietary
sudo apt-get install -y cuda-drivers

# open
sudo apt-get install -y nvidia-open
```

Install CUDA Toolkit:

```shell
# before executing, check if a more recent version is available
# get the package
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2404/x86_64/cuda-keyring_1.1-1_all.deb

# install
sudo dpkg -i cuda-keyring_1.1-1_all.deb
sudo apt-get update
sudo apt-get -y install cuda-toolkit-12-8
```

Verify the install:

```shell
# should display GPU details
nvidia-smi 
```

#### 3. Install Ollama

Official Ollama [install guide](https://github.com/ollama/ollama/blob/main/docs/linux.md). Choose manual or automatic install. Make sure to review [the script](https://ollama.com/install.sh), if the second one is chosen.

```shell
# automatic install
curl -fsSL https://ollama.com/install.sh | sh
```

#### 4. Set OLLAMA_HOST

To make sure Ollama is available on the network from machines other than the server, we have to enable it to listen on all interfaces and IPs. To do so, we have to update Ollama by modifying one of the Ollama environment configurations:

```shell
# open Ollama service config file
nano /etc/systemd/system/ollama.service

# under 'Service' section set 'Environment' to the following
Environment="OLLAMA_HOST=0.0.0.0"

# optionally, the port can be set to one other than the default (11434)
Environment="OLLAMA_HOST=0.0.0.0:8080"
```

#### 5. Serve Ollama

```shell
# start Ollama
ollama serve
```

Sometimes, starting Ollama for the first time results in the following error:

```
Error: listen tcp 127:0.0.1:11434: bind: address already in use
```

Although, I did not get to the exact reason behind the error, a simple restart seems to work:

```shell
# stop the service
systemctl stop ollama

# start back up
ollama serve
```

To verify Ollama was able to properly identify the GPU, search for the following log message in the console output:

```
level=INFO msg="inference compute" id=GPU-<your-guid> library=cuda variant=v12 compute=8.6 driver=12.8 name="NVIDIA GeForce RTX 3060" total="11.6 GiB" available="11.5 GiB"
```

#### 6. Configure firewall (optional)

```shell
# allow incoming traffic for Ollama
sudo ufw allow 11434

# optionally, allow ssh connections for easy server management
sudo ufw allow ssh

# make sure the firewall is active
# check status 
sudo ufw status

# if returns 'Status: inactive', run 
sudo ufw enable
```

#### 7. Verify Ollama is running correctly

In a browser from any computer on the network other the server itself, navigate to http://{server-ip}:11434, or run the following command:

```shell
curl -X GET http://{server-ip}:11434
```

In any case, you should be able to se the following output: `Ollama is running`. From this point, Ollama should be fully functional and running. To start talking to LLMs, ssh into the server, and run:

```shell
# pull the model weights
ollama pull llama3.2

# run the model
ollama run llama3.2

# stop the model
ollama stop llama3.2
```

Official Ollama [Quickstart guide](https://github.com/ollama/ollama?tab=readme-ov-file#quickstart).

#### 8. Deploy Open Web UI (optional)

For a more convenient, Chat-GPT like experience, consider deploying a chat UI app, like [`Open Web UI`](https://github.com/open-webui/open-webui):

```shell
# make sure to replace your server ip and port in 'OLLAMA_BASE_URL'
docker run -d -p 3000:8080 -e OLLAMA_BASE_URL=http://127.0.0.1:11434/ -v open-webui:/app/backend/data --name open-webui --restart always ghcr.io/open-webui/open-webui:main
```

## Useful links

- <https://pve.proxmox.com/wiki/Qemu-guest-agent>
- <https://github.com/ollama/ollama/blob/main/docs/linux.md>
- <https://developer.nvidia.com/cuda-downloads>
- <https://github.com/ollama/ollama?tab=readme-ov-file#quickstart>
- <https://github.com/open-webui/open-webui>