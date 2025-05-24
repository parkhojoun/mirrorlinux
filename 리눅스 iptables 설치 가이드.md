# 리눅스 iptables 설치 및 설정 가이드

iptables는 리눅스 커널의 netfilter 프레임워크를 관리하는 강력한 방화벽 도구입니다. 대부분의 리눅스 배포판에 기본으로 포함되어 있습니다.

## Ubuntu/Debian 계열

### 설치 확인 및 설치

```bash
# iptables 설치 확인
iptables --version

# 설치되지 않은 경우 설치
sudo apt update
sudo apt install iptables

# iptables-persistent 설치 (규칙 저장용)
sudo apt install iptables-persistent

# IPv6 지원
sudo apt install ip6tables-persistent
```

### 서비스 관리

```bash
# netfilter-persistent 서비스 활성화
sudo systemctl enable netfilter-persistent

# 현재 규칙 저장
sudo netfilter-persistent save

# 저장된 규칙 로드
sudo netfilter-persistent reload
```

## CentOS/RHEL/Fedora 계열

### CentOS/RHEL 7 이하

```bash
# iptables 설치 (보통 기본 설치됨)
sudo yum install iptables iptables-services

# iptables 서비스 활성화
sudo systemctl enable iptables
sudo systemctl enable ip6tables

# firewalld 비활성화 (충돌 방지)
sudo systemctl stop firewalld
sudo systemctl disable firewalld
sudo systemctl mask firewalld

# iptables 서비스 시작
sudo systemctl start iptables
sudo systemctl start ip6tables
```

### CentOS/RHEL 8+ / Fedora

```bash
# iptables 설치
sudo dnf install iptables iptables-services

# nftables 비활성화 (선택사항)
sudo systemctl stop nftables
sudo systemctl disable nftables

# iptables 서비스 활성화
sudo systemctl enable iptables
sudo systemctl start iptables
```

## Arch Linux

```bash
# iptables 설치 (보통 기본 설치됨)
sudo pacman -S iptables

# iptables 서비스 활성화
sudo systemctl enable iptables
sudo systemctl start iptables

# IPv6 지원
sudo systemctl enable ip6tables
sudo systemctl start ip6tables
```

## 기본 사용법

### 현재 규칙 확인

```bash
# 모든 규칙 확인
sudo iptables -L

# 자세한 정보와 함께 확인
sudo iptables -L -v -n

# 번호와 함께 확인
sudo iptables -L --line-numbers

# 특정 체인 확인
sudo iptables -L INPUT
sudo iptables -L OUTPUT
sudo iptables -L FORWARD
```

### 기본 정책 설정

```bash
# 기본 정책 설정 (주의: SSH 연결 끊어질 수 있음)
sudo iptables -P INPUT DROP
sudo iptables -P FORWARD DROP
sudo iptables -P OUTPUT ACCEPT

# 기본 정책 확인
sudo iptables -L | grep policy
```

### 기본 규칙 설정

```bash
# 로컬 루프백 허용
sudo iptables -A INPUT -i lo -j ACCEPT
sudo iptables -A OUTPUT -o lo -j ACCEPT

# 기존 연결 유지
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# SSH 허용 (22번 포트)
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# HTTP/HTTPS 허용
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT
```

## 규칙 관리

### 규칙 추가

```bash
# 특정 포트 허용
sudo iptables -A INPUT -p tcp --dport 8080 -j ACCEPT
sudo iptables -A INPUT -p udp --dport 53 -j ACCEPT

# 특정 IP 허용
sudo iptables -A INPUT -s 192.168.1.100 -j ACCEPT

# 특정 IP에서 특정 포트 허용
sudo iptables -A INPUT -s 192.168.1.100 -p tcp --dport 22 -j ACCEPT

# 포트 범위 허용
sudo iptables -A INPUT -p tcp --dport 6000:6010 -j ACCEPT

# 특정 네트워크 허용
sudo iptables -A INPUT -s 192.168.1.0/24 -j ACCEPT
```

### 규칙 삽입

```bash
# 특정 위치에 규칙 삽입
sudo iptables -I INPUT 1 -s 192.168.1.100 -j ACCEPT

# 체인의 맨 앞에 삽입
sudo iptables -I INPUT -p tcp --dport 80 -j ACCEPT
```

### 규칙 삭제

```bash
# 라인 번호로 삭제
sudo iptables -L INPUT --line-numbers
sudo iptables -D INPUT 3

# 규칙 내용으로 삭제
sudo iptables -D INPUT -p tcp --dport 80 -j ACCEPT

# 체인의 모든 규칙 삭제
sudo iptables -F INPUT
sudo iptables -F OUTPUT
sudo iptables -F FORWARD

# 모든 규칙 삭제
sudo iptables -F
```

### 규칙 교체

```bash
# 특정 라인의 규칙 교체
sudo iptables -R INPUT 2 -s 192.168.1.200 -j DROP
```

## 고급 규칙 설정

### 연결 상태 기반 필터링

```bash
# 새로운 연결만 허용
sudo iptables -A INPUT -p tcp --dport 80 -m state --state NEW -j ACCEPT

# 기존 연결과 관련 연결 허용
sudo iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# 잘못된 패킷 차단
sudo iptables -A INPUT -m state --state INVALID -j DROP
```

### 시간 기반 규칙

```bash
# 특정 시간대만 허용
sudo iptables -A INPUT -p tcp --dport 80 -m time --timestart 09:00 --timestop 18:00 -j ACCEPT

# 특정 요일만 허용
sudo iptables -A INPUT -p tcp --dport 22 -m time --weekdays Mon,Tue,Wed,Thu,Fri -j ACCEPT
```

### 연결 제한

```bash
# 동시 연결 수 제한
sudo iptables -A INPUT -p tcp --dport 80 -m connlimit --connlimit-above 10 -j REJECT

# 연결 속도 제한
sudo iptables -A INPUT -p tcp --dport 22 -m limit --limit 3/min --limit-burst 3 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 22 -j DROP
```

### MAC 주소 기반 필터링

```bash
# 특정 MAC 주소 허용
sudo iptables -A INPUT -m mac --mac-source 00:11:22:33:44:55 -j ACCEPT

# 알려진 MAC 주소만 허용
sudo iptables -A INPUT -m mac --mac-source ! 00:11:22:33:44:55 -j DROP
```

## NAT 설정

### SNAT (Source NAT)

```bash
# 특정 네트워크의 나가는 트래픽 변환
sudo iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j SNAT --to-source 203.0.113.1

# MASQUERADE (동적 IP)
sudo iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j MASQUERADE
```

### DNAT (Destination NAT)

```bash
# 포트 포워딩
sudo iptables -t nat -A PREROUTING -p tcp --dport 8080 -j DNAT --to-destination 192.168.1.100:80

# 외부에서 내부 서버로 포워딩
sudo iptables -t nat -A PREROUTING -p tcp --dport 80 -j DNAT --to-destination 192.168.1.10:8080
```

## 로깅 설정

### 기본 로깅

```bash
# 특정 규칙에 로깅 추가
sudo iptables -A INPUT -p tcp --dport 22 -j LOG --log-prefix "SSH_ACCESS: "
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# 차단된 패킷 로깅
sudo iptables -A INPUT -j LOG --log-prefix "BLOCKED: " --log-level 4
sudo iptables -A INPUT -j DROP
```

### 로그 확인

```bash
# 시스템 로그에서 iptables 로그 확인
sudo tail -f /var/log/messages | grep iptables
sudo tail -f /var/log/syslog | grep iptables
sudo journalctl -f | grep iptables

# 커널 로그 확인
sudo dmesg | grep iptables
```

## 규칙 저장 및 복원

### Ubuntu/Debian

```bash
# 현재 규칙 저장
sudo iptables-save > /etc/iptables/rules.v4
sudo ip6tables-save > /etc/iptables/rules.v6

# 또는 netfilter-persistent 사용
sudo netfilter-persistent save

# 규칙 복원
sudo iptables-restore < /etc/iptables/rules.v4
sudo netfilter-persistent reload
```

### CentOS/RHEL/Fedora

```bash
# 규칙 저장
sudo service iptables save
# 또는
sudo iptables-save > /etc/sysconfig/iptables

# 규칙 복원
sudo systemctl restart iptables
# 또는
sudo iptables-restore < /etc/sysconfig/iptables
```

## 실전 설정 예제

### 웹서버 설정

```bash
#!/bin/bash
# 웹서버용 iptables 설정

# 모든 규칙 초기화
iptables -F
iptables -X
iptables -t nat -F
iptables -t nat -X

# 기본 정책 설정
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

# 로컬 루프백 허용
iptables -A INPUT -i lo -j ACCEPT

# 기존 연결 유지
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# SSH 허용 (특정 IP에서만)
iptables -A INPUT -s 192.168.1.100 -p tcp --dport 22 -j ACCEPT

# 웹 서비스 허용
iptables -A INPUT -p tcp --dport 80 -j ACCEPT
iptables -A INPUT -p tcp --dport 443 -j ACCEPT

# ICMP 허용 (ping)
iptables -A INPUT -p icmp --icmp-type echo-request -j ACCEPT

# 로깅
iptables -A INPUT -j LOG --log-prefix "DROPPED: "

# 설정 저장
iptables-save > /etc/iptables/rules.v4
```

### 라우터/게이트웨이 설정

```bash
#!/bin/bash
# 라우터용 iptables 설정

# IP 포워딩 활성화
echo 1 > /proc/sys/net/ipv4/ip_forward

# 기본 정책
iptables -P INPUT DROP
iptables -P FORWARD ACCEPT
iptables -P OUTPUT ACCEPT

# 로컬 트래픽 허용
iptables -A INPUT -i lo -j ACCEPT

# 기존 연결 유지
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT
iptables -A FORWARD -m state --state ESTABLISHED,RELATED -j ACCEPT

# 내부 네트워크에서 외부로 나가는 트래픽 허용
iptables -A FORWARD -s 192.168.1.0/24 -j ACCEPT

# NAT 설정 (인터넷 공유)
iptables -t nat -A POSTROUTING -s 192.168.1.0/24 -o eth0 -j MASQUERADE

# SSH 관리 허용
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# DNS 허용
iptables -A INPUT -p udp --dport 53 -j ACCEPT
iptables -A INPUT -p tcp --dport 53 -j ACCEPT

# DHCP 허용
iptables -A INPUT -p udp --sport 68 --dport 67 -j ACCEPT
```

### 데이터베이스 서버 설정

```bash
#!/bin/bash
# DB 서버용 iptables 설정

# 기본 정책
iptables -P INPUT DROP
iptables -P FORWARD DROP
iptables -P OUTPUT ACCEPT

# 로컬 루프백 허용
iptables -A INPUT -i lo -j ACCEPT

# 기존 연결 유지
iptables -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# SSH 관리 허용
iptables -A INPUT -p tcp --dport 22 -j ACCEPT

# MySQL (특정 네트워크에서만)
iptables -A INPUT -s 192.168.1.0/24 -p tcp --dport 3306 -j ACCEPT

# PostgreSQL (특정 네트워크에서만)
iptables -A INPUT -s 192.168.1.0/24 -p tcp --dport 5432 -j ACCEPT

# 모니터링 도구 허용
iptables -A INPUT -s 192.168.1.200 -p tcp --dport 9100 -j ACCEPT
```

## 보안 강화 설정

### DDoS 방어

```bash
# SYN Flood 방어
iptables -A INPUT -p tcp --syn -m limit --limit 1/s --limit-burst 3 -j ACCEPT
iptables -A INPUT -p tcp --syn -j DROP

# Ping of Death 방어
iptables -A INPUT -p icmp --icmp-type echo-request -m limit --limit 1/s -j ACCEPT

# 포트 스캔 방어
iptables -A INPUT -m recent --name portscan --rcheck --seconds 86400 -j DROP
iptables -A INPUT -m recent --name portscan --remove
iptables -A INPUT -p tcp -m tcp --dport 139 -m recent --name portscan --set -j LOG --log-prefix "Portscan:"
iptables -A INPUT -p tcp -m tcp --dport 139 -m recent --name portscan --set -j DROP
```

### 스푸핑 방어

```bash
# IP 스푸핑 방어
iptables -A INPUT -s 10.0.0.0/8 -i eth0 -j DROP
iptables -A INPUT -s 172.16.0.0/12 -i eth0 -j DROP
iptables -A INPUT -s 192.168.0.0/16 -i eth0 -j DROP
iptables -A INPUT -s 224.0.0.0/4 -i eth0 -j DROP
iptables -A INPUT -s 240.0.0.0/5 -i eth0 -j DROP
```

## 문제 해결

### 긴급 복구

```bash
# 모든 규칙 제거 (긴급시)
sudo iptables -F
sudo iptables -X
sudo iptables -t nat -F
sudo iptables -t nat -X

# 기본 정책을 ACCEPT로 변경
sudo iptables -P INPUT ACCEPT
sudo iptables -P OUTPUT ACCEPT
sudo iptables -P FORWARD ACCEPT
```

### 백업 및 복원

```bash
# 현재 설정 백업
sudo iptables-save > /root/iptables-backup-$(date +%Y%m%d).txt

# 백업에서 복원
sudo iptables-restore < /root/iptables-backup-20231201.txt
```

### 규칙 테스트

```bash
# 규칙 임시 적용 (재부팅시 사라짐)
iptables -A INPUT -s 192.168.1.100 -j DROP

# 연결 테스트
telnet localhost 22
nc -zv localhost 80

# 로그 실시간 모니터링
tail -f /var/log/messages | grep iptables
```

## 유용한 스크립트

### 자동 방화벽 스크립트

```bash
#!/bin/bash
# /usr/local/bin/firewall.sh

IPTABLES="/sbin/iptables"
INTERNAL_NET="192.168.1.0/24"
TRUSTED_HOSTS="192.168.1.100 192.168.1.101"

# 초기화
$IPTABLES -F
$IPTABLES -X
$IPTABLES -t nat -F
$IPTABLES -t nat -X

# 기본 정책
$IPTABLES -P INPUT DROP
$IPTABLES -P FORWARD DROP
$IPTABLES -P OUTPUT ACCEPT

# 로컬 루프백
$IPTABLES -A INPUT -i lo -j ACCEPT

# 기존 연결
$IPTABLES -A INPUT -m state --state ESTABLISHED,RELATED -j ACCEPT

# 신뢰할 수 있는 호스트
for host in $TRUSTED_HOSTS; do
    $IPTABLES -A INPUT -s $host -j ACCEPT
done

# 서비스별 설정
$IPTABLES -A INPUT -p tcp --dport 22 -j ACCEPT  # SSH
$IPTABLES -A INPUT -p tcp --dport 80 -j ACCEPT  # HTTP
$IPTABLES -A INPUT -p tcp --dport 443 -j ACCEPT # HTTPS

# 로깅
$IPTABLES -A INPUT -j LOG --log-prefix "FIREWALL: "

echo "방화벽 설정이 완료되었습니다."
$IPTABLES -L -n -v --line-numbers
```

### 모니터링 스크립트

```bash
#!/bin/bash
# 실시간 연결 모니터링

echo "실시간 iptables 로그 모니터링..."
tail -f /var/log/messages | grep --line-buffered "FIREWALL:" | while read line; do
    echo "$(date): $line"
done
```
