# Gentoo Linux 간단 가이드

## Gentoo Linux란?

Gentoo는 **소스 코드 기반**의 메타 배포판으로, 모든 패키지를 사용자 시스템에 맞춰 컴파일하는 것이 특징입니다.

### 핵심 철학
- **"Choice"** - 사용자에게 최대한의 선택권 제공
- **성능 최적화** - 각 시스템에 맞춘 최적화 컴파일
- **유연성** - 원하는 기능만 포함하여 빌드
- **학습** - 리눅스 시스템의 깊은 이해 도모

## 주요 특징

### 장점
- ✅ **극도의 커스터마이징**: 모든 것을 세밀하게 조정 가능
- ✅ **성능 최적화**: CPU 아키텍처에 맞춘 최적화
- ✅ **최신 소프트웨어**: 소스 기반이라 빠른 업데이트
- ✅ **깊은 학습**: 시스템 내부 구조 완전 이해
- ✅ **보안**: 불필요한 기능 제거로 공격 표면 축소

### 단점
- ❌ **긴 컴파일 시간**: 모든 것을 소스에서 빌드
- ❌ **높은 진입 장벽**: 상당한 Linux 지식 필요
- ❌ **시간 소모**: 설치와 관리에 많은 시간 필요
- ❌ **복잡성**: 의존성 관리가 복잡할 수 있음

## 패키지 관리자: Portage

### emerge 기본 명령어

```bash
# 패키지 설치
emerge 패키지명

# 시스템 업데이트
emerge --sync                    # Portage 트리 동기화
emerge -avuDN @world            # 전체 시스템 업데이트

# 패키지 제거
emerge --unmerge 패키지명

# 패키지 검색
emerge --search 검색어
# 또는
eix 검색어                      # eix 설치 후 사용

# 의존성 정리
emerge --depclean               # 불필요한 패키지 제거
revdep-rebuild                  # 의존성 재빌드
```

### 중요한 emerge 옵션

| 옵션 | 설명 |
|------|------|
| `-a, --ask` | 설치 전 확인 요청 |
| `-v, --verbose` | 상세한 출력 |
| `-u, --update` | 업데이트된 패키지만 |
| `-D, --deep` | 깊이 있는 의존성 검사 |
| `-N, --newuse` | USE 플래그 변경 시 재빌드 |
| `--oneshot` | world 파일에 추가하지 않음 |

## USE 플래그 시스템

USE 플래그는 Gentoo의 핵심 기능으로, 패키지 컴파일 시 포함할 기능을 선택합니다.

### USE 플래그 설정

```bash
# 전역 USE 플래그 설정
echo 'USE="X gtk gnome -kde -qt5 alsa pulseaudio"' >> /etc/portage/make.conf

# 패키지별 USE 플래그 설정
echo 'media-video/vlc dvd bluray' >> /etc/portage/package.use

# USE 플래그 확인
emerge -pv 패키지명
```

### 자주 사용하는 USE 플래그

```bash
# 데스크톱 환경
USE="X gtk gnome kde qt5 wayland"

# 멀티미디어
USE="alsa pulseaudio ffmpeg gstreamer mp3 mp4 dvd"

# 네트워킹
USE="wifi bluetooth networkmanager"

# 개발 도구
USE="git python java ruby go nodejs"
```

## 컴파일 최적화

### make.conf 설정

```bash
# /etc/portage/make.conf 예시

# CPU 최적화 (예: Intel Core i7)
COMMON_FLAGS="-march=native -O2 -pipe"
CFLAGS="${COMMON_FLAGS}"
CXXFLAGS="${COMMON_FLAGS}"
FCFLAGS="${COMMON_FLAGS}"
FFLAGS="${COMMON_FLAGS}"

# 병렬 컴파일 (CPU 코어 수 + 1)
MAKEOPTS="-j9"

# 포티지 병렬 처리
EMERGE_DEFAULT_OPTS="--jobs=4 --load-average=8"

# 미러 설정
GENTOO_MIRRORS="https://mirror.kakao.com/gentoo/"

# USE 플래그
USE="X gtk gnome pulseaudio wifi bluetooth -systemd"

# 비디오 카드 (예: NVIDIA)
VIDEO_CARDS="nvidia"

# 입력 장치
INPUT_DEVICES="libinput"
```

## 설치 과정 (개요)

### 1. 부팅 환경 준비
```bash
# 최소 설치 CD 또는 LiveGUI 사용
# 네트워크 설정
net-setup
```

### 2. 디스크 파티셔닝
```bash
# 파티션 생성
fdisk /dev/sda

# 파일시스템 생성
mkfs.ext4 /dev/sda3
mkswap /dev/sda2
```

### 3. 스테이지 3 다운로드
```bash
# 마운트
mount /dev/sda3 /mnt/gentoo
cd /mnt/gentoo

# 스테이지 3 다운로드 및 압축 해제
wget [스테이지3-URL]
tar xpf stage3-*.tar.xz --xattrs-include='*.*' --numeric-owner
```

### 4. 컴파일 옵션 설정
```bash
# make.conf 편집
nano -w /mnt/gentoo/etc/portage/make.conf
```

### 5. chroot 환경 구성
```bash
# 필요한 파일시스템 마운트
mount --types proc /proc /mnt/gentoo/proc
mount --rbind /sys /mnt/gentoo/sys
mount --rbind /dev /mnt/gentoo/dev

# chroot 진입
chroot /mnt/gentoo /bin/bash
source /etc/profile
export PS1="(chroot) ${PS1}"
```

### 6. Portage 동기화 및 기본 시스템 구성
```bash
# Portage 동기화
emerge-webrsync
emerge --sync

# 프로파일 선택
eselect profile list
eselect profile set [번호]

# 시스템 업데이트
emerge -avuDN @world
```

## 커널 컴파일

### 수동 커널 설정 (권장)
```bash
# 커널 소스 설치
emerge sys-kernel/gentoo-sources

# 커널 설정
cd /usr/src/linux
make menuconfig

# 컴파일 및 설치
make && make modules_install
make install
```

### 자동 커널 (초보자용)
```bash
# genkernel 사용
emerge sys-kernel/genkernel
genkernel all
```

## 시스템 서비스 관리

### OpenRC (기본)
```bash
# 서비스 시작
rc-service 서비스명 start

# 서비스 자동 시작 설정
rc-update add 서비스명 default

# 런레벨 확인
rc-status
```

### systemd (선택사항)
```bash
# systemd 프로파일 선택 후
systemctl enable 서비스명
systemctl start 서비스명
```

## 데스크톱 환경 설치

### GNOME
```bash
# USE 플래그 설정
echo 'USE="gnome gtk X wayland"' >> /etc/portage/make.conf

# GNOME 설치
emerge gnome-base/gnome

# 디스플레이 매니저 설정
rc-update add xdm default
echo 'DISPLAYMANAGER="gdm"' > /etc/conf.d/xdm
```

### KDE Plasma
```bash
# USE 플래그 설정
echo 'USE="kde qt5 X wayland"' >> /etc/portage/make.conf

# KDE 설치
emerge kde-plasma/plasma-meta

# SDDM 설정
rc-update add xdm default
echo 'DISPLAYMANAGER="sddm"' > /etc/conf.d/xdm
```

## 유용한 도구들

### 패키지 관리 도구
```bash
# eix - 빠른 패키지 검색
emerge app-portage/eix
eix-update

# gentoolkit - 유용한 도구 모음
emerge app-portage/gentoolkit

# layman - 오버레이 관리 (구버전)
emerge app-portage/layman

# eselect-repository - 저장소 관리 (신버전)
emerge app-eselect/eselect-repository
```

### 시스템 모니터링
```bash
# genlop - 컴파일 시간 분석
emerge app-portage/genlop
genlop -t 패키지명    # 예상 컴파일 시간
genlop -c           # 현재 컴파일 상황

# htop - 시스템 모니터링
emerge sys-process/htop
```

## 성능 최적화 팁

### 1. 컴파일 옵션 최적화
```bash
# CPU 특성에 맞춘 최적화
gcc -march=native -Q --help=target

# 링크 타임 최적화 (LTO)
COMMON_FLAGS="-march=native -O2 -pipe -flto"
```

### 2. tmpfs 활용
```bash
# /tmp를 RAM에 마운트
echo 'tmpfs /tmp tmpfs nodev,nosuid,size=4G 0 0' >> /etc/fstab

# 포티지 임시 디렉토리도 tmpfs 사용
echo 'PORTAGE_TMPDIR="/tmp"' >> /etc/portage/make.conf
```

### 3. ccache 사용
```bash
# ccache 설치 및 설정
emerge dev-util/ccache
echo 'FEATURES="ccache"' >> /etc/portage/make.conf
echo 'CCACHE_SIZE="4G"' >> /etc/portage/make.conf
```

## 문제 해결

### 의존성 문제
```bash
# 의존성 그래프 확인
emerge -pv --tree 패키지명

# 블록된 패키지 해결
emerge -C 블록패키지명
emerge 새패키지명
```

### 컴파일 실패
```bash
# 로그 확인
tail -f /var/log/portage/elog/summary.log

# 자세한 빌드 로그
less /var/tmp/portage/패키지명/temp/build.log
```

### 시스템 복구
```bash
# 라이브 CD로 부팅 후 chroot
mount /dev/sda3 /mnt/gentoo
mount /dev/sda1 /mnt/gentoo/boot
chroot /mnt/gentoo /bin/bash
source /etc/profile
```

## 추천 대상

### 적합한 사용자
- **고급 Linux 사용자**: 시스템 관리 경험이 풍부한 사용자
- **성능 최적화 추구자**: 최대 성능을 원하는 사용자
- **학습 열의가 있는 사용자**: 시스템 내부를 깊이 배우고 싶은 사용자
- **완벽한 제어를 원하는 사용자**: 모든 것을 커스터마이징하고 싶은 사용자

### 부적합한 사용자
- **Linux 초보자**: 기본적인 시스템 관리 지식이 부족한 사용자
- **빠른 설치를 원하는 사용자**: 즉시 사용 가능한 시스템이 필요한 사용자
- **시간이 부족한 사용자**: 유지보수에 시간을 투자하기 어려운 사용자

## 유용한 리소스

- **공식 홈페이지**: https://www.gentoo.org/
- **Gentoo Wiki**: https://wiki.gentoo.org/
- **Gentoo 핸드북**: https://wiki.gentoo.org/wiki/Handbook:Main_Page
- **Gentoo 포럼**: https://forums.gentoo.org/
- **한국 커뮤니티**: https://gentoo.kr/

## 마무리

Gentoo Linux는 **"Linux의 최종 보스"**라고 불릴 만큼 어려우면서도 강력한 배포판입니다. 

**"컴파일하는 동안 커피를 마시며 기다리는 것이 일상"**이 되지만, 그만큼 자신만의 완벽한 시스템을 구축할 수 있는 자유를 제공합니다.

시간과 인내심을 가지고 접근한다면, Linux 시스템에 대한 깊은 이해와 함께 세상에서 가장 최적화된 시스템을 만들 수 있습니다.
