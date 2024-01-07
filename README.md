# raspberry-pi

## 설치하기
- 하드웨어: 라즈베리파이4 2GB 기준
- 설치OS: RASPBERRY PI OS LITE (64-BIT)

## 환경설정
```sh
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

## 트러블슈팅
```sh
# SSH 접속 시도시 IP 키문제가 생길 경우, 키를 초기화시킴
ssh-keygen -R {아이피주소}

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
