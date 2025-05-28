# 서버 네트워크 사용량 모니터링

이 프로젝트는 리눅스 서버의 인터넷 트래픽을 실시간으로 모니터링하는 웹 대시보드입니다.

## 🧩 기능

- 📊 누적 사용량 (vnstat)
- ⚡ 실시간 속도 (proc/net/dev)
- 🎨 깔끔한 핑크 테마 UI + Chart.js 그래프
- 🔁 systemd 서비스 등록 지원

## 🚀 실행 방법

```bash
sudo apt install vnstat
sudo apt install python3-pip
pip install flask
python3 app.py
```

## 🛠️ systemd 서비스 등록

`/etc/systemd/system/netmonitor.service` 생성:

```ini
[Unit]
Description=Network Monitor Flask Web Service
After=network.target

[Service]
User=www-data
WorkingDirectory=/opt/net_monitor
ExecStart=/usr/bin/python3 app.py
Restart=always
Environment=PYTHONUNBUFFERED=1

[Install]
WantedBy=multi-user.target
```

활성화:

```bash
sudo systemctl daemon-reexec
sudo systemctl daemon-reload
sudo systemctl enable netmonitor
sudo systemctl start netmonitor
```

## 🌐 웹 UI 접속

http://<서버IP>:5000

## 📁 구성

- `app.py` – Flask 백엔드
- `templates/index.html` – HTML UI
- `static/style.css` – 핑크 테마 CSS
