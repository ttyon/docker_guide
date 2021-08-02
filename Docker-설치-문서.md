- [Docker 설치](#docker-설치)
- [Docker Compose 설치](#docker-compose-설치)
- [Nvidia Docker 설치](#nvidia-docker-설치)
- [기본 명령어](#기본-명령어)
- [설정 옵션](#설정-옵션)
- [참고 자료](#참고-자료)

## Docker 설치

**다음 공식 문서를 참조: [Get Docker CE for Ubuntu](https://docs.docker.com/install/linux/docker-ce/ubuntu/)**

```bash
sudo apt-get remove docker docker-engine docker.io
sudo apt-get update
sudo apt-get install -y \
    apt-transport-https \
    ca-certificates \
    curl \
    software-properties-common
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo apt-key add -
```

```bash
sudo apt-key fingerprint 0EBFCD88
```

다음 출력을 확인
```text
pub   4096R/0EBFCD88 2017-02-22
      Key fingerprint = 9DC8 5822 9FC7 DD38 854A  E2D8 8D81 803C 0EBF CD88
uid                  Docker Release (CE deb) <docker@docker.com>
sub   4096R/F273FCD8 2017-02-22
```

```bash
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
 
sudo apt-get update
sudo apt-get install -y docker-ce
```

```bash
sudo docker run hello-world
```

다음 출력을 확인
```text
Hello from Docker!
This message shows that your installation appears to be working correctly.
 
To generate this message, Docker took the following steps:
 1. The Docker client contacted the Docker daemon.
 2. The Docker daemon pulled the "hello-world" image from the Docker Hub.
    (amd64)
 3. The Docker daemon created a new container from that image which runs the
    executable that produces the output you are currently reading.
 4. The Docker daemon streamed that output to the Docker client, which sent it
    to your terminal.
 
To try something more ambitious, you can run an Ubuntu container with:
 $ docker run -it ubuntu bash
 
Share images, automate workflows, and more with a free Docker ID:
 https://cloud.docker.com/
 
For more examples and ideas, visit:
 https://docs.docker.com/engine/userguide/
```

```bash
sudo usermod -aG docker $USER
```

한 후, 모든 창을 닫고 다시 로그인하면 설치 완료


## Docker Compose 설치

**다음 공식 문서를 참조: [Install Docker Compose](https://docs.docker.com/compose/install)**

```bash
sudo curl -L https://github.com/docker/compose/releases/download/1.21.2/docker-compose-$(uname -s)-$(uname -m) -o /usr/local/bin/docker-compose
```

```bash
sudo chmod +x /usr/local/bin/docker-compose
```

```bash
docker-compose --version
```

다음 출력을 확인
```text
docker-compose version 1.21.2, build 1719ceb
```




## Nvidia Docker 설치

**다음 공식 문서를 참조: [Docker Engine Utility for NVIDIA GPUs](https://github.com/NVIDIA/nvidia-docker)**

```bash
# If you have nvidia-docker 1.0 installed: we need to remove it and all existing GPU containers
docker volume ls -q -f driver=nvidia-docker | xargs -r -I{} -n1 docker ps -q -a -f volume={} | xargs -r docker rm -f
sudo apt-get purge -y nvidia-docker
 
# Add the package repositories
curl -s -L https://nvidia.github.io/nvidia-docker/gpgkey | \
  sudo apt-key add -
distribution=$(. /etc/os-release;echo $ID$VERSION_ID)
curl -s -L https://nvidia.github.io/nvidia-docker/$distribution/nvidia-docker.list | \
  sudo tee /etc/apt/sources.list.d/nvidia-docker.list
sudo apt-get update
 
# Install nvidia-docker2 and reload the Docker daemon configuration
sudo apt-get install -y nvidia-docker2
sudo pkill -SIGHUP dockerd
 
# Test nvidia-smi with the latest official CUDA image
docker run --runtime=nvidia --rm nvidia/cuda nvidia-smi
```

## Raspberry 환경에서 Docker 설치

**다음 공식 문서를 참조: [Get Docker CE for Debian](https://docs.docker.com/install/linux/docker-ce/debian/)**

```
# Install some required packages first
sudo apt update
sudo apt install -y \
     apt-transport-https \
     ca-certificates \
     curl \
     gnupg2 \
     software-properties-common

# Get the Docker signing key for packages
curl -fsSL https://download.docker.com/linux/$(. /etc/os-release; echo "$ID")/gpg | sudo apt-key add -

# Add the Docker official repos
echo "deb [arch=armhf] https://download.docker.com/linux/$(. /etc/os-release; echo "$ID") \
     $(lsb_release -cs) stable" | \
    sudo tee /etc/apt/sources.list.d/docker.list

# Install Docker
sudo apt update
sudo apt install docker-ce

# Start and Enable Docker
sudo systemctl enable docker
sudo systemctl start docker

# Modify Permission 
sudo usermod -aG docker pi

# Test hello-world
docker run --rm arm32v7/hello-world
```

## 기본 명령어

만약 GPU 환경의 nvidia-docker를 사용해야 한다면 아래의 명령어에서 docker를 모두 nvidia-docker로 변경

docker ps -a : 현재 실행중인 모든 컨테이너 보기

docker start ~ : 컨테이너 시작하기

docker attach ~ : 컨테이너 접속하기

docker stop ~ : 컨테이너 중단하기

docker rm ~ : 컨테이너 지우기

docker image ls : 현재 가지고 있는 모든 이미지 보기

docker image rm ~ : 이미지 지우기

docker run ~ : 이미지를 컨테니어에 올리기

* -ti : bash로 시작 (만약 되지 않는다면, 맨 끝에 /bin/bash 추가
* -p A:B 옵션: host의 A포트를 컨테이어의 B포트에 연결
* -v A:B 옵견: host의 저장소 A 위치를 컨테이어의 저장소 B위치에 연결
* --name A: 해당 컨테이너의 별명을 A로 지음
* --network A: docker network A에 해당 컨테이너를 포함
* ex) docker run -ti -p 8022:22 -p 8000:8000 -v /home/mmlab:/workspace --name ubuntu_example --network mmlabnet ubuntu:16.04
    * ubuntu:16.04 이미지를 컨테이너에 올리면서, 별명은 "ubuntu_example"로 한다. 이 컨테이너는 docker network 중 mmlabnet에 속하도록 한다. 이 때, host의 /home/mmlab 폴더를 컨테이너 안의 /workspace와 연결한다. 또한 컨테이너의 포트 중 22번은 호스트의 8022번으로, 8000번 포트는 8000번으로 각각 포트 매핑을 시켜준다


## 설정 옵션

### Docker 디스크 설정: [Runtime directory and storage driver](https://docs.docker.com/config/daemon/systemd/#runtime-directory-and-storage-driver)

만약 Docker 가 설치된 디스크의 용량이 가득 차 이미지 및 컨테이너를 다른 위치로 옮기고자 한다면

```bash
sudo vi /etc/docker/daemon.json
```

맨 윗줄 괄호 이후에 다음을 추가

```text
{
    "data-root": "/mnt/docker-data",  # 원하는 Directory 설정
    "storage-driver": "overlay",
    ...
    (생략)
    ...
}
```

## 참고 자료
* [가장 빨리 만나는 도커(Docker)](http://pyrasis.com/private/2014/11/30/publish-docker-for-the-really-impatient-book)
* [초보를 위한 도커 안내서](https://subicura.com/2017/01/19/docker-guide-for-beginners-1.html)
* [컨테이너 기반 가상화 플랫폼 ‘도커(Doker)’의 이해](https://tacademy.sktechx.com/live/player/onlineLectureDetail.action?seq=125)