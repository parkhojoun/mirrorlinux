# Synology NAS CPU 확인 가이드

## 📋 개요

Synology NAS에서 패키지를 수동 설치하거나 업데이트할 때 CPU 아키텍처를 정확히 파악해야 합니다. 잘못된 패키지를 설치하면 시스템이 작동하지 않을 수 있습니다.

## 🔍 방법 1: DSM 웹 인터페이스로 확인 (권장)

### 1-1. 제어판 접속
- Synology NAS DSM에 로그인
- **제어판(Control Panel)** 실행

### 1-2. 하드웨어 정보 확인
- **시스템** 섹션 → **하드웨어 및 전원** 선택
- **일반** 탭에서 **CPU 모델** 확인
- CPU 세부 정보가 표시됨

### 1-3. CPU 아키텍처 판별
CPU 모델명을 확인한 후 아래 기준으로 아키텍처를 판별:

#### Intel 계열
- **64bit**: Core i3, i5, i7, Xeon, Atom C 시리즈
- **32bit**: Atom N 시리즈 (구형 모델)

#### AMD 계열  
- **64bit**: Ryzen, EPYC 시리즈

#### ARM 계열
- **64bit**: Cortex-A 시리즈 (최신 모델)
- **32bit**: ARM v7 기반 (구형 모델)

## 🖥️ 방법 2: SSH 명령어로 확인

### 2-1. SSH 활성화
- 제어판 → **연결성** → **터미널 및 SNMP**
- **SSH 서비스 활성화** 체크
- 포트 번호 확인 (기본값: 22)

### 2-2. SSH 접속
- PuTTY 또는 터미널 프로그램 사용
- 호스트명 또는 IP 주소 입력
- 지정한 포트로 연결
- NAS 관리자 계정으로 로그인

### 2-3. 명령어 실행
```bash
# 일반 사용자 쉘로 전환
Q
Y

# CPU 정보 확인
uname -a

# 더 자세한 CPU 정보
cat /proc/cpuinfo
```

### 2-4. 결과 해석
- **x86_64**: 64bit Intel/AMD 아키텍처
- **i686 또는 i386**: 32bit Intel 아키텍처  
- **armv7l**: 32bit ARM 아키텍처
- **aarch64**: 64bit ARM 아키텍처

## 📊 시리즈별 CPU 아키텍처 참고표

### 최신 시리즈 (주로 64bit)
- **DS/RS 20xx, 21xx, 22xx, 23xx 시리즈**: 대부분 64bit
- **DS/RS 19xx 시리즈**: 대부분 64bit
- **DS/RS 18xx 시리즈**: 혼재 (모델별 확인 필요)

### 구형 시리즈 (주로 32bit)
- **DS/RS 17xx 이전 시리즈**: 대부분 32bit
- **DS/RS 15xx, 16xx 시리즈**: 혼재
- **DS/RS 14xx 이전**: 주로 32bit

### J 시리즈 (Entry 모델)
- **DS220j, DS218j** 등 최신 모델: 주로 64bit
- **DS216j, DS115j** 등 구형 모델: 주로 32bit

## ⚠️ 주의사항

### 확실하지 않은 경우
- **32bit와 64bit 패키지를 모두 다운로드** 후 테스트
- 64bit 패키지 설치 실패시 32bit 패키지 시도

### 패키지 선택 기준
- **Intel 32bit**: `x86` 또는 `i386` 표시
- **Intel 64bit**: `x64`, `x86_64`, `amd64` 표시
- **ARM 32bit**: `armv7`, `arm` 표시
- **ARM 64bit**: `aarch64`, `arm64` 표시

## 💡 유용한 팁

### 모델명으로 빠른 확인
- **DS218+, DS920+**: 64bit Intel
- **DS218j, DS220j**: 64bit ARM (최신 모델)
- **DS216j, DS115j**: 32bit ARM (구형 모델)
- **DS1621+, RS1221+**: 64bit AMD

### 패키지 설치 전 확인사항
1. CPU 아키텍처 정확히 파악
2. DSM 버전 호환성 확인
3. 필요시 두 가지 아키텍처 패키지 준비

---

## 🔗 참고 링크

- [Synology 제품 사양](https://www.synology.com/ko-kr/products)
- [Synology 지원 센터](https://www.synology.com/ko-kr/support)
- [DSM 호환성 확인](https://www.synology.com/ko-kr/dsm)

## 📞 추가 지원

CPU 아키텍처 확인이 어려운 경우:
- Synology 공식 지원팀 문의
- 모델명으로 온라인 검색
- 커뮤니티 포럼 활용
