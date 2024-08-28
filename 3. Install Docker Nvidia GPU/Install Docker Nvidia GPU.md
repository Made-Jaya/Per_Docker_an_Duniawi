
# Install Docker Nvidia GPU 

OwnCloud is a self-hosted file sync and share server. It provides access to your data through a web interface, sync clients or WebDAV while providing a platform to view, sync and share across devices easily—all under your control. ownCloud’s open architecture is extensible via a simple but powerful API for applications and plugins and it works with any storage.


## Prerequisites

- Linux Server running Ubuntu 20.04 LTS or newer

You can still install Docker on a Linux Server that is not running Ubuntu, however, this may require different commands!

## 1. Install Docker, and Docker-Compose
 
You can still install Docker on a Linux Server that is not running Ubuntu, however, this may require different commands!

### 1.1. Install Docker
```bash
sudo apt update

sudo apt install apt-transport-https ca-certificates curl gnupg-agent software-properties-common

curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -

sudo add-apt-repository "deb [arch=amd64] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable"

sudo apt update

sudo apt-get install docker-ce docker-ce-cli containerd.io
```

### 1.2. Check if Docker is installed correctly
```bash
sudo docker run hello-world
```

### 1.3. Install Docker-Compose

Download the latest version (in this case it is 1.25.5, this may change whenever you read this tutorial!)

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.25.5/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
```

### 1.4. Check if Docker-Compose is installed correctly
```bash
sudo docker-compose --version
```

### 1.5. (optional) Add your linux user to the `docker` group
```bash
sudo usermod -aG docker $USER
```

## 2. Set up nvidia-container-toolkit

It appears that the package nvidia-container-toolkit is not available in your current package repositories. This is likely because the NVIDIA repository hasn't been added to your system. Let's address this step-by-step:

## Initial System State

- GPU: NVIDIA GeForce GTX 1650 (4GB memory)
- NVIDIA Driver Version: 535.183.06
- CUDA Version: 12.2

### 2.1. First, let's add the NVIDIA repository. Run these commands:

```bash
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/libnvidia-container/gpgkey | sudo apt-key add -
curl -s -L https://nvidia.github.io/libnvidia-container/$distribution/libnvidia-container.list | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list
```

### 2.2. Now, update your package list:

```bash
sudo apt-get update
```

### 2.3. Then try to install the NVIDIA Container Toolkit again:

```bash
sudo apt-get install -y nvidia-container-toolkit
```
### 2.4. If successful, configure the Docker daemon:

```bash
sudo nvidia-ctk runtime configure --runtime=docker
```
### 2.5. Restart the Docker daemon:

```bash
sudo systemctl restart docker
```
### 2.6. Verify the installation:

```bash
sudo docker run --rm --gpus all nvidia/cuda:12.2.2-base-ubuntu22.04 nvidia-smi
```

### 2.7. Test the installation:

```bash
docker run --rm -it --gpus=all nvcr.io/nvidia/k8s/cuda-sample:nbody nbody -gpu -benchmark
```