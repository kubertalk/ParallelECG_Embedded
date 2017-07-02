# Parallel computations of simulation of electrocardiogram on embedded TK1 SoC

Abstract: To transplant the paralleling program of computer simulation of electrocardiogram(ECG) from PC/Server based on x86 to TK1/TX1 based on ARM CPU-Nvidia GPU integrated architecture, I make this project.



1.System Construction

1.1 Hardware Environment

Jetson Tegra K1: ARM CPU-NVIDIAGPU Integrated Architecture

![TK1](/Users/kuber/Downloads/TK1.png)

1.1.1Prepare the system image in host device

A. Download the [JetsonTK1](http://developer.download.nvidia.com/embedded/L4T/r21_Release_v3.0/Tegra124_Linux_R21.3.0_armhf.tbz2) and [samplefile system](http://developer.download.nvidia.com/embedded/L4T/r21_Release_v3.0/Tegra_Linux_Sample-Root-Filesystem_R21.3.0_armhf.tbz2) these two hrefs from the [L4T](https://developer.nvidia.com/linux-tegra-r213)(Nvidia official link);

B. For example, the two files are:

```shell
Tegra124_Linux_R21.4.0armhf.tbz2

Tegra_Linux_Sample-Root-Filesystem_R21.4.0_armhf.tbz2
```

C. Uncompress the second file in the same folder.

```shell
$sudotar --numeric-owner -jxpf Tegra124_Linux_R21.4.0_armhf.tbz2
```

D. Watting for a folder named Linux_for_Tegra emerges, go in the related folder to uncompress the another file:

```shell
$cd Linux_for_Tegra/rootfs
$sudo tar --numeric-owner -jxpf ../../Tegra_Linux_Sample-Root-Filesystem_R21.4.0_armhf.tbz2
```

E. Return to upper folder and excute the installation script for apply binaries.

```shell
$cd ..
$sudo ./apply_binaries.sh
```

1.1.2 Transfer system from host to device

A. Power down the device; connect the Micro-B plug on the USB cable to the Recovery Port on the device and the other end to an available USB port on the host PC.

B. Connect to the power adapter to the device; Press and release the POWER button.

C.When you see a 16G eMMC device is mounted on the host.

```shell
$sudo ./flash.sh -S 8GiB jetson-tk1 mmcblk0p1
```

D. Wait a moment and restart. Connect the HDMI cable, USB hub, and enternal cable. The hardware setup is done.



1.2 Software Environment

CUDA6.5 + OpenMP

1.2.1 Install cuda6.5

A.Install repository meta-data

```shell
$ sudo dpkg -i cuda-repo-l4t-r21.3-6-5-prod_6.5-42_armhf
```

B.Update the Apt repository cache 

```shell
$ sudo apt-get update
```

C.Install CUDA Toolkit

```shell
$ sudo apt-get install cuda-toolkit-6-5 
```

D.Add the user to the video group

```shell
$ sudo usermod -a -G video
```

E. Make the samples and choose one to excute for testing the cuda

```shell
$cd usr/local/cuda/
$./cuda-install-samples-6.5.sh /home/ubuntu/
$cd /home/NVIDIA-6.5_Samples & make
$cd /home/ubuntu/NVIDIA-6.5_Samples/bin/armvl7/linux/release/gnueabinf
```

F. CUDA6.5's installation is done.

1.2.2 GCC+OpenMP

​	Because the gcc 4.6 implements version 3.0 of the OpenMP standart. We just need to enable OpenMP by using: gcc -fopenmp foo.c after installing gcc.

1.3 Whole Program

![whole program](/Users/kuber/Downloads/whole program.png)

1.3.1 Inputs:

![Inputs](/Users/kuber/Downloads/Inputs.png)

The files of the left screen are the inputs which are wrote by some domain special language in eletrophysilogy.

The files of the right screen are some .cu file wrote by me in the coding course. in this stage, the .c and .cu are not to be divided. 

1.3.2 Compiling

```shell
$nvcc -g -Xcompiler -fopenmp (-lgomp) lps2.cu -o lps2
```

nvcc is the CUDA compiler driver that hides the intricate details of CUDA compilation from developers. With the aid of -Xcompiler, you can easily compiler C language and CUDA language. and with the help of -fopenmp, the compiler with know the openmp section in the whole code.

1.3.3 Outputs

![11111](/Users/kuber/Downloads/11111.png)

The outputs mainly are computing time of this ECG application and the number of dipole. According to the computing time of different level of optimization, i can draw a conclusion that the TK1 board can provide an easy available environment with good performance and high energy efficiency under some effective optimization strategies like cuda+OpenMP+Load prediction dynamic scheduling.



1.4 Others

A portable embedded cluster for parallel rendering using TK1 board:

![正面](/Users/kuber/Documents/2A你点不点/并行-CUDA-TK1/TK1/TK1集群实物照片/正面.jpg)