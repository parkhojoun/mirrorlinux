# Arch Linux 간단 가이드

## Arch Linux란?

Arch Linux는 **KISS (Keep It Simple, Stupid)** 철학을 따르는 경량화된 리눅스 배포판입니다.

### 주요 특징

- **롤링 릴리스**: 지속적인 업데이트, 버전 개념 없음
- **미니멀리즘**: 기본 설치 시 최소한의 구성 요소만 포함
- **DIY (Do It Yourself)**: 사용자가 직접 시스템을 구성
- **최신 패키지**: 항상 최신 버전의 소프트웨어 제공
- **Arch Wiki**: 세계 최고 수준의 문서화

## 패키지 관리자: Pacman

### 기본 명령어

```bash
# 시스템 전체 업데이트
sudo pacman -Syu

# 패키지 설치
sudo pacman -S 패키지명

# 패키지 제거
sudo pacman -R 패키지명

# 패키지 검색
pacman -Ss 검색어

# 설치된 패키지 목록
pacman -Q

# 패키지 정보 확인
pacman -Si 패키지명
```

### 자주 사용하는 옵션

| 명령어 | 설명 |
|--------|------|
| `pacman -Syu` | 시스템 업데이트 |
| `pacman -S package` | 패키지 설치 |
| `pacman -Rs package` | 패키지와 의존성 제거 |
| `pacman -Qdt` | 고아 패키지 찾기 |
| `pacman -Sc` | 캐시 정리 |

## AUR (Arch User Repository)

### AUR이란?
- 사용자 기여 패키지 저장소
- 공식 저장소에 없는 소프트웨어 제공
- **주의**: 신뢰할 수 있는 패키지만 설치

### AUR 헬퍼 (yay 추천)

```bash
# yay 설치 (먼저 git과 base-devel 필요)
sudo pacman -S git base-devel
cd /tmp
git clone https://aur.archlinux.org/yay.git
cd yay
makepkg -si

# yay 사용법
yay -S 패키지명        # AUR 패키지 설치
yay -Syu              # 시스템 + AUR 업데이트
yay -Ss 검색어        # AUR에서 검색
```

## 시스템 관리

### 서비스 관리 (systemd)

```bash
# 서비스 시작
sudo systemctl start 서비스명

# 서비스 활성화 (부팅 시 자동 시작)
sudo systemctl enable 서비스명

# 서비스 상태 확인
systemctl status 서비스명

# 활성화된 서비스 목록
systemctl list-unit-files --state=enabled
```

### 로그 확인

```bash
# 시스템 로그 확인
journalctl

# 최근 로그만
journalctl -n 50

# 실시간 로그
journalctl -f
```

## 설치 과정 (간략)

### 1. 부팅 미디어 준비
- ISO 다운로드 후 USB에 굽기
- UEFI/BIOS 설정에서 USB 부팅 설정

### 2. 파티셔닝
```bash
# 디스크 확인
lsblk

# 파티션 생성 (cfdisk 권장)
cfdisk /dev/sda
```

### 3. 기본 시스템 설치
```bash
# 미러 업데이트
reflector --country 'South Korea' --latest 12 --protocol https --sort rate --save /etc/pacman.d/mirrorlist

# 기본 시스템 설치
pacstrap /mnt base linux linux-firmware

# fstab 생성
genfstab -U /mnt >> /mnt/etc/fstab

# chroot 진입
arch-chroot /mnt
```

### 4. 시스템 설정
```bash
# 시간대 설정
ln -sf /usr/share/zoneinfo/Asia/Seoul /etc/localtime
hwclock --systohc

# 로케일 설정
echo "en_US.UTF-8 UTF-8" >> /etc/locale.gen
echo "ko_KR.UTF-8 UTF-8" >> /etc/locale.gen
locale-gen

# 호스트명 설정
echo "myhostname" > /etc/hostname

# root 비밀번호 설정
passwd
```

## 데스크톱 환경 설치

### GNOME 설치
```bash
sudo pacman -S gnome gnome-extra
sudo systemctl enable gdm
```

### KDE Plasma 설치
```bash
sudo pacman -S plasma kde-applications
sudo systemctl enable sddm
```

### XFCE 설치
```bash
sudo pacman -S xfce4 xfce4-goodies lightdm lightdm-gtk-greeter
sudo systemctl enable lightdm
```

## 필수 소프트웨어

### 개발 도구
```bash
sudo pacman -S base-devel git vim nano
```

### 멀티미디어
```bash
sudo pacman -S vlc firefox thunderbird
```

### 한글 입력기
```bash
sudo pacman -S ibus ibus-hangul
```

## 유용한 팁

### 1. 미러 최적화
```bash
# reflector 설치 및 사용
sudo pacman -S reflector
sudo reflector --country 'South Korea' --latest 12 --protocol https --sort rate --save /etc/pacman.d/mirrorlist
```

### 2. 시스템 백업
```bash
# timeshift 설치 (스냅샷 도구)
yay -S timeshift
```

### 3. 성능 모니터링
```bash
# htop, neofetch 설치
sudo pacman -S htop neofetch
```

### 4. 파일 관리
```bash
# ranger (터미널 파일 관리자)
sudo pacman -S ranger
```

## 문제 해결

### 패키지 충돌 해결
```bash
# 패키지 데이터베이스 동기화
sudo pacman -Syy

# 강제 업데이트
sudo pacman -Syu --overwrite '*'
```

### 부팅 문제
```bash
# Live USB로 부팅 후 chroot
mount /dev/sda2 /mnt  # 루트 파티션
arch-chroot /mnt
# 부트로더 재설치
```

### 로그 확인
```bash
# 부팅 로그
journalctl -b

# 에러 메시지만
journalctl -p err
```

## Arch Linux의 장단점

### 장점
- ✅ 최신 패키지
- ✅ 뛰어난 문서화 (Arch Wiki)
- ✅ 높은 커스터마이징 자유도
- ✅ 경량화된 시스템
- ✅ 강력한 패키지 관리자

### 단점
- ❌ 초보자에게 어려움
- ❌ 설치 과정이 복잡
- ❌ 롤링 릴리스로 인한 불안정성 가능
- ❌ 시간과 노력 많이 필요

## 추천 대상

- **적합**: 리눅스 중급자 이상, 커스터마이징을 좋아하는 사용자
- **부적합**: 리눅스 초보자, 안정성을 중시하는 서버 운영자

## 유용한 리소스

- **공식 사이트**: https://archlinux.org/
- **Arch Wiki**: https://wiki.archlinux.org/
- **설치 가이드**: https://wiki.archlinux.org/title/Installation_guide
- **한국 커뮤니티**: https://archlinux.kr/

Arch Linux는 "**설치했다고 끝이 아니라, 설치부터가 시작**"이라는 철학을 가진 배포판입니다. 시간을 투자할 가치가 있는 훌륭한 배포판이지만, 충분한 학습과 준비가 필요합니다.
