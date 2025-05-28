# Rocky Linux

## 개요
Rocky Linux는 **Red Hat Enterprise Linux(RHEL)와 완전히 호환되는 무료 오픈소스 엔터프라이즈 리눅스 배포판**입니다. CentOS의 공동 창립자인 Gregory Kurtzer가 주도하여 개발했으며, "Rocky McGaugh"를 기리기 위해 명명되었습니다.

## 주요 특징

### 완벽한 RHEL 호환성
- RHEL과 1:1 바이너리 호환성 보장
- RHEL 소스코드를 기반으로 빌드
- 기존 RHEL 환경에서 무중단 마이그레이션 가능

### 엔터프라이즈급 안정성
- 프로덕션 환경에 최적화된 안정성
- 엄격한 품질 관리 프로세스
- 대기업 환경에서 검증된 신뢰성

### 장기 지원 (LTS)
- 각 메이저 릴리스마다 **10년간 지원**
- 정기적인 보안 패치 및 버그 수정
- 예측 가능한 업데이트 주기

### 커뮤니티 중심 개발
- 투명한 오픈소스 개발 프로세스
- 활발한 글로벌 커뮤니티 참여
- 기업과 개인 기여자들의 협력

## 탄생 배경

### CentOS 정책 변경
2020년 12월 Red Hat이 CentOS 8의 지원을 2029년에서 2021년으로 단축하고 CentOS Stream으로 전환한다고 발표했습니다.

### Rocky Linux 프로젝트 시작
- **Gregory Kurtzer** (CentOS 공동 창립자)가 새로운 프로젝트 발표
- CentOS의 원래 정신을 계승하는 것이 목표
- 2021년 6월 첫 번째 안정 버전 출시

### 이름의 유래
CentOS 공동 창립자 중 한 명인 **Rocky McGaugh**를 기리기 위해 "Rocky Linux"로 명명되었습니다.

## 지원 아키텍처

- **x86_64** (AMD64) - 표준 서버 및 데스크톱
- **aarch64** (ARM64) - ARM 기반 서버 및 IoT
- **ppc64le** (PowerPC) - IBM Power Systems
- **s390x** - IBM Z 시리즈 메인프레임

## 주요 사용 사례

### 엔터프라이즈 서버
- 미션 크리티컬 애플리케이션 서버
- 대용량 데이터베이스 서버
- 고가용성 웹 서버 클러스터

### 클라우드 및 가상화
- 퍼블릭/프라이빗 클라우드 인프라
- 컨테이너 오케스트레이션 플랫폼
- 가상화 환경 (VMware, KVM, Hyper-V)

### 개발 및 배포
- DevOps CI/CD 파이프라인
- 개발/테스트/스테이징 환경
- 마이크로서비스 아키텍처

### HPC 및 연구
- 고성능 컴퓨팅 클러스터
- 과학 연구 및 시뮬레이션
- 빅데이터 분석 플랫폼

## 설치 및 관리

### 시스템 업데이트
```bash
# 시스템 전체 업데이트
sudo dnf update -y

# 특정 패키지 업데이트
sudo dnf update package-name
```

### 패키지 관리
```bash
# 패키지 설치
sudo dnf install package-name

# 패키지 검색
dnf search keyword

# 설치된 패키지 목록
dnf list installed

# 패키지 정보 확인
dnf info package-name
```

### 서비스 관리
```bash
# 서비스 시작/중지/재시작
sudo systemctl start service-name
sudo systemctl stop service-name
sudo systemctl restart service-name

# 서비스 자동 시작 설정
sudo systemctl enable service-name

# 서비스 상태 확인
systemctl status service-name
```

### 방화벽 설정
```bash
# firewalld 서비스 관리
sudo systemctl start firewalld
sudo systemctl enable firewalld

# 포트 열기/닫기
sudo firewall-cmd --permanent --add-port=80/tcp
sudo firewall-cmd --permanent --remove-port=80/tcp
sudo firewall-cmd --reload
```

## 주요 장점

- ✅ **무료 및 오픈소스**: 라이선스 비용 없음
- ✅ **RHEL 완전 호환**: 기존 시스템 무중단 마이그레이션
- ✅ **장기 안정 지원**: 10년간 지속적 지원
- ✅ **엔터프라이즈 검증**: 대기업 환경에서 검증된 안정성
- ✅ **활발한 커뮤니티**: 글로벌 개발자 커뮤니티 지원
- ✅ **투명한 개발**: 오픈소스 개발 프로세스
- ✅ **다중 아키텍처**: 다양한 하드웨어 플랫폼 지원

## 주요 단점

- ❌ **상대적 신생**: AlmaLinux 대비 늦은 시작
- ❌ **에코시스템 성숙도**: RHEL/CentOS 대비 작은 생태계
- ❌ **기업 지원**: 공식 기업 지원 서비스 제한적

## 다른 배포판과의 상세 비교

| 특징 | Rocky Linux | AlmaLinux | CentOS Stream | RHEL |
|------|-------------|-----------|---------------|------|
| **RHEL 호환성** | 완전 호환 | 완전 호환 | 개발 버전 | 원본 |
| **지원 기간** | 10년 | 10년 | 짧음 | 10년+ |
| **릴리스 주기** | RHEL 후속 | RHEL 후속 | RHEL 선행 | 정기 |
| **안정성** | 매우 높음 | 매우 높음 | 중간 | 최고 |
| **비용** | 무료 | 무료 | 무료 | 유료 |
| **기업 지원** | 제한적 | 제한적 | 제한적 | 완전 |
| **커뮤니티** | 활발 | 활발 | 보통 | 공식 |

## 버전 및 릴리스 정보

### 현재 버전
- **Rocky Linux 9.x** (2022년 출시)
- **Rocky Linux 8.x** (2021년 출시)

### 업데이트 주기
- **메이저 릴리스**: RHEL 메이저 버전 출시 후 수개월 내
- **마이너 업데이트**: 분기별 정기 업데이트
- **보안 패치**: 필요시 즉시 릴리스

## 마이그레이션 가이드

### CentOS에서 Rocky Linux로
```bash
# migrate2rocky 스크립트 사용
curl -O https://raw.githubusercontent.com/rocky-linux/rocky-tools/main/migrate2rocky/migrate2rocky.sh
sudo bash migrate2rocky.sh -r
```

### 시스템 검증
```bash
# OS 정보 확인
cat /etc/os-release

# 커널 정보 확인
uname -r

# 설치된 패키지 확인
rpm -qa | grep rocky
```

## 커뮤니티 및 지원

### 공식 채널
- **웹사이트**: https://rockylinux.org
- **GitHub**: https://github.com/rocky-linux
- **포럼**: https://forums.rockylinux.org
- **Mattermost**: 실시간 커뮤니티 채팅

### 문서 및 학습 자료
- 공식 문서 및 가이드
- 커뮤니티 튜토리얼
- 비디오 교육 자료
- 웨비나 및 온라인 세미나

## 결론

Rocky Linux는 CentOS의 정신적 계승자로서 엔터프라이즈 환경에서 안정적이고 신뢰할 수 있는 무료 대안을 제공합니다. RHEL과의 완벽한 호환성, 장기 지원, 그리고 활발한 커뮤니티를 바탕으로 기업용 서버 운영에 최적화된 선택지입니다. 특히 CentOS 사용자들에게는 자연스러운 마이그레이션 경로를 제공하며, 새로운 프로젝트에서도 검증된 안정성을 바탕으로 안심하고 사용할 수 있는 배포판입니다.
