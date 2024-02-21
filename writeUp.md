# Installing Large Language Models

## Purpose

The purpose of this assignment is to show that Tallen Pelissero and Joel Kamminga are capable of deploying Large Language Models (LLMs) on a virtual machine with GPU acceleration.

## Personal Choices

For the host operating system, we chose Fedora 39 Server edition. This was due to the stable, yet up-to-date packages that the system provides, as well as cockpit (A web interface to monitor and control various parts of the system) comes installed by default.

The Hypervisor that we chose was Qemu, a standard virtualization and hypervisor software for linux and unix-based systems. The installation was rather simple due to cockpit providing an installer via the GUI.

Originally, the virtual machine was going to be a debian install. However, many issues facing the Nvidia Cuda drivers forced us to switch to a more up-to-date system, that being Fedora 37. The choice of Fedora and its specific version due to the familiarty of the system as well as the version number being explicitly stated on the Cuda driver installation guide.

- Running a Fedora virtual machine
- Supports several LLMs
    - Phi
    - Mixtral
    - Orca
    - Etc.
- Able to be interfaced through WebUI