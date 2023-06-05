---
layout: post
title: "Getting MLC LLM up on Linux Laptop"
---

Afer reading Simon Willison's [til](https://til.simonwillison.net/){:target="_blank"}, I decided to try to get the mlc-chat running in my old Dell laptop running Linux Mint 21.2 with an NVIDIA GTX 1050 GPU with 4 GB with these [instructions](https://mlc.ai/mlc-llm/#windows-linux-mac){:target="_blank"}

**TLDR** I got mlc-chat running on my laptop, but when answering a prompt, I ran out of memory. After the MLC dev team provided a [workaround](https://github.com/mlc-ai/mlc-llm/issues/263){:target="_blank"}, I was able to get the mlc-chat running and it wrote a few poems and answered questions about SQL. The workaround involvved running mlx-chat with Server disabled.

### Memory
View memory 
>  nvidia-smi
       
| NVIDIA-SMI 525.105.17   Driver Version: 525.105.17   CUDA Version: 12.0     |

|-------------------------------|----------------------|-----------------------
| GPU  Name        Persistence-M| Bus-Id        Disp.A | Volatile Uncorr. ECC |
| Fan  Temp  Perf  Pwr:Usage/Cap| Memory-Usage         | GPU-Util  Compute M. |
|                               |                      |               MIG M. |
|-------------------------------|----------------------|----------------------|
|   0  NVIDIA GeForce ...  Off  | 00000000:01:00.0 Off |                  N/A |
| N/A   51C    P0    N/A /  N/A |    607MiB /  4096MiB |      0%      Default |
|                               |                      |                  N/A |
                                                                           
# Disable X Server

To run in Text Mode (without X Server):

> sudo systemctl stop lightdm

To get into text mode, type `ctrl + alt + f4`

ran mlc_chat_cli 

To exit text mode, type `ctrl + alt + f7`

Note that the f4/f7 may have different numbers depending on your system

### Drivers

Getting the correct driver's installed for the NVIDIA GPU took some doing. The folks at MLC reccomended installing the Vulkan driver. Here are the steps I took to get the driver loaded.

Intall Vulkan tools & drivers

> sudo apt install vulkan-tools

> sudo add-apt-repository ppa:graphics-drivers/ppa
> sudo apt update

Reboot

List drivers
> ubuntu-drivers devices

Install the driver
> sudo apt install nvidia-driver-525-server

Run the Vulkan tool

> vulkaninfo

GPU1:
VkPhysicalDeviceProperties:
.

.

.

deviceName  = **NVIDIA GeForce GTX 1050**

VkPhysicalDeviceLimits:
.

.

.

maxMemoryAllocationCount  = **4294967295**

