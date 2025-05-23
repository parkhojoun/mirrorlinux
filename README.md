# 리눅스 Nginx 설치 가이드

리눅스에서 Nginx를 설치하는 방법은 배포판에 따라 다릅니다. 주요 배포판별 설치 방법을 알려드리겠습니다.

## Ubuntu/Debian 계열

```bash
# 패키지 목록 업데이트
sudo apt update

# Nginx 설치
sudo apt install nginx

# 설치 확인
nginx -v
```

## CentOS/RHEL/Fedora 계열

```bash
# CentOS/RHEL 7/8
sudo yum install nginx

# Fedora/RHEL 9+
sudo dnf install nginx

# EPEL 저장소가 필요한 경우 (CentOS/RHEL)
sudo yum install epel-release
sudo yum install nginx
```

## Arch Linux

```bash
sudo pacman -S nginx
```

## 설치 후 설정

```bash
# Nginx 서비스 시작
sudo systemctl start nginx

# 부팅 시 자동 시작 설정
sudo systemctl enable nginx

# 서비스 상태 확인
sudo systemctl status nginx

# Nginx 설정 테스트
sudo nginx -t

# 설정 파일 다시 로드
sudo systemctl reload nginx
```

## 방화벽 설정

```bash
# Ubuntu (ufw 사용시)
sudo ufw allow 'Nginx Full'
# 또는 개별 포트 허용
sudo ufw allow 80
sudo ufw allow 443

# CentOS/RHEL (firewalld 사용시)
sudo firewall-cmd --permanent --add-service=http
sudo firewall-cmd --permanent --add-service=https
sudo firewall-cmd --reload

# 또는 포트 번호로 허용
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --permanent --add-port=443/tcp
sudo firewall-cmd --reload
```

## 설치 확인

```bash
# 웹 브라우저에서 확인
# http://localhost 또는 http://서버IP주소

# 커맨드라인에서 확인
curl http://localhost
```

## 주요 디렉토리 및 파일

```bash
# 주요 설정 파일
/etc/nginx/nginx.conf           # 메인 설정 파일
/etc/nginx/sites-available/     # 사이트 설정 파일 (Ubuntu/Debian)
/etc/nginx/sites-enabled/       # 활성화된 사이트 (Ubuntu/Debian)
/etc/nginx/conf.d/              # 추가 설정 파일 (CentOS/RHEL)

# 웹 루트 디렉토리
/var/www/html/                  # Ubuntu/Debian
/usr/share/nginx/html/          # CentOS/RHEL

# 로그 파일
/var/log/nginx/access.log       # 액세스 로그
/var/log/nginx/error.log        # 에러 로그
```

## 기본 사이트 설정 예제

```bash
# Ubuntu/Debian - 새 사이트 설정
sudo nano /etc/nginx/sites-available/example.com

# CentOS/RHEL - 새 사이트 설정
sudo nano /etc/nginx/conf.d/example.com.conf
```

### 기본 설정 파일 예제

```nginx
server {
    listen 80;
    server_name example.com www.example.com;
    root /var/www/example.com;
    index index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    # 로그 설정
    access_log /var/log/nginx/example.com.access.log;
    error_log /var/log/nginx/example.com.error.log;
}
```

## 사이트 활성화 (Ubuntu/Debian)

```bash
# 사이트 활성화
sudo ln -s /etc/nginx/sites-available/example.com /etc/nginx/sites-enabled/

# 기본 사이트 비활성화 (선택사항)
sudo unlink /etc/nginx/sites-enabled/default

# 설정 테스트
sudo nginx -t

# Nginx 재시작
sudo systemctl restart nginx
```

## 유용한 명령어

```bash
# Nginx 버전 확인
nginx -v
nginx -V  # 자세한 정보

# 설정 파일 문법 검사
sudo nginx -t

# Nginx 프로세스 확인
ps aux | grep nginx

# Nginx 중지/시작/재시작
sudo systemctl stop nginx
sudo systemctl start nginx
sudo systemctl restart nginx
sudo systemctl reload nginx

# 로그 실시간 확인
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log
```

## SSL/HTTPS 설정 (Let's Encrypt)

```bash
# Certbot 설치 (Ubuntu/Debian)
sudo apt install certbot python3-certbot-nginx

# Certbot 설치 (CentOS/RHEL)
sudo yum install certbot python3-certbot-nginx

# SSL 인증서 발급 및 자동 설정
sudo certbot --nginx -d example.com -d www.example.com

# 인증서 자동 갱신 테스트
sudo certbot renew --dry-run
```
