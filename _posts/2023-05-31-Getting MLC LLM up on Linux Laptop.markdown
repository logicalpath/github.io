---
layout: post
title: "Getting MLC LLM up on Linux Laptop"
---

Afer reading Simon Willison's [til](https://til.simonwillison.net/){:target="_blank"}, I decided to try to get the mlc-chat running in my old Dell laptop running Linux Mint 21.2 with an NVIDIA GTX 1050 GPU with 4 GB with these [instructions](https://mlc.ai/mlc-llm/#windows-linux-mac){:target="_blank"}

**TLDR** I got mlc-chat running on my laptop, but when answering a prompt, I ran out of memory. After the MLC dev team provided a [workaround](https://github.com/mlc-ai/mlc-llm/issues/263){:target="_blank"}, I was able to get the mlc-chat running and it wrote a few poems and answered questions about SQL. 

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



To run in Text Mode (without X Server):

> sudo systemctl stop lightdm

then typed `ctrl + alt + f4`

ran mlc_chat_cli 

and the MLC-LLM wrote me a nice poem about San Francisco.
