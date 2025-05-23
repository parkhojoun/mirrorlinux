# 리눅스 UFW 방화벽 설치 가이드

UFW(Uncomplicated Firewall)는 Ubuntu에서 개발된 사용하기 쉬운 방화벽 도구입니다. iptables의 복잡함을 단순화하여 초보자도 쉽게 사용할 수 있습니다.

## Ubuntu/Debian 계열

### 설치
```bash
# 패키지 목록 업데이트
sudo apt update

# UFW 설치 (대부분 기본 설치됨)
sudo apt install ufw

# 설치 확인
ufw --version
```

### 기본 설정
```bash
# UFW 상태 확인
sudo ufw status

# UFW 활성화 (주의: SSH 연결 차단될 수 있음)
sudo ufw enable

# UFW 비활성화
sudo ufw disable

# 기본 정책 설정 (들어오는 연결 차단, 나가는 연결 허용)
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

## CentOS/RHEL/Fedora 계열

### 설치 (EPEL 저장소 필요)
```bash
# EPEL 저장소 설치
sudo yum install epel-release

# UFW 설치
sudo yum install ufw

# 또는 Fedora
sudo dnf install ufw

# 설치 확인
ufw --version
```

### firewalld 비활성화 (UFW와 충돌 방지)
```bash
# firewalld 중지 및 비활성화
sudo systemctl stop firewalld
sudo systemctl disable firewalld
sudo systemctl mask firewalld

# UFW 서비스 활성화
sudo systemctl enable ufw
sudo systemctl start ufw
```

## Arch Linux

```bash
# UFW 설치
sudo pacman -S ufw

# 서비스 활성화
sudo systemctl enable ufw
sudo systemctl start ufw
```

## 기본 사용법

### 상태 확인
```bash
# UFW 상태 확인
sudo ufw status

# 자세한 상태 확인
sudo ufw status verbose

# 번호와 함께 규칙 표시
sudo ufw status numbered
```

### 기본 규칙 설정
```bash
# SSH 허용 (UFW 활성화 전 필수!)
sudo ufw allow ssh
sudo ufw allow 22

# HTTP/HTTPS 허용
sudo ufw allow http
sudo ufw allow https
sudo ufw allow 80
sudo ufw allow 443

# FTP 허용
sudo ufw allow ftp
sudo ufw allow 21
```

### 포트별 규칙 설정
```bash
# 특정 포트 허용
sudo ufw allow 8080
sudo ufw allow 3000
sudo ufw allow 25

# 포트 범위 허용
sudo ufw allow 6000:6007/tcp
sudo ufw allow 6000:6007/udp

# 특정 프로토콜 지정
sudo ufw allow 53/udp
sudo ufw allow 993/tcp
```

### IP 주소별 규칙 설정
```bash
# 특정 IP 허용
sudo ufw allow from 192.168.1.100

# 특정 IP 차단
sudo ufw deny from 192.168.1.200

# 특정 서브넷 허용
sudo ufw allow from 192.168.1.0/24

# 특정 IP에서 특정 포트로 접근 허용
sudo ufw allow from 192.168.1.100 to any port 22
sudo ufw allow from 10.0.0.0/8 to any port 80
```

### 서비스별 규칙 설정
```bash
# 일반적인 서비스 허용
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https
sudo ufw allow ftp
sudo ufw allow smtp
sudo ufw allow pop3
sudo ufw allow imap
sudo ufw allow dns

# 서비스 차단
sudo ufw deny http
sudo ufw deny ftp
```

### 고급 규칙 설정
```bash
# 특정 인터페이스에서만 허용
sudo ufw allow in on eth0 to any port 80
sudo ufw allow in on wlan0 from 192.168.1.0/24

# 나가는 연결 제한
sudo ufw deny out 25
sudo ufw allow out 53
sudo ufw allow out on eth0 to 8.8.8.8 port 53

# 로깅 포함 규칙
sudo ufw allow log 22/tcp
sudo ufw deny log from 192.168.1.100
```

## 규칙 관리

### 규칙 삭제
```bash
# 번호로 규칙 삭제
sudo ufw status numbered
sudo ufw delete 2

# 규칙 내용으로 삭제
sudo ufw delete allow 80
sudo ufw delete allow from 192.168.1.100

# 모든 규칙 삭제
sudo ufw --force reset
```

### 규칙 삽입 및 수정
```bash
# 특정 위치에 규칙 삽입
sudo ufw insert 1 allow from 192.168.1.0/24

# 규칙 수정 (삭제 후 재추가)
sudo ufw delete allow 80
sudo ufw allow 80/tcp
```

## 애플리케이션 프로필

### 사용 가능한 프로필 확인
```bash
# 애플리케이션 프로필 목록
sudo ufw app list

# 특정 프로필 정보 확인
sudo ufw app info 'Apache Full'
sudo ufw app info 'Nginx Full'
sudo ufw app info 'OpenSSH'
```

### 프로필 사용
```bash
# Apache 허용
sudo ufw allow 'Apache'
sudo ufw allow 'Apache Full'
sudo ufw allow 'Apache Secure'

# Nginx 허용
sudo ufw allow 'Nginx HTTP'
sudo ufw allow 'Nginx HTTPS'
sudo ufw allow 'Nginx Full'

# OpenSSH 허용
sudo ufw allow 'OpenSSH'
```

### 커스텀 프로필 생성
```bash
# 애플리케이션 프로필 디렉토리
sudo nano /etc/ufw/applications.d/myapp

# 프로필 내용 예제
[MyWebApp]
title=My Web Application
description=Custom web application
ports=8080,8443/tcp
```

## 로깅 설정

### 로깅 활성화
```bash
# 로깅 켜기
sudo ufw logging on

# 로깅 레벨 설정
sudo ufw logging low      # 기본
sudo ufw logging medium   # 중간
sudo ufw logging high     # 높음
sudo ufw logging full     # 전체

# 로깅 끄기
sudo ufw logging off
```

### 로그 확인
```bash
# UFW 로그 확인
sudo tail -f /var/log/ufw.log

# 시스템 로그에서 UFW 관련 확인
sudo journalctl -u ufw
sudo grep ufw /var/log/syslog
```

## 일반적인 설정 예제

### 웹서버 설정
```bash
# 기본 정책 설정
sudo ufw default deny incoming
sudo ufw default allow outgoing

# SSH 허용 (관리용)
sudo ufw allow ssh

# 웹서버 포트 허용
sudo ufw allow http
sudo ufw allow https

# 특정 관리 IP에서만 SSH 허용 (보안 강화)
sudo ufw delete allow ssh
sudo ufw allow from 192.168.1.100 to any port 22

# UFW 활성화
sudo ufw enable
```

### 데이터베이스 서버 설정
```bash
# 기본 정책
sudo ufw default deny incoming
sudo ufw default allow outgoing

# SSH 허용
sudo ufw allow ssh

# MySQL (특정 네트워크에서만)
sudo ufw allow from 192.168.1.0/24 to any port 3306

# PostgreSQL (특정 네트워크에서만)
sudo ufw allow from 192.168.1.0/24 to any port 5432

# UFW 활성화
sudo ufw enable
```

### 메일서버 설정
```bash
# 기본 정책
sudo ufw default deny incoming
sudo ufw default allow outgoing

# SSH 허용
sudo ufw allow ssh

# 메일 서비스 포트
sudo ufw allow 25    # SMTP
sudo ufw allow 110   # POP3
sudo ufw allow 143   # IMAP
sudo ufw allow 993   # IMAPS
sudo ufw allow 995   # POP3S
sudo ufw allow 587   # SMTP submission

# UFW 활성화
sudo ufw enable
```

## 문제 해결

### 일반적인 문제
```bash
# UFW 규칙 백업
sudo cp /etc/ufw/user.rules /etc/ufw/user.rules.backup
sudo cp /etc/ufw/user6.rules /etc/ufw/user6.rules.backup

# UFW 설정 초기화
sudo ufw --force reset

# UFW 서비스 재시작
sudo systemctl restart ufw

# iptables 규칙 직접 확인
sudo iptables -L -n -v
sudo ip6tables -L -n -v
```

### SSH 연결이 차단된 경우
```bash
# 복구 모드로 부팅하거나 콘솔 접근 후
sudo ufw disable
sudo ufw delete deny ssh
sudo ufw allow ssh
sudo ufw enable
```

### UFW와 Docker 충돌 해결
```bash
# Docker가 UFW 규칙을 우회하는 경우
sudo nano /etc/ufw/after.rules

# 파일 끝에 추가
*filter
:ufw-user-forward - [0:0]
:DOCKER-USER - [0:0]
-A DOCKER-USER -j ufw-user-forward
-A DOCKER-USER -j RETURN
COMMIT

# UFW 재시작
sudo systemctl restart ufw
```

## 보안 모범 사례

### 기본 보안 설정
```bash
# 기본 정책을 제한적으로 설정
sudo ufw default deny incoming
sudo ufw default deny outgoing
sudo ufw default deny routed

# 필요한 서비스만 허용
sudo ufw allow out 53        # DNS
sudo ufw allow out 80        # HTTP
sudo ufw allow out 443       # HTTPS
sudo ufw allow out 123       # NTP

# 관리 포트는 특정 IP에서만
sudo ufw allow from 192.168.1.100 to any port 22
```

### 로그 모니터링
```bash
# 로그 모니터링 스크립트 예제
#!/bin/bash
tail -f /var/log/ufw.log | while read line; do
    echo "$line" | grep "BLOCK" && echo "Blocked connection detected!"
done
```

### 자동화 스크립트
```bash
#!/bin/bash
# UFW 초기 설정 스크립트

# 기본 정책 설정
sudo ufw default deny incoming
sudo ufw default allow outgoing

# 기본 서비스 허용
sudo ufw allow ssh
sudo ufw allow http
sudo ufw allow https

# 로깅 활성화
sudo ufw logging on

# UFW 활성화
sudo ufw --force enable

echo "UFW 설정이 완료되었습니다."
sudo ufw status verbose
```
