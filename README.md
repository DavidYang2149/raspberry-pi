# raspberry-pi

## 설치하기
- 하드웨어: 라즈베리파이4 2GB 기준
- 설치OS: RASPBERRY PI OS LITE (64-BIT)

## 환경설정
```sh
# IP 키문제가 생길 경우, 키를 초기화시킴
ssh-keygen -R {아이피주소}

# OS 업데이트
sudo rpi-update
sudo reboot

# apt 설치(없을 경우)
sudo apt-get update

# 크롬 브라우저 설치
sudo apt install chromium-browser

# NPM 설치
sudo apt install npm

# 노드 설치
sudo apt install nodejs
```
