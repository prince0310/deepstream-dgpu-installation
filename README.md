# deepstream-dgpu-installation
# dGPU installation

## To install the DeepStream on dGPU (x86 platform), without docker, we need to do some steps to prepare the computer.


<details><summary>DeepStream 6.3</summary>
 

### 1. Disable Secure Boot in BIOS and Must install 

* Ubuntu 20.04
* GStreamer 1.16.3
* NVIDIA driver 525.125.06
* CUDA 12.1
* TensorRT 8.5.3.1

### 2. Install Dependencies
```
 sudo apt install \
libssl1.1 \
libgstreamer1.0-0 \
gstreamer1.0-tools \
gstreamer1.0-plugins-good \
gstreamer1.0-plugins-bad \
gstreamer1.0-plugins-ugly \
gstreamer1.0-libav \
libgstreamer-plugins-base1.0-dev \
libgstrtspserver-1.0-0 \
libjansson4 \
libyaml-cpp-dev \
libjsoncpp-dev \
protobuf-compiler \
gcc \
make \
git \
python3
```
### 2.1.Install CUDA Toolkit 12.1

```
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/ /"
sudo apt-get update
sudo apt-get install cuda-toolkit-12-1
```

### 3. Install NVIDIA driver 530.41.03, for professional and consumer GPUs (GeForce/ NVIDIA RTX/ QUADRO)

Download and install using NVIDIA driver 530.41.03 from NVIDIA Unix drivers page at: https://www.nvidia.com/download/driverResults.aspx/200481/en-us/

```
chmod 755 NVIDIA-Linux-x86_64-530.41.03.run
sudo ./NVIDIA-Linux-x86_64-530.41.03.run --no-cc-version-check
```

### 4. Install TensorRT 8.5.3.1
  ```
  sudo apt-get install libnvinfer8=8.5.3-1+cuda11.8 libnvinfer-plugin8=8.5.3-1+cuda11.8 libnvparsers8=8.5.3-1+cuda11.8 \
libnvonnxparsers8=8.5.3-1+cuda11.8 libnvinfer-bin=8.5.3-1+cuda11.8 libnvinfer-dev=8.5.3-1+cuda11.8 \
libnvinfer-plugin-dev=8.5.3-1+cuda11.8 libnvparsers-dev=8.5.3-1+cuda11.8 libnvonnxparsers-dev=8.5.3-1+cuda11.8 \
libnvinfer-samples=8.5.3-1+cuda11.8 libcudnn8=8.7.0.84-1+cuda11.8 libcudnn8-dev=8.7.0.84-1+cuda11.8 \
python3-libnvinfer=8.5.3-1+cuda11.8 python3-libnvinfer-dev=8.5.3-1+cuda11.8
  ```


### 5. Install librdkafka (to enable Kafka protocol adaptor for message broker)

```
git clone https://github.com/edenhill/librdkafka.git
cd librdkafka
git reset --hard 7101c2310341ab3f4675fc565f64f0967e135a6a
./configure
make
sudo make install
sudo mkdir -p /opt/nvidia/deepstream/deepstream-6.3/lib
sudo cp /usr/local/lib/librdkafka* /opt/nvidia/deepstream/deepstream-6.3/lib
```

### 6. Install the DeepStream SDK


Download the DeepStream 6.3 dGPU Debian package deepstream-6.3_6.3.0-1_amd64.deb : https://catalog.ngc.nvidia.com/orgs/nvidia/resources/deepstream

```
sudo apt-get install ./deepstream-6.3_6.3.0-1_amd64.deb
```


### 7.  To run the deepstream-app for test

```
cd /opt/nvidia/deepstream/deepstream-6.3/samples/configs/deepstream-app
deepstream-app -c source4_1080p_dec_infer-resnet_tracker_sgie_tiled_display_int8.txt
```




 
</details>

<details><summary>DeepStream 6.2</summary>

### 1. Disable Secure Boot in BIOS

### 2. Install Dependencies
```
sudo apt install \
libssl1.1 \
libgstreamer1.0-0 \
gstreamer1.0-tools \
gstreamer1.0-plugins-good \
gstreamer1.0-plugins-bad \
gstreamer1.0-plugins-ugly \
gstreamer1.0-libav \
libgstreamer-plugins-base1.0-dev \
libgstrtspserver-1.0-0 \
libjansson4 \
libyaml-cpp-dev \
libjsoncpp-dev \
protobuf-compiler \
gcc \
make \
git \
python3
```
### 2.1. Install dependencies

```
sudo apt-get update
sudo apt-get install gcc make git libtool autoconf autogen pkg-config cmake
sudo apt-get install python3 python3-dev python3-pip
sudo apt-get install dkms
sudo apt install libssl1.1 libgstreamer1.0-0 gstreamer1.0-tools gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav libgstreamer-plugins-base1.0-dev libgstrtspserver-1.0-0 libjansson4 libyaml-cpp-dev libjsoncpp-dev protobuf-compiler
sudo apt-get install linux-headers-$(uname -r)
```

**NOTE**: Purge all NVIDIA driver, CUDA, etc (replace $CUDA_PATH to your CUDA path)

```
sudo nvidia-uninstall
sudo $CUDA_PATH/bin/cuda-uninstaller
sudo apt-get remove --purge '*nvidia*'
sudo apt-get remove --purge '*cuda*'
sudo apt-get remove --purge '*cudnn*'
sudo apt-get remove --purge '*tensorrt*'
sudo apt autoremove --purge && sudo apt autoclean && sudo apt clean
```

### 3. Install CUDA Keyring

```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb
sudo apt-get update
```

### 4. Download and install NVIDIA Driver

<details><summary>TITAN, GeForce RTX / GTX series and RTX / Quadro series</summary><blockquote>

- Download

  ```
  wget https://us.download.nvidia.com/XFree86/Linux-x86_64/525.105.17/NVIDIA-Linux-x86_64-525.105.17.run
  ```

<blockquote><details><summary>Laptop</summary>

* Run

  ```
  sudo sh NVIDIA-Linux-x86_64-525.105.17.run --no-cc-version-check --silent --disable-nouveau --dkms --install-libglvnd
  ```

  **NOTE**: This step will disable the nouveau drivers.

* Reboot

  ```
  sudo reboot
  ```

* Install

  ```
  sudo sh NVIDIA-Linux-x86_64-525.105.17.run --no-cc-version-check --silent --disable-nouveau --dkms --install-libglvnd
  ```

**NOTE**: If you are using a laptop with NVIDIA Optimius, run

```
sudo apt-get install nvidia-prime
sudo prime-select nvidia
```

</details></blockquote>

<blockquote><details><summary>Desktop</summary>

* Run

  ```
  sudo sh NVIDIA-Linux-x86_64-525.105.17.run --no-cc-version-check --silent --disable-nouveau --dkms --install-libglvnd --run-nvidia-xconfig
  ```

  **NOTE**: This step will disable the nouveau drivers.

* Reboot

  ```
  sudo reboot
  ```

* Install

  ```
  sudo sh NVIDIA-Linux-x86_64-525.105.17.run --no-cc-version-check --silent --disable-nouveau --dkms --install-libglvnd --run-nvidia-xconfig
  ```

</details></blockquote>

</blockquote></details>

<details><summary>Data center / Tesla series</summary><blockquote>

  - Download

    ```
    wget https://us.download.nvidia.com/XFree86/Linux-x86_64/525.105.17/NVIDIA-Linux-x86_64-525.105.17.run
    ```

  * Run

    ```
    sudo sh NVIDIA-Linux-x86_64-525.105.17.run --no-cc-version-check --silent --disable-nouveau --dkms --install-libglvnd --run-nvidia-xconfig
    ```

</blockquote></details>

### 5. Download and install CUDA

```
wget https://developer.download.nvidia.com/compute/cuda/11.8.0/local_installers/cuda_11.8.0_520.61.05_linux.run
sudo sh cuda_11.8.0_520.61.05_linux.run --silent --toolkit
```

* Export environment variables

  ```
  echo $'export PATH=/usr/local/cuda-11.8/bin${PATH:+:${PATH}}\nexport LD_LIBRARY_PATH=/usr/local/cuda-11.8/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.bashrc && source ~/.bashrc
  ```

### 6. Install TensorRT

```
sudo apt-key adv --fetch-keys https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/3bf863cc.pub
sudo add-apt-repository "deb https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/ /"
sudo apt-get update
sudo apt-get install libnvinfer8=8.5.2-1+cuda11.8 libnvinfer-plugin8=8.5.2-1+cuda11.8 libnvparsers8=8.5.2-1+cuda11.8 libnvonnxparsers8=8.5.2-1+cuda11.8 libnvinfer-bin=8.5.2-1+cuda11.8 libnvinfer-dev=8.5.2-1+cuda11.8 libnvinfer-plugin-dev=8.5.2-1+cuda11.8 libnvparsers-dev=8.5.2-1+cuda11.8 libnvonnxparsers-dev=8.5.2-1+cuda11.8 libnvinfer-samples=8.5.2-1+cuda11.8 libcudnn8=8.7.0.84-1+cuda11.8 libcudnn8-dev=8.7.0.84-1+cuda11.8 python3-libnvinfer=8.5.2-1+cuda11.8 python3-libnvinfer-dev=8.5.2-1+cuda11.8
sudo apt-mark hold libnvinfer* libnvparsers* libnvonnxparsers* libcudnn8* python3-libnvinfer* tensorrt
```

### 7. Download from [NVIDIA website](https://developer.nvidia.com/deepstream-getting-started) and install the DeepStream SDK

DeepStream 6.2 for Servers and Workstations (.deb)

```
sudo apt-get install ./deepstream-6.2_6.2.0-1_amd64.deb
rm ${HOME}/.cache/gstreamer-1.0/registry.x86_64.bin
sudo ln -snf /usr/local/cuda-11.8 /usr/local/cuda
```

### 8. Reboot the computer

```
sudo reboot
```

### 9.  To run the deepstream-app for test

```
cd /opt/nvidia/deepstream/deepstream-6.2/samples/configs/deepstream-app
deepstream-app -c source4_1080p_dec_infer-resnet_tracker_sgie_tiled_display_int8.txt
```

</details>

<details><summary>DeepStream 6.1.1</summary>

### 1. Disable Secure Boot in BIOS


### 2. Install dependencies
```
$ sudo apt install \
libssl1.1 \
libgstreamer1.0-0 \
gstreamer1.0-tools \
gstreamer1.0-plugins-good \
gstreamer1.0-plugins-bad \
gstreamer1.0-plugins-ugly \
gstreamer1.0-libav \
libgstreamer-plugins-base1.0-dev \
libgstrtspserver-1.0-0 \
libjansson4 \
libyaml-cpp-dev \
gcc \
make \
git \
python3

```

### 2.1 Install dependencies

```
sudo apt-get update
sudo apt-get install gcc make git libtool autoconf autogen pkg-config cmake
sudo apt-get install python3 python3-dev python3-pip
sudo apt-get install dkms
sudo apt-get install libssl1.1 libgstreamer1.0-0 gstreamer1.0-tools gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav libgstreamer-plugins-base1.0-dev libgstrtspserver-1.0-0 libjansson4 libyaml-cpp-dev
sudo apt-get install linux-headers-$(uname -r)
```

**NOTE**: Purge all NVIDIA driver, CUDA, etc (replace $CUDA_PATH to your CUDA path)

```
sudo nvidia-uninstall
sudo $CUDA_PATH/bin/cuda-uninstaller
sudo apt-get remove --purge '*nvidia*'
sudo apt-get remove --purge '*cuda*'
sudo apt-get remove --purge '*cudnn*'
sudo apt-get remove --purge '*tensorrt*'
sudo apt autoremove --purge && sudo apt autoclean && sudo apt clean
```

### 3. Install CUDA Keyring

```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb
sudo apt-get update
```

### 4. Download and install NVIDIA Driver

<details><summary>TITAN, GeForce RTX / GTX series and RTX / Quadro series</summary><blockquote>

- Download

  ```
  wget https://us.download.nvidia.com/XFree86/Linux-x86_64/515.65.01/NVIDIA-Linux-x86_64-515.65.01.run
  ```

<blockquote><details><summary>Laptop</summary>

* Run

  ```
  sudo sh NVIDIA-Linux-x86_64-515.65.01.run --silent --disable-nouveau --dkms --install-libglvnd
  ```

  **NOTE**: This step will disable the nouveau drivers.

* Reboot

  ```
  sudo reboot
  ```

* Install

  ```
  sudo sh NVIDIA-Linux-x86_64-515.65.01.run --silent --disable-nouveau --dkms --install-libglvnd
  ```

**NOTE**: If you are using a laptop with NVIDIA Optimius, run

```
sudo apt-get install nvidia-prime
sudo prime-select nvidia
```

</details></blockquote>

<blockquote><details><summary>Desktop</summary>

* Run

  ```
  sudo sh NVIDIA-Linux-x86_64-515.65.01.run --silent --disable-nouveau --dkms --install-libglvnd --run-nvidia-xconfig
  ```

  **NOTE**: This step will disable the nouveau drivers.

* Reboot

  ```
  sudo reboot
  ```

* Install

  ```
  sudo sh NVIDIA-Linux-x86_64-515.65.01.run --silent --disable-nouveau --dkms --install-libglvnd --run-nvidia-xconfig
  ```

</details></blockquote>

</blockquote></details>

<details><summary>Data center / Tesla series</summary><blockquote>

  - Download

    ```
    wget https://us.download.nvidia.com/tesla/515.65.01/NVIDIA-Linux-x86_64-515.65.01.run
    ```

  * Run

    ```
    sudo sh NVIDIA-Linux-x86_64-515.65.01.run --silent --disable-nouveau --dkms --install-libglvnd --run-nvidia-xconfig
    ```

</blockquote></details>

### 5. Download and install CUDA

```
wget https://developer.download.nvidia.com/compute/cuda/11.7.1/local_installers/cuda_11.7.1_515.65.01_linux.run
sudo sh cuda_11.7.1_515.65.01_linux.run --silent --toolkit
```

* Export environment variables

  ```
  echo $'export PATH=/usr/local/cuda-11.7/bin${PATH:+:${PATH}}\nexport LD_LIBRARY_PATH=/usr/local/cuda-11.7/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.bashrc && source ~/.bashrc
  ```

### 6. Download from [NVIDIA website](https://developer.nvidia.com/nvidia-tensorrt-8x-download) and install the TensorRT

TensorRT 8.4 GA for Ubuntu 20.04 and CUDA 11.0, 11.1, 11.2, 11.3, 11.4, 11.5, 11.6 and 11.7 DEB local repo Package

```
sudo dpkg -i nv-tensorrt-repo-ubuntu2004-cuda11.6-trt8.4.1.5-ga-20220604_1-1_amd64.deb 
sudo apt-key add /var/nv-tensorrt-repo-ubuntu2004-cuda11.6-trt8.4.1.5-ga-20220604/9a60d8bf.pub
sudo apt-get update
sudo apt-get install libnvinfer8=8.4.1-1+cuda11.6 libnvinfer-plugin8=8.4.1-1+cuda11.6 libnvparsers8=8.4.1-1+cuda11.6 libnvonnxparsers8=8.4.1-1+cuda11.6 libnvinfer-bin=8.4.1-1+cuda11.6 libnvinfer-dev=8.4.1-1+cuda11.6 libnvinfer-plugin-dev=8.4.1-1+cuda11.6 libnvparsers-dev=8.4.1-1+cuda11.6 libnvonnxparsers-dev=8.4.1-1+cuda11.6 libnvinfer-samples=8.4.1-1+cuda11.6 libcudnn8=8.4.1.50-1+cuda11.6 libcudnn8-dev=8.4.1.50-1+cuda11.6 python3-libnvinfer=8.4.1-1+cuda11.6 python3-libnvinfer-dev=8.4.1-1+cuda11.6
sudo apt-mark hold libnvinfer* libnvparsers* libnvonnxparsers* libcudnn8* tensorrt
```

### 7. Download from [NVIDIA website](https://developer.nvidia.com/deepstream-getting-started) and install the DeepStream SDK

DeepStream 6.1.1 for Servers and Workstations (.deb)

```
sudo apt-get install ./deepstream-6.1_6.1.1-1_amd64.deb
rm ${HOME}/.cache/gstreamer-1.0/registry.x86_64.bin
sudo ln -snf /usr/local/cuda-11.7 /usr/local/cuda
```

### 8. Reboot the computer

```
sudo reboot
```


### 9.  To run the deepstream-app for test

```
cd /opt/nvidia/deepstream/deepstream-6.1.1/samples/configs/deepstream-app
deepstream-app -c source4_1080p_dec_infer-resnet_tracker_sgie_tiled_display_int8.txt
```

</details>

<details><summary>DeepStream 6.1</summary>

### 1. Disable Secure Boot in BIOS

### 2. Install dependencies
```
$ sudo apt install \
libssl1.1 \
libgstreamer1.0-0 \
gstreamer1.0-tools \
gstreamer1.0-plugins-good \
gstreamer1.0-plugins-bad \
gstreamer1.0-plugins-ugly \
gstreamer1.0-libav \
libgstrtspserver-1.0-0 \
libjansson4 \
libyaml-cpp-dev \
gcc \
make \
git \
python3

```
### 2.1 Install dependencies

```
sudo apt-get update
sudo apt-get install gcc make git libtool autoconf autogen pkg-config cmake
sudo apt-get install python3 python3-dev python3-pip
sudo apt-get install dkms
sudo apt-get install libssl1.1 libgstreamer1.0-0 gstreamer1.0-tools gstreamer1.0-plugins-good gstreamer1.0-plugins-bad gstreamer1.0-plugins-ugly gstreamer1.0-libav libgstrtspserver-1.0-0 libjansson4 libyaml-cpp-dev
sudo apt-get install linux-headers-$(uname -r)
```

**NOTE**: Purge all NVIDIA driver, CUDA, etc (replace $CUDA_PATH to your CUDA path)

```
sudo nvidia-uninstall
sudo $CUDA_PATH/bin/cuda-uninstaller
sudo apt-get remove --purge '*nvidia*'
sudo apt-get remove --purge '*cuda*'
sudo apt-get remove --purge '*cudnn*'
sudo apt-get remove --purge '*tensorrt*'
sudo apt autoremove --purge && sudo apt autoclean && sudo apt clean
```

### 3. Install CUDA Keyring

```
wget https://developer.download.nvidia.com/compute/cuda/repos/ubuntu2004/x86_64/cuda-keyring_1.0-1_all.deb
sudo dpkg -i cuda-keyring_1.0-1_all.deb
sudo apt-get update
```

### 4. Download and install NVIDIA Driver

<details><summary>TITAN, GeForce RTX / GTX series and RTX / Quadro series</summary><blockquote>

- Download

  ```
  wget https://us.download.nvidia.com/XFree86/Linux-x86_64/510.47.03/NVIDIA-Linux-x86_64-510.47.03.run
  ```

<blockquote><details><summary>Laptop</summary>

* Run

  ```
  sudo sh NVIDIA-Linux-x86_64-510.47.03.run --silent --disable-nouveau --dkms --install-libglvnd
  ```

  **NOTE**: This step will disable the nouveau drivers.

* Reboot

  ```
  sudo reboot
  ```

* Install

  ```
  sudo sh NVIDIA-Linux-x86_64-510.47.03.run --silent --disable-nouveau --dkms --install-libglvnd
  ```

**NOTE**: If you are using a laptop with NVIDIA Optimius, run

```
sudo apt-get install nvidia-prime
sudo prime-select nvidia
```

</details></blockquote>

<blockquote><details><summary>Desktop</summary>

* Run

  ```
  sudo sh NVIDIA-Linux-x86_64-510.47.03.run --silent --disable-nouveau --dkms --install-libglvnd --run-nvidia-xconfig
  ```

  **NOTE**: This step will disable the nouveau drivers.

* Reboot

  ```
  sudo reboot
  ```

* Install

  ```
  sudo sh NVIDIA-Linux-x86_64-510.47.03.run --silent --disable-nouveau --dkms --install-libglvnd --run-nvidia-xconfig
  ```

</details></blockquote>

</blockquote></details>

<details><summary>Data center / Tesla series</summary><blockquote>

  - Download

    ```
    wget https://us.download.nvidia.com/tesla/510.47.03/NVIDIA-Linux-x86_64-510.47.03.run
    ```

  * Run

    ```
    sudo sh NVIDIA-Linux-x86_64-510.47.03.run --silent --disable-nouveau --dkms --install-libglvnd --run-nvidia-xconfig
    ```

</blockquote></details>

### 5. Download and install CUDA

```
wget https://developer.download.nvidia.com/compute/cuda/11.6.1/local_installers/cuda_11.6.1_510.47.03_linux.run
sudo sh cuda_11.6.1_510.47.03_linux.run --silent --toolkit
```

* Export environment variables

  ```
  echo $'export PATH=/usr/local/cuda-11.6/bin${PATH:+:${PATH}}\nexport LD_LIBRARY_PATH=/usr/local/cuda-11.6/lib64${LD_LIBRARY_PATH:+:${LD_LIBRARY_PATH}}' >> ~/.bashrc && source ~/.bashrc
  ```

### 6. Download from [NVIDIA website](https://developer.nvidia.com/nvidia-tensorrt-8x-download) and install the TensorRT

TensorRT 8.2 GA Update 4 for Ubuntu 20.04 and CUDA 11.0, 11.1, 11.2, 11.3, 11.4 and 11.5 DEB local repo Package

```
sudo dpkg -i nv-tensorrt-repo-ubuntu2004-cuda11.4-trt8.2.5.1-ga-20220505_1-1_amd64.deb
sudo apt-key add /var/nv-tensorrt-repo-ubuntu2004-cuda11.4-trt8.2.5.1-ga-20220505/82307095.pub
sudo apt-get update
sudo apt-get install libnvinfer8=8.2.5-1+cuda11.4 libnvinfer-plugin8=8.2.5-1+cuda11.4 libnvparsers8=8.2.5-1+cuda11.4 libnvonnxparsers8=8.2.5-1+cuda11.4 libnvinfer-bin=8.2.5-1+cuda11.4 libnvinfer-dev=8.2.5-1+cuda11.4 libnvinfer-plugin-dev=8.2.5-1+cuda11.4 libnvparsers-dev=8.2.5-1+cuda11.4 libnvonnxparsers-dev=8.2.5-1+cuda11.4 libnvinfer-samples=8.2.5-1+cuda11.4 libnvinfer-doc=8.2.5-1+cuda11.4 libcudnn8-dev=8.4.0.27-1+cuda11.6 libcudnn8=8.4.0.27-1+cuda11.6
sudo apt-mark hold libnvinfer* libnvparsers* libnvonnxparsers* libcudnn8* tensorrt
```

### 7. Download from [NVIDIA website](https://developer.nvidia.com/deepstream-sdk-download-tesla-archived) and install the DeepStream SDK

DeepStream 6.1 for Servers and Workstations (.deb)

```
sudo apt-get install ./deepstream-6.1_6.1.0-1_amd64.deb
rm ${HOME}/.cache/gstreamer-1.0/registry.x86_64.bin
sudo ln -snf /usr/local/cuda-11.6 /usr/local/cuda
```

### 8. Reboot the computer

```
sudo reboot
```
### 9.  To run the deepstream-app for test
```
cd /opt/nvidia/deepstream/deepstream-6.1.1/samples/configs/deepstream-app
deepstream-app -c source4_1080p_dec_infer-resnet_tracker_sgie_tiled_display_int8.txt
```

</details>



## To install the DeepStream on dGPU (x86 platform), with docker.


<details><summary>DeepStream 6.3</summary>
 
## Prerequisites

* Install docker-ce by following the [official instructions](https://docs.docker.com/engine/install).
* Install ``nvidia-container-toolkit`` by following the [install-guide](https://docs.nvidia.com/datacenter/cloud-native/container-toolkit/latest/install-guide.html).
* Get an NGC account and API key:

    * Go to NGC and search the <b> DeepStream </b> in the  <b> Container </b> tab. This message is displayed: “Sign in to access the PULL feature of this repository”.

    * Enter your Email address and click <b> Next </b>, or click <b>  Create an Account </b>.

    * Choose your organization when prompted for <b> Organization/Team </b>.

    * Click <b> Sign In</b>.

* Log in to the NGC docker registry ``(nvcr.io)`` using the command ``docker login nvcr.io`` and enter the credentials:
  
### Commands to run a docker container:

````
# Pull the required docker.  Refer Docker Containers table to get docker container name.
$ docker pull nvcr.io/nvidia/deepstream:6.3-samples
# Step to run the docker
$ export DISPLAY=:0
$ xhost +
$ docker run -it --rm --net=host --gpus all -e DISPLAY=$DISPLAY --device /dev/snd -v /tmp/.X11-unix/:/tmp/.X11-unix deepstream:6.3-samples
````

<b>Note </b>: With DS 6.3, DeepStream docker containers do not package libraries necessary for certain multimedia operations like audio data parsing, CPU decode, and CPU encode. This change could affect processing certain video streams/files like mp4 that include audio track. Run the below script inside the docker images to install additional packages (e.g. `gstreamer1.0-libav, gstreamer1.0-plugins-good`, gstreamer1.0-plugins-bad, gstreamer1.0-plugins-ugly as required) that might be necessary to use all of the DeepStreamSDK features: ```/opt/nvidia/deepstream/deepstream/user_additional_install.sh```



### 7.  To run the deepstream-app for test

```
deepstream-app -c source4_1080p_dec_infer-resnet_tracker_sgie_tiled_display_int8.txt
```
 
</details>


https://github.com/prince0310/deepstream-dgpu-installation/assets/85225054/b41c5102-944e-4b7e-9cc3-4ef99dc4c7d3

