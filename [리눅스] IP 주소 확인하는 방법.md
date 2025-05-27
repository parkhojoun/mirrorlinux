# 리눅스에서 IP 주소 확인하는 방법

리눅스 시스템에서 네트워크 인터페이스의 IP 주소를 확인하는 다양한 방법들을 소개합니다.

## 1. ip 명령어 사용 (권장 방법)

최신 리눅스 배포판에서 권장되는 방법입니다.

### 기본 사용법

```bash
# 모든 네트워크 인터페이스 정보 보기
ip addr show

# 줄여서 사용 가능
ip a
```

### 특정 인터페이스 확인

```bash
# 특정 인터페이스만 보기
ip addr show eth0
ip a show enp0s3
```

### IPv4 주소만 필터링

```bash
# IPv4 주소만 간단히 보기
ip -4 addr show | grep inet

# 더 간단한 출력
ip -4 -o addr show | awk '{print $2, $4}'
```

## 2. ifconfig 명령어 사용 (전통적인 방법)

오래된 방법이지만 여전히 널리 사용됩니다.

### 기본 사용법

```bash
# 모든 인터페이스 정보 보기
ifconfig

# 활성화된 모든 인터페이스 보기
ifconfig -a
```

### 특정 인터페이스 확인

```bash
# 특정 인터페이스만 보기
ifconfig eth0
ifconfig wlan0
```

> **참고**: `ifconfig`가 없다면 다음 명령어로 설치할 수 있습니다:
> ```bash
> # Ubuntu/Debian
> sudo apt install net-tools
> 
> # CentOS/RHEL/Fedora
> sudo yum install net-tools  # 또는 dnf install net-tools
> ```

## 3. hostname 명령어 사용

간단하고 빠른 방법입니다.

```bash
# 시스템의 모든 IP 주소 표시
hostname -I

# 로컬호스트 IP 주소 표시
hostname -i
```

## 4. 외부 IP 주소 확인

인터넷에서 보이는 공인 IP 주소를 확인하는 방법입니다.

```bash
# 다양한 서비스 이용
curl ifconfig.me
curl ipinfo.io/ip
curl icanhazip.com
curl ipecho.net/plain

# wget 사용
wget -qO- ifconfig.me
```

## 5. 네트워크 연결 상태 확인

### ss 명령어 (권장)

```bash
# 모든 소켓 정보
ss -tuln

# TCP 연결만
ss -tln

# UDP 연결만
ss -uln
```

### netstat 명령어

```bash
# 네트워크 연결 상태
netstat -tuln

# 라우팅 테이블
netstat -rn
```

## 6. 시스템 파일에서 직접 확인

### 네트워크 설정 파일들

```bash
# 네트워크 라우팅 정보
cat /proc/net/route

# DNS 설정
cat /etc/resolv.conf

# 네트워크 인터페이스 설정 (Ubuntu/Debian)
cat /etc/netplan/*.yaml

# 네트워크 설정 (CentOS/RHEL)
ls /etc/sysconfig/network-scripts/ifcfg-*
```

## 7. 실용적인 원라이너 명령어들

### 주요 IP 주소만 추출

```bash
# 기본 IP 주소 (첫 번째 인터페이스)
ip route get 1.1.1.1 | grep -oP 'src \K\S+'

# 모든 IPv4 주소 나열
ip -4 addr | grep -oP '(?<=inet\s)\d+(\.\d+){3}'

# 게이트웨이 주소
ip route | grep default | awk '{print $3}'
```

### 네트워크 인터페이스 상태 확인

```bash
# 활성 인터페이스만
ip link show up

# 인터페이스별 통계
cat /proc/net/dev
```

## 8. 그래픽 환경에서 확인

### GUI 도구들

- **네트워크 관리자**: `nm-connection-editor`
- **시스템 설정**: 각 데스크톱 환경의 네트워크 설정
- **터미널 기반 TUI**: `nmtui`

```bash
# NetworkManager TUI 실행
nmtui
```

## 빠른 참조 표

| 명령어 | 용도 | 설치 필요 여부 |
|--------|------|----------------|
| `ip a` | 모든 인터페이스 정보 | 기본 설치됨 |
| `hostname -I` | 시스템 IP 주소 | 기본 설치됨 |
| `ifconfig` | 전통적인 방법 | net-tools 필요 |
| `curl ifconfig.me` | 외부 IP 확인 | curl 필요 |

## 문제 해결 팁

### 일반적인 문제들

1. **명령어를 찾을 수 없는 경우**
   ```bash
   # PATH 확인
   echo $PATH
   
   # 명령어 위치 찾기
   which ip
   whereis ifconfig
   ```

2. **권한 문제**
   ```bash
   # 일부 명령어는 관리자 권한 필요
   sudo ip addr show
   ```

3. **네트워크 인터페이스가 보이지 않는 경우**
   ```bash
   # 모든 인터페이스 (비활성 포함)
   ip link show
   ifconfig -a
   ```

이 가이드를 통해 리눅스 시스템에서 IP 주소를 확인하는 다양한 방법을 활용하실 수 있습니다.
