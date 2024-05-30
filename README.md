# docker vs nondocker

도커를 사용하지 않았을 때 빌드 시간 차이가 얼마나 날까?

환경 : ec2 ubuntu 22.04

# 방법

개발후 push하는 상황

docker : git pull -> docker-compose down, up(build)

nondocker : git pull -> venv, pip install -> migrate -> restart

# 결과
1차 - 무효
![스크린샷 2024-05-30 135349](https://github.com/Gothax/docker-vs-nondocker/assets/82752784/cc7e32f9-0658-405c-a6e6-1f9e606ae997)
docker : 23s<br>
nondocker : 7s

docker의 경우 python이미지를 받는시간이 있을것이고 nondocker는 service enable, start까지 완료한 상태에서 push했기 때문

2차 (라이브러리 변화 없을때)
![스크린샷 2024-05-30 131408](https://github.com/Gothax/docker-vs-nondocker/assets/82752784/39f7f7f3-3dfa-4c96-8a0b-1c9fa229d519)

docker : 4s<br>
nondocker : 4s

3차 (라이브러리 변화 있을때)
![image](https://github.com/Gothax/docker-vs-nondocker/assets/82752784/65ef9101-be56-4ec0-b739-45e1163e7fbe)

docker : 15s<br>
nondocker : 6s


# 과정
Docker
```
1. apt update, apt install docker docker-compose
2. git clone

```

Non docker
```
1. apt update, install python, python3.10 -m venv env
2. git clone 
3. /etc/systemd/system/ -> myapp.service 생성
[Unit]
Description=gunicorn daemon for Django myapp
After=network.target

[Service]
User=ubuntu
Group=ubuntu
WorkingDirectory=/home/ubuntu/docker-vs-nondocker
ExecStart=/home/ubuntu/env/bin/gunicorn --bind 0.0.0.0:8000 config.wsgi:application

[Install]
WantedBy=multi-user.target

4. sudo systemctl daemon-reload
5. sudo systemctl enable myapp.service
6. sudo systemctl start myapp.service
```

# 결론
개발환경 구축의 편리함, 안정성으로 Docker 판정승