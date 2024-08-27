
# Install Docker Portainer

Portainer Community Edition (CE) is a lightweight platform that effortlessly delivers containerized applications. It provides seamless management of Docker, Swarm, Kubernetes, and ACI environments. With its intuitive interface and expansive API, Portainer CE empowers you to efficiently manage all your orchestrator resources, including containers, images, volumes, and networks. Effortlessly deploy and efficiently manage containers with the feature-rich toolkit and sleek graphical user interface.

Portainer Community Edition (CE) consists of a single container that can run on any cluster. It can be deployed as a Linux container or a Windows native container.

Portainer Business Edition (BE) builds on the open-source base. It includes a range of advanced features, including RBAC, Registry Management, Industrial IoT and Edge capabilities that are specific to the needs of business and enterprise users. Portainer BE is trusted by customers across various industries, including financial services, information technology, manufacturing, energy, automotive, and healthcare, to simplify the adoption of containers securely with exceptional speed.


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

## 2. Set up Portainer

### Start Portainer simple
```bash
sudo docker run -d -p 8000:8000 -p 9443:9443 --name=portainer --restart=always -v /var/run/docker.sock:/var/run/docker.sock -v portainer_data:/data portainer/portainer-ce
```
