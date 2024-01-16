# raspberry-pi

## 설치하기
- 하드웨어: 라즈베리파이4 2GB 기준
- 설치OS: Raspberry Pi 4에 Ubuntu Server 20.04

## 환경설정
```sh
# apt 설치 (없을 경우)
sudo apt-get update

# 캐시 초기화
sudo rm -rf /var/cache/apt/archives/*
sudo rm -rf /var/lib/apt/lists/*
sudo apt-get clean

# 노드 현재 설치 가능 버전 확인
apt list | grep nodejs

# 노드 최신 버전 업데이트
sudo curl -fsSL https://deb.nodesource.com/setup_20.x | sudo -E bash -

# 노드 최신 버전 설치
sudo apt-get install nodejs -y
# 노드에 필요한 부수 도구 설치
sudo apt-get install gcc g++ make -y

# 크롬 브라우저 설치
sudo apt install chromium-browser chromium-codecs-ffmpeg -y
which chromium-browser

# Git 설정
sudo apt-get install git -y
which git

# Github SSH 설정
ssh-keygen -t ed25519 -C "test@gmail.com"
cat ~/.ssh/id_ed25519.pub

# Docker 설치
# 공식 문서 (https://docs.docker.com/engine/install/ubuntu/#install-using-the-repository)
# Add Docker's official GPG key:
sudo apt-get update
sudo apt-get install ca-certificates curl gnupg -y
sudo install -m 0755 -d /etc/apt/keyrings
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg
sudo chmod a+r /etc/apt/keyrings/docker.gpg

# Add the repository to Apt sources:
echo \
  "deb [arch=$(dpkg --print-architecture) signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu \
  $(. /etc/os-release && echo "$VERSION_CODENAME") stable" | \
  sudo tee /etc/apt/sources.list.d/docker.list > /dev/null
sudo apt-get update
sudo apt-get install docker-ce docker-ce-cli containerd.io docker-buildx-plugin docker-compose-plugin -y

sudo docker run hello-world

# 도커에게 계정 권한 주기
sudo usermod -aG docker {계정명}

# 도커 이미지 만들기 (라즈베리파이는 arm64 플랫폼)
sudo docker buildx build --platform=linux/arm64 --no-cache . -t {이미지이름}

# 도커 컨터이너 올리기
sudo docker run -d --network=host --name {컨테이너이름} {이미지명}
```

## 환경설정
```sh
# 캐시 초기화
sudo rm -rf /var/cache/apt/archives/*
sudo rm -rf /var/lib/apt/lists/*

# OS 업데이트 (필요할 때만 할 것)
sudo rpi-update
sudo reboot

# apt 업데이트
sudo apt-get upgrade
```

## 도커 명령어
```sh
# 라이브 컨테이너 조회
sudo docker ps                 
# 모든 컨테이너 조회
sudo docker ps -a
# 이미지 조회
sudo docker images
# 이미지 삭제
sudo docker rmi 이미지명

# 컨테이너 시작
sudo docker start 컨테이너ID
# 컨테이너 중지
sudo docker stop 컨테이너ID
# 컨테이너 삭제
sudo docker rm 컨테이너ID
# 컨테이너 재부팅
sudo docker restart 컨테이너ID
# 컨테이너의 로그 보기
sudo docker logs 컨테이너ID
# 컨테이너 내부로 접속
docker exec -it 컨테이너ID /bin/bash

# 도커 이미지 추가
sudo docker build . -t {이미지명}
# 도커 이미지 만들기 (라즈베리파이는 arm64 플랫폼)
sudo docker buildx build --platform=linux/arm64 --no-cache . -t {이미지이름}

# 도커 실행
sudo docker run -d --name {컨테이너이름} -p 8080:8080 {이미지명}
```

## 트러블슈팅
```sh
# SSH 접속 시도시 IP 키문제가 생길 경우, 키를 초기화시킴
ssh-keygen -R {아이피주소}

# 에러: -bash: /usr/bin/node: cannot execute binary file: Exec format error
# 재현: node -v 실행시 발생
sudo apt remove nodejs -y
sudo apt purge nodejs -y

# 에러: node: error while loading shared libraries: /lib/aarch64-linux-gnu/libnode.so.108: invalid ELF header
# 재현: node -v 실행시 발생
sudo mv /var/lib/apt/extended_states /var/lib/apt/extended_states_tmp
sudo apt install libnode108

# 에러: dpkg: error: fgets gave an empty string from '/var/lib/dpkg/triggers/File'
# 재현: sudo apt install libnode108 실행시 발생
sudo rm /var/lib/dpkg/triggers/File
sudo touch /var/lib/dpkg/triggers/File
sudo dpkg --configure -a

# 에러: E: Unable to parse package file /var/lib/dpkg/status (1)
# 재현: sudo apt-get update 실행시 발생
sudo rm /var/lib/dpkg/status
sudo apt-get update
```
