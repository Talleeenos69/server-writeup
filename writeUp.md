# Installing Large Language Models

## Purpose

The purpose of this assignment is to show that Tallen Pelissero and Joel Kamminga are capable of deploying Large Language Models (LLMs) on a virtual machine with GPU acceleration.

## Personal Choices

#### Operating system :

For the host server operating system, we chose Fedora 39 Server edition. This was due to the stable, yet up-to-date packages that the system provides, as well as [Cockpit](https://cockpit-project.org/) (A web interface to monitor and control various parts of the system) coming installed by default. We chose Fedora over a system like [Debian](https://www.debian.org) because it is much more up-to-date and therefore performant. Alternatively, we chose to steer away from a system like [Arch Linux](https://archlinux.org/) because the lack of support distros like this have and the packages tend to be unstable.

#### Disk Maneagement :

We chose RAID5 as our data storage virtualization technology. This was due to the fact that it allows for one HDD to fail at any time, without any data being lost. We chose RAID5 despite its 1 drive overhead because we wanted to make sure our system was redundant and would not fail if one of our drives was removed.

#### Hypervisor :

The Hypervisor that we chose was [KVM](https://www.kernel.org/doc/html/latest/virt/kvm/index.html), a standard virtualization and hypervisor for Linux and Unix-based systems. The installation was rather simple due to cockpit providing an installer via the GUI.

#### GPU :

To accelerate the LLMs on our server, we installed an Nvidia GTX 1070 (as that was the best one readily available to us). The server's chassis was too small to fit the GPU internally, so instead, we simply ran a PCIe cable extender outside of the server chassis.

#### Virtual Machine :

Originally, the virtual machine was going to be a Debian system. However, many issues facing the Nvidia CUDA drivers forced us to switch to a more up-to-date system, that being Fedora 37. The choice of Fedora and its specific version due to the familiarity of the system as well as the version number being explicitly stated on the Cuda driver installation guide.

#### LLM Deployment Software :

To run the language models, we used a program called [Ollama](https://ollama.com/) a program that acts similarly to Docker, but for LLMs.

#### Language Models :

The language models that we chose are as follows : 

- [Microsoft's Phi model (2.7B)](https://ollama.com/library/phi)
- [Llava (7B)](https://ollama.com/library/llava)
- [Mistral (7B)](https://ollama.com/library/mistral)
- [Orca-mini (3B)](https://ollama.com/library/orca-mini)
- [Orca2 (7B)](https://ollama.com/library/orca2)
- [Deepseek-coder (6.7B)](https://ollama.com/library/deepseek-coder)

We chose these models because of the variety of strengths that they offer, such as excellent reasoning, a high degree of language understanding, and proficient skills in programming.

#### Frontend :

The web UI that we chose is [OpenWebUI](https://github.com/open-webui/open-webui), an open-source project that can be used to interface with Ollama. The reason we chose this UI is because it is robust and secure, allowing for different user accounts with saved chats and allows for management of Language models without using the command line. We chose not to port forward the interface due to security concerns. However, any user connected to the school's local network can access the URL at any time.

## How we set it up

#### Specs :

Before installing the operating system to the server, we needed to identify the specifications. To do this, we needed to connect a display and keyboard to interface with the server. Next, we booted our server into the BIOS by repeatedly pressing the 'f10' key. After a few memory checks, the server booted into the about section of the server's BIOS. From there, we identified two 8-core, 16 thread [Intel Xeon E5-2450L](https://ark.intel.com/content/www/us/en/ark/products/64610/intel-xeon-processor-e5-2450l-20m-cache-1-80-ghz-8-00-gt-s-intel-`q`pi.html)s clocked at 2.8GHz with a boost clock of 2.3GHz, 96Gb of RAM, four 2Tb hard drives, etc.

#### Disk Setup :

Now that we knew our specifications, we configured our hard drives in RAID 5. We did this by entering the systems disk configuration tool in the BIOS. We opted to format the disks while setting them up, which is not necessary.

#### OS Installation :

Once our disks were configured, we installed Fedora server 39. (The latest version as of the time of writing) This was quite simple via the graphical installer, [Anaconda](https://docs.fedoraproject.org/en-US/fedora/f36/install-guide/install/Installing_Using_Anaconda/). We opted out of creating a root account, and instead added the newly created user to the 'Wheel' group, which gives us administrator privileges to the server.

#### Software Setup :

After the system was installed, we needed to install the required packages to get out virtual machine running.

Convieniently, the Cockpit interface allows for the installation of common server packages. One of these packages is the "Libvirt" package which allows us to create and run virtual machines. This also gives us a nice UI within Cockpit for managing and running VMs.

Once the libvirt package was installed, we needed to enable the service. This can be done via the GUI or by running a simple command:

```
# systemctl enable libvirtd --now
```
This command enables and starts the service, and this also tells [systemd](https://systemd.io/) to start the service every time the system boots.

#### Virtual Machine Setup and Issues :

Now that the virtualization deamon was running, we needed to deploy a virtual machine.

From the Cockpit "*Virtual Machines*" tab, we selected "Create VM" and configured it as follows:

```
Name : Ollama
Connection : System
Installation Type : Download an OS
Operating System : Debian 12 (bookworm)
Storage : Create new qcow2 volume
Storage Limit : 120 GiB
Memory : 40.0 GiB
```
Then we selected "*Create and edit*" which started the download for the iso file and prompted us to the advanced configuration menu. From there, we edited the vCPU configuration.

```
vCPU maximum : 8
vCPU count : 8
Sockets : 2
Cores per socket : 2
Threads per core : 2
Mode : host-passthrough
```
Outside of the CPU configuration menu, we selected "*Autostart*" which starts the virtual machine at boot.

Under the *Network Interfaces* tab, we changes the settings as follows;
```
Interface type : Direct Attachment
Source : eno1 (your primary network connection)
Model : e1000e (PCI)
```
This allows the virtual machine to be assigned an IP address on the LAN so we can secure shell into it and access the future web interface.

