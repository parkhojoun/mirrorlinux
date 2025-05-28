# Synology NAS에서 최신 Plex Media Server 설치 가이드

## 📋 개요

시놀로지 패키지 센터에서 제공하는 Plex Media Server는 업데이트가 늦어 버전이 낮습니다. 따라서 Plex 공식 홈페이지에서 최신 설치 파일을 다운로드하여 직접 설치하는 방법을 안내합니다.

## 🔧 사전 준비 작업

- **Plex 계정 가입**: [https://plex.tv](https://plex.tv)에서 회원가입 (Gmail, iCloud 등 연동 가능)
- **CPU 아키텍처 확인**: [시놀로지 CPU 확인 페이지](https://kb.synology.com/ko-kr/DSM/tutorial/What_kind_of_CPU_does_my_NAS_have)

## 📥 1단계: 설치 파일 다운로드

### 1-1. Plex 다운로드 페이지 접속
- [https://plex.tv/download](https://plex.tv/download) 접속

### 1-2. 설치 대상 선택
- **Windows** 드롭다운 클릭 → **Synology (DSM 7)** 선택
- DSM 버전에 맞는 옵션 선택

### 1-3. CPU 아키텍처별 패키지 선택
- Intel 32bit 또는 64bit 선택
- **확실하지 않은 경우**: 32bit, 64bit 모두 다운로드 권장

### 1-4. 파일 저장
- PC 로컬 폴더에 저장 (나스에 직접 저장할 필요 없음)

## ⚙️ 2단계: NAS에 Plex Media Server 설치

### 2-1. 시놀로지 패키지 센터 접속
- DSM에 로그인 → **패키지 센터** 실행

### 2-2. 수동 설치 시작
- **수동 설치** 버튼 클릭

### 2-3. 패키지 파일 업로드
- **찾아보기** 클릭 → PC에서 다운로드한 설치 파일 선택
- ⚠️ **주의**: 나스에 파일을 복사할 필요 없음, PC에서 직접 선택

### 2-4. 설치 진행
- 선택 완료 후 **다음** 클릭

### 2-5. 경고 문구 동의
- 비공식 패키지 설치 경고 → **동의** 선택

### 2-6. 설치 타입 및 Claim Token 설정
#### Installation Type
- **NEW or Lost servers only** 선택 (처음 설치 또는 재설치시)

#### Claim Token 획득 (필수)
1. **Get Plex Claim Token** 버튼 클릭
2. Plex 웹사이트로 이동하여 로그인
3. 생성된 Claim Code를 **Copy to Clipboard**로 복사
4. 시놀로지 설치 화면으로 돌아와 **Ctrl + V**로 붙여넣기

### 2-7. 설치 완료
- 설정 확인 후 **완료** 버튼 클릭
- 설치 진행 대기
- 설치 완료 후 **확인** 클릭

### 2-8. 설치 확인
- 패키지 센터 → **설치됨** 탭에서 **Plex Media Server** 확인

## ✅ 설치 완료

Plex Media Server 설치가 완료되었습니다. 이제 라이브러리 설정 및 권한 설정을 통해 미디어 서버를 구성할 수 있습니다.

---

## 💡 주요 팁

- **CPU 아키텍처 불확실시**: 32bit, 64bit 모두 다운로드하여 테스트
- **파일 위치**: PC 로컬에 저장된 파일로 직접 설치 가능
- **Claim Token**: 설치 과정에서 반드시 필요한 필수 항목
- **업데이트**: 공식 홈페이지에서 다운로드하여 최신 버전 유지 가능

## 🔗 참고 링크

- [Plex 다운로드](https://plex.tv/download)
- [시놀로지 CPU 확인](https://kb.synology.com/ko-kr/DSM/tutorial/What_kind_of_CPU_does_my_NAS_have)
