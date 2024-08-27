
# Install Docker OwnCloud 

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

## 2. Set up OwnCloud

### Start ownCloud simple
```bash
$ docker run -d -p 80:80 owncloud:latest
```
Add Trust Ip addres to owncloud to can acces from other computer

### Start ownCloud docker-compose
To deploy this project run with docker-compose "docker-compose.yml"

```yml
  version: "3"

volumes:
  files:
    driver: local
  mysql:
    driver: local
  redis:
    driver: local

services:
  owncloud:
    image: owncloud/server:latest
    container_name: owncloud_server
    restart: always
    ports:
      - 8080:8080
    depends_on:
      - mariadb
      - redis
    environment:
      - OWNCLOUD_DOMAIN=localhost:8080
      - OWNCLOUD_TRUSTED_DOMAINS=10.42.0.1
      - OWNCLOUD_DB_TYPE=mysql
      - OWNCLOUD_DB_NAME=owncloud
      - OWNCLOUD_DB_USERNAME=owncloud
      - OWNCLOUD_DB_PASSWORD=owncloud
      - OWNCLOUD_DB_HOST=mariadb
      - OWNCLOUD_ADMIN_USERNAME=admin
      - OWNCLOUD_ADMIN_PASSWORD=admin
      - OWNCLOUD_MYSQL_UTF8MB4=true
      - OWNCLOUD_REDIS_ENABLED=true
      - OWNCLOUD_REDIS_HOST=redis
    healthcheck:
      test: ["CMD", "/usr/bin/healthcheck"]
      interval: 30s
      timeout: 10s
      retries: 5
    volumes:
      - files:/mnt/data

  mariadb:
    image: mariadb:10.11 # minimum required ownCloud version is 10.9
    container_name: owncloud_mariadb
    restart: always
    environment:
      - MYSQL_ROOT_PASSWORD=owncloud
      - MYSQL_USER=owncloud
      - MYSQL_PASSWORD=owncloud
      - MYSQL_DATABASE=owncloud
      - MARIADB_AUTO_UPGRADE=1
    command: ["--max-allowed-packet=128M", "--innodb-log-file-size=64M"]
    healthcheck:
      test: ["CMD", "mysqladmin", "ping", "-u", "root", "--password=owncloud"]
      interval: 10s
      timeout: 5s
      retries: 5
    volumes:
      - mysql:/var/lib/mysql

  redis:
    image: redis:6
    container_name: owncloud_redis
    restart: always
    command: ["--databases", "1"]
    healthcheck:
      test: ["CMD", "redis-cli", "ping"]
      interval: 10s
      timeout: 5s
      retries: 5
    volumes:
      - redis:/data
```


## 1. Lan Konek antar 2 Ubuntu
```
mengonfigurasi sharing internet antara dua PC Ubuntu menggunakan WiFi dan kabel LAN. Berikut langkah-langkah umumnya:

PC 1 (Sumber internet):

-Terhubung ke internet via WiFi
-Memiliki port Ethernet untuk berbagi koneksi


PC 2 (Penerima internet):

-Terhubung ke PC 1 via kabel LAN
-Akan menerima koneksi internet dari PC 1


Konfigurasi pada PC 1:

-Buka Network Manager
-Pilih koneksi Ethernet
-Atur metode ke "Shared to other computers"
-Simpan pengaturan


Konfigurasi pada PC 2:

-Pastikan pengaturan jaringan dalam mode DHCP otomatis


Atur IP berbeda:

-PC 1 biasanya akan mendapatkan IP 10.42.0.1
-PC 2 akan mendapatkan IP dalam rentang 10.42.0.x


Uji koneksi:

-Dari PC 2, coba ping ke alamat umum seperti 8.8.8.8
-Buka browser untuk memastikan koneksi internet berfungsi 
```
