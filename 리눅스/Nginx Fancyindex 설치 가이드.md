# 리눅스 Nginx Fancyindex 설치 가이드

Nginx Fancyindex는 디렉토리 목록을 보기 좋게 표시해주는 모듈입니다. 기본 autoindex보다 훨씬 예쁘고 기능이 많습니다.

## Ubuntu/Debian 계열

### 패키지 저장소에서 설치
```bash
# 패키지 목록 업데이트
sudo apt update

# nginx-extras 패키지 설치 (fancyindex 포함)
sudo apt install nginx-extras

# 또는 nginx-full 패키지
sudo apt install nginx-full
```

### 컴파일된 모듈 설치 (Ubuntu 18.04+)
```bash
# 동적 모듈 설치
sudo apt install libnginx-mod-http-fancyindex

# Nginx 재시작
sudo systemctl restart nginx
```

### 소스에서 컴파일 설치
```bash
# 필요한 패키지 설치
sudo apt install build-essential libpcre3-dev libssl-dev zlib1g-dev

# Nginx 소스 다운로드
cd /tmp
wget http://nginx.org/download/nginx-1.24.0.tar.gz
tar -xzf nginx-1.24.0.tar.gz

# Fancyindex 모듈 다운로드
git clone https://github.com/aperezdc/ngx-fancyindex.git

# 컴파일 및 설치
cd nginx-1.24.0
./configure --add-module=../ngx-fancyindex \
    --with-http_ssl_module \
    --with-http_v2_module \
    --with-http_realip_module \
    --with-http_gzip_static_module \
    --prefix=/etc/nginx \
    --sbin-path=/usr/sbin/nginx \
    --conf-path=/etc/nginx/nginx.conf

make
sudo make install
```

## CentOS/RHEL/Fedora 계열

### EPEL 저장소 사용
```bash
# EPEL 저장소 설치
sudo yum install epel-release

# nginx-mod-http-fancyindex 설치 (있는 경우)
sudo yum install nginx-mod-http-fancyindex

# 또는 Fedora
sudo dnf install nginx-mod-http-fancyindex
```

### 소스에서 컴파일 설치
```bash
# 필요한 패키지 설치
sudo yum groupinstall "Development Tools"
sudo yum install pcre-devel openssl-devel zlib-devel

# Nginx 소스 다운로드
cd /tmp
wget http://nginx.org/download/nginx-1.24.0.tar.gz
tar -xzf nginx-1.24.0.tar.gz

# Fancyindex 모듈 다운로드
git clone https://github.com/aperezdc/ngx-fancyindex.git

# 컴파일 및 설치
cd nginx-1.24.0
./configure --add-module=../ngx-fancyindex \
    --with-http_ssl_module \
    --with-http_v2_module \
    --prefix=/etc/nginx \
    --sbin-path=/usr/sbin/nginx \
    --conf-path=/etc/nginx/nginx.conf

make
sudo make install
```

## 설치 확인

```bash
# Nginx 모듈 확인
nginx -V 2>&1 | grep -o with-http_fancyindex_module

# 또는 전체 모듈 확인
nginx -V

# Nginx 재시작
sudo systemctl restart nginx
```

## Fancyindex 설정

### 기본 설정
```bash
# 사이트 설정 파일 편집
sudo nano /etc/nginx/sites-available/default
# 또는
sudo nano /etc/nginx/conf.d/default.conf
```

### 기본 Fancyindex 설정 예제
```nginx
server {
    listen 80;
    server_name localhost;
    root /var/www/html;
    
    location / {
        # Fancyindex 활성화
        fancyindex on;
        
        # 기본 설정
        fancyindex_exact_size off;          # 파일 크기를 KB, MB 단위로 표시
        fancyindex_localtime on;            # 로컬 시간 사용
        fancyindex_name_length 50;          # 파일명 최대 길이
        
        # 헤더와 푸터 설정
        fancyindex_header "/fancyindex/header.html";
        fancyindex_footer "/fancyindex/footer.html";
        
        # 무시할 파일
        fancyindex_ignore ".*";             # 숨김 파일 무시
        fancyindex_ignore "fancyindex";     # fancyindex 디렉토리 무시
    }
    
    # Fancyindex 정적 파일 경로
    location /fancyindex/ {
        alias /var/www/fancyindex/;
    }
}
```

### 고급 설정 예제
```nginx
server {
    listen 80;
    server_name files.example.com;
    root /var/www/files;
    
    location / {
        # Fancyindex 활성화
        fancyindex on;
        
        # 표시 설정
        fancyindex_exact_size off;
        fancyindex_localtime on;
        fancyindex_name_length 255;
        fancyindex_show_path on;
        
        # CSS 스타일 적용
        fancyindex_css_href "/fancyindex/style.css";
        
        # 헤더/푸터
        fancyindex_header "/fancyindex/header.html";
        fancyindex_footer "/fancyindex/footer.html";
        
        # 무시할 파일 패턴
        fancyindex_ignore "^\.";
        fancyindex_ignore "~$";
        fancyindex_ignore "\.tmp$";
        
        # 기본 정렬 (name, date, size)
        fancyindex_default_sort name;
        fancyindex_directories_first on;
        
        # 추가 보안
        location ~* \.(php|jsp|asp|sh|py)$ {
            deny all;
        }
    }
    
    location /fancyindex/ {
        alias /var/www/fancyindex/;
        expires 30d;
    }
}
```

## 테마 및 스타일 설정

### 기본 테마 디렉토리 생성
```bash
# 테마 디렉토리 생성
sudo mkdir -p /var/www/fancyindex

# 권한 설정
sudo chown -R www-data:www-data /var/www/fancyindex
```

### CSS 스타일 파일 예제
```css
/* /var/www/fancyindex/style.css */
body {
    font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
    background-color: #f5f5f5;
    margin: 0;
    padding: 20px;
}

.container {
    max-width: 1200px;
    margin: 0 auto;
    background: white;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 10px rgba(0,0,0,0.1);
}

h1 {
    color: #333;
    border-bottom: 2px solid #007bff;
    padding-bottom: 10px;
}

table {
    width: 100%;
    border-collapse: collapse;
}

th, td {
    padding: 12px;
    text-align: left;
    border-bottom: 1px solid #ddd;
}

th {
    background-color: #007bff;
    color: white;
    font-weight: bold;
}

tr:hover {
    background-color: #f8f9fa;
}

a {
    color: #007bff;
    text-decoration: none;
}

a:hover {
    text-decoration: underline;
}

.icon {
    width: 16px;
    height: 16px;
    margin-right: 8px;
}
```

### 헤더 파일 예제
```html
<!-- /var/www/fancyindex/header.html -->
<!DOCTYPE html>
<html lang="ko">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>파일 목록</title>
    <link rel="stylesheet" href="/fancyindex/style.css">
</head>
<body>
    <div class="container">
        <h1>📁 파일 목록</h1>
        <p>아래에서 원하는 파일을 선택하세요.</p>
```

### 푸터 파일 예제
```html
<!-- /var/www/fancyindex/footer.html -->
    </div>
    <footer style="text-align: center; margin-top: 20px; padding: 20px; color: #666;">
        <p>Powered by Nginx Fancyindex</p>
    </footer>
</body>
</html>
```

## 주요 설정 옵션

```nginx
# 기본 설정
fancyindex on;                          # Fancyindex 활성화
fancyindex_exact_size off;              # 정확한 크기 표시 (off = KB, MB 단위)
fancyindex_localtime on;                # 로컬 시간 사용
fancyindex_name_length 50;              # 파일명 최대 표시 길이

# 표시 옵션
fancyindex_show_path on;                # 경로 표시
fancyindex_directories_first on;        # 디렉토리를 먼저 표시
fancyindex_default_sort name;           # 기본 정렬 (name, date, size)

# 스타일링
fancyindex_css_href "/path/to/style.css";
fancyindex_header "/path/to/header.html";
fancyindex_footer "/path/to/footer.html";

# 필터링
fancyindex_ignore "pattern";            # 특정 패턴 파일 무시
fancyindex_hide_symlinks on;            # 심볼릭 링크 숨김
```

## 보안 설정

```nginx
server {
    location / {
        fancyindex on;
        
        # 실행 파일 접근 차단
        location ~* \.(php|jsp|asp|cgi|pl|py|sh|exe)$ {
            deny all;
        }
        
        # 설정 파일 접근 차단
        location ~* \.(conf|config|ini|htaccess|htpasswd)$ {
            deny all;
        }
        
        # 숨김 파일 접근 차단
        location ~ /\. {
            deny all;
        }
        
        # 특정 IP만 허용 (선택사항)
        # allow 192.168.1.0/24;
        # deny all;
    }
}
```

## 테스트 및 문제 해결

```bash
# 설정 문법 검사
sudo nginx -t

# Nginx 재시작
sudo systemctl restart nginx

# 로그 확인
sudo tail -f /var/log/nginx/error.log

# 모듈 로드 확인
nginx -V 2>&1 | grep fancyindex

# 권한 확인
ls -la /var/www/html/
sudo chown -R www-data:www-data /var/www/html/
```

## 인기 있는 Fancyindex 테마

### 1. Nginx-Fancyindex-Flat-Theme
```bash
cd /var/www/fancyindex
wget https://github.com/Naereen/Nginx-Fancyindex-Flat-Theme/archive/master.zip
unzip master.zip
mv Nginx-Fancyindex-Flat-Theme-master/* .
```

### 2. h5ai 스타일 테마
```bash
git clone https://github.com/marcantondahmen/h5ai-nginx.git /var/www/fancyindex
```

### 설정 적용
```nginx
fancyindex_header "/fancyindex/header.html";
fancyindex_footer "/fancyindex/footer.html";
fancyindex_css_href "/fancyindex/style.css";
```
