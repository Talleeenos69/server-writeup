# Installing Large Language Models

## Purpose

The purpose of this assignment is to show that Tallen Pelissero and Joel Kamminga are capable of deploying Large Language Models (LLMs) on a virtual machine with GPU acceleration.

## Personal Choices

For the host operating system, we chose Fedora 39 Server edition. This was due to the stable, yet up-to-date packages that the system provides, as well as cockpit (A web interface to monitor and control various parts of the system) coming installed by default.

We chose RAID5 as our data storage virtualization technology. This was due to the fact that it allows for one HDD to fail at any time, with no data being lost. The downside to this is that RAID5 requires 1 drive of overhead, which was 2Tb in our case.

The Hypervisor that we chose was QEMU, a standard virtualization and hypervisor software for Linux and Unix-based systems. The installation was rather simple due to cockpit providing an installer via the GUI.

Originally, the virtual machine was going to be a debian system. However, many issues facing the Nvidia CUDA drivers forced us to switch to a more up-to-date system, that being Fedora 37. The choice of Fedora and its specific version due to the familiarty of the system as well as the version number being explicitly stated on the Cuda driver installation guide.

To run the language models, we used a program called "Ollama" a program that acts similarly to Docker, but for LLMs. 

The language models that we chose are as follows :
- Microsoft's Phi model (2.7B)
- Llava (7B)
- Mistral (7B)
- Orca-mini (3B)
- Orca2 (7B)
- Deepseek-coder (6.7B)

We chose these models because of the variety of strengths that they have, such as excellent reasoning, good language understanting, and high level of programmng knowledge.

The webUI that we chose is OpenWebUI, an open-source project that can be used to interface with Ollama. THe reason we chose this UI is because it is rebust and secure, allowing for different user accounts with saved chats and allows for maneagement of Language models without using the command line. The webUI can be accessed easily at any point by typing or clicking the IP while connected to the school network.

## How we set it up

Before installing the operating system to the server, we needed to identify the specifications. In order to do this, we booted our server into the BIOS. After a few memory checks, the server booted into the about section of the server's BIOS. From there, we identified two 16-core Intel Xeon E5-2450Ls clocked at 2.3GHz, 96Gb of RAM, four 2Tb hard drives, and much more information.

To hardware accelerate the AI on the server, we installed an Nvidia GTX 1070 (as that was the best one readily available to us). The server box was too small to fit the GPU internally, so instead, we simply ran the PCIe cable outside of the box and let the GPU sit on the outside.

Now that we knew our specifications, we set up our disks to run in RAID5