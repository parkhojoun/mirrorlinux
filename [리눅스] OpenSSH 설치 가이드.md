# 리눅스 OpenSSH 설치 가이드

리눅스에서 OpenSSH를 설치하는 방법은 배포판에 따라 다릅니다. 주요 배포판별 설치 방법을 알려드리겠습니다.

## Ubuntu/Debian 계열

```bash
# 패키지 목록 업데이트
sudo apt update

# OpenSSH 서버 설치
sudo apt install openssh-server

# OpenSSH 클라이언트 설치 (보통 기본 설치됨)
sudo apt install openssh-client
```

## CentOS/RHEL/Fedora 계열

```bash
# CentOS/RHEL 7/8
sudo yum install openssh-server openssh-clients

# Fedora/RHEL 9+
sudo dnf install openssh-server openssh-clients
```

## Arch Linux

```bash
sudo pacman -S openssh
```

## 설치 후 설정

```bash
# SSH 서비스 시작
sudo systemctl start ssh          # Ubuntu/Debian
sudo systemctl start sshd         # CentOS/RHEL/Fedora

# 부팅 시 자동 시작 설정
sudo systemctl enable ssh         # Ubuntu/Debian
sudo systemctl enable sshd        # CentOS/RHEL/Fedora

# 서비스 상태 확인
sudo systemctl status ssh         # Ubuntu/Debian
sudo systemctl status sshd        # CentOS/RHEL/Fedora
```

## 방화벽 설정 (필요시)

```bash
# Ubuntu (ufw 사용시)
sudo ufw allow ssh
sudo ufw allow 22

# CentOS/RHEL (firewalld 사용시)
sudo firewall-cmd --permanent --add-service=ssh
sudo firewall-cmd --reload
```

## SSH 연결 테스트

```bash
# 로컬 연결 테스트
ssh localhost

# 원격 연결 (다른 컴퓨터에서)
ssh username@서버IP주소
```

## 추가 보안 설정

설치가 완료되면 `/etc/ssh/sshd_config` 파일에서 추가 보안 설정을 할 수 있습니다.

```bash
# 설정 파일 편집
sudo nano /etc/ssh/sshd_config

# 설정 변경 후 서비스 재시작
sudo systemctl restart ssh   # Ubuntu/Debian
sudo systemctl restart sshd  # CentOS/RHEL/Fedora
```

### 주요 보안 설정 옵션

- `Port 22` - SSH 포트 번호 (기본값: 22)
- `PermitRootLogin no` - root 계정 로그인 금지
- `PasswordAuthentication yes` - 패스워드 인증 허용/금지
- `PubkeyAuthentication yes` - 공개키 인증 허용/금지
