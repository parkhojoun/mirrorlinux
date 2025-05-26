# Home Assistant NPM 연결 문제 해결 가이드

## 문제 상황
- NPMplus(Nginx Proxy Manager Plus)를 통해 Home Assistant 접속 시도
- **400: Bad Request** 오류 발생
- 직접 내부 IP 접속은 정상 작동

## 오류 로그 분석
```
A request from a reverse proxy was received from 192.168.1.100, but your HTTP integration is not set-up for reverse proxies
```

## 네트워크 환경
- **NPM 서버**: `본인의 NPM서버 아이피로 수정하세요.`
- **Home Assistant**: `본인의 HA서버 아이피로 수정하세요.`
- **포트**: `8123`

## 해결 과정

### 1. NPM Proxy Host 설정

#### Details 탭
- **Domain Names**: `본인의 HA서버 도메인으로 수정하세요.`
- **Scheme**: `http`
- **Forward Hostname/IP**: `본인의 HA서버 아이피로 수정하세요`
- **Forward Port**: `8123`
- **Websockets Support**: ✅ 활성화

#### TLS 탭
- **Force HTTPS**: ❌ 비활성화
- **Enable HTTP/3-QUIC**: ✅ 활성화

#### Advanced 탭 - Custom Nginx Configuration
```nginx
proxy_set_header Host $http_host;
proxy_set_header X-Real-IP $remote_addr;
proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
proxy_set_header X-Forwarded-Proto $scheme;
proxy_set_header X-Forwarded-Host $server_name;
proxy_set_header Upgrade $http_upgrade;
proxy_set_header Connection "upgrade";
proxy_buffering off;
```

### 2. Home Assistant 설정

#### configuration.yaml 파일 위치
- Docker 환경: `/config/configuration.yaml`

#### 초기 문제
- `http:` 섹션이 존재하지 않음
- `trusted_proxies` 설정 누락

#### 올바른 설정 추가
```yaml
# Default config. See docs for configuration options
default_config:

# Text to speech
tts:
  - platform: google_translate

# Load frontend themes from the themes folder
frontend:
  themes: !include_dir_merge_named themes

automation: !include automations.yaml
script: !include scripts.yaml
scene: !include scenes.yaml

# Reverse Proxy 설정 추가
http:
  use_x_forwarded_for: true
  trusted_proxies:
    - 본인의 NPM서버 아이피로 수정하세요.  # NPM 서버 IP
```

### 3. 설정 적용 방법

1. **파일 수정 후 검증**
   - Home Assistant 웹 UI → 개발자 도구 → "구성 확인"

2. **완전 재시작 필요**
   - 단순 재시작이 아닌 완전 종료 후 재시작

3. **설정 확인**
   - 로그에서 reverse proxy 오류 메시지 사라짐 확인

## 주요 포인트

### ⚠️ 흔한 실수들
- **잘못된 IP 설정**: Home Assistant IP를 trusted_proxies에 설정 (❌)
- **올바른 설정**: NPM 서버 IP를 trusted_proxies에 설정 (✅)
- **YAML 문법**: 들여쓰기는 스페이스 2칸, 탭 문자 사용 금지
- **설정 위치**: `http:` 섹션이 누락된 경우 직접 추가 필요

### 🔧 문제 해결 단계
1. 내부 IP 직접 접속 테스트 (`본인의 HA서버 아이피로 수정하세요.`)
2. NPM 설정 확인 (Domain, Forward IP/Port)
3. Home Assistant `trusted_proxies` 설정
4. Custom Nginx Configuration 적용
5. 완전 재시작 및 로그 확인

### 📝 참고사항
- Home Assistant는 보안상 reverse proxy 접속을 제한
- 적절한 헤더와 trusted_proxies 설정 필수
- Force HTTPS 사용 시 인증서 설정 필요
