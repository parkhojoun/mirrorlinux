# Qbittorrent + VueTorrent UI 사용하기

저는 몇 달 전부터 쓰고 있는데 관련 글이 없는 것 같아서 작성해봅니다. 이미 기존 도커로 토렌트를 사용하시는 분들은 WebUI만 추가하시면 되는 거라 간단합니다.

## 설정 과정 개요

1. NAS에 Docker 설치
2. Docker에 qBittorrent 설치 및 설정
3. qBittorrent에 vueTorrent UI 설정
4. 아이폰에서 직접 토렌트 파일 업로드 또는 Magnet 링크 직접 붙여넣어서 파일 관리

> **참고**: 1, 2번 항목은 이미 나스 커뮤니티에 많은 분들이 설명한 글이 많아서 넘어갑니다.

## 3. qBittorrent에 vueTorrent UI 설정

Docker 폴더 아래 qBittorrent config 폴더 설정을 보통 하시기 때문에 간단합니다.

### 3-1. vueTorrent 다운로드

우선 아래 링크에서 Web UI 파일을 받습니다.

**다운로드 링크**: [https://github.com/WDaan/VueTorrent/releases](https://github.com/WDaan/VueTorrent/releases)

### 3-2. 파일 설치

1. 다운로드한 파일의 압축을 해제합니다.
2. `docker/qbittorrent/VueTorrent`에 압축을 해제해서 넣어줍니다.
   - 경로는 각자 설정에 맞게 하시면 됩니다.
   - 폴더 안에 `public` 폴더 이하 내용이 들어가면 됩니다.

### 3-3. qBittorrent 설정

1. qBittorrent에서 **Web UI를 체크**해주고 경로 설정을 해줍니다.
2. 설정 후 저장하시면 qBittorrent의 UI가 변경됩니다.

> **참고**: 저는 Docker 폴더 설정에서 `/config = /docker/qbittorrent`로 되어 있어서 경로가 다르게 보일 수 있습니다.

### 3-4. UI 변경 및 복원

- **일반적인 설정**: 상단 톱니바퀴 눌러서 설정 가능
- **원래 UI로 복원**: Web UI 항목에서 체크 해제하고 저장하면 원래의 UI로 돌아갑니다.

## 4. 아이폰에서 토렌트 파일 관리

동일한 토렌트 페이지를 아이폰에서 접속하면 모바일에 최적화된 인터페이스를 확인할 수 있습니다.

### 4-1. 토렌트 관리 방법

1. 모바일 페이지에서 **오른쪽 상단 점 3개** 있는 아이콘을 클릭
2. 토렌트 관리 메뉴에 접근 가능
3. 사파리에서 바로 다음 작업 수행 가능:
   - **토렌트 파일 직접 추가**
   - **Magnet 링크 직접 추가**

## 장점

- 별도의 앱 설치 없이 사파리 브라우저만으로 토렌트 관리 가능
- 모바일에 최적화된 깔끔한 인터페이스
- 기존 qBittorrent 설정을 그대로 유지하면서 UI만 변경
- 언제든지 원래 UI로 쉽게 복원 가능

---

*이 가이드가 도움이 되셨기를 바랍니다. vueTorrent UI를 통해 더 편리한 토렌트 관리 환경을 구축해보세요!*
