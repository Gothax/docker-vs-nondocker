# docker-vs-nondocker

# 과정
```
Non docker
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

```


```