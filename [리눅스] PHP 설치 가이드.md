# 리눅스 PHP 설치 가이드

리눅스에서 PHP를 설치하는 방법은 배포판과 웹서버에 따라 다릅니다. 주요 배포판별 설치 방법을 알려드리겠습니다.

## Ubuntu/Debian 계열

### Apache와 함께 설치
```bash
# 패키지 목록 업데이트
sudo apt update

# PHP와 Apache 모듈 설치
sudo apt install php libapache2-mod-php

# PHP 확장 모듈 설치 (자주 사용되는 것들)
sudo apt install php-mysql php-curl php-gd php-mbstring php-xml php-zip php-json php-cli

# Apache 재시작
sudo systemctl restart apache2
```

### Nginx와 함께 설치 (PHP-FPM)
```bash
# PHP-FPM 설치
sudo apt install php-fpm php-cli

# PHP 확장 모듈 설치
sudo apt install php-mysql php-curl php-gd php-mbstring php-xml php-zip php-json

# PHP-FPM 서비스 시작
sudo systemctl start php8.2-fpm  # 버전에 따라 다름
sudo systemctl enable php8.2-fpm
```

### 특정 PHP 버전 설치 (Ubuntu)
```bash
# Ondřej Surý PPA 추가
sudo add-apt-repository ppa:ondrej/php
sudo apt update

# PHP 8.2 설치
sudo apt install php8.2 php8.2-fpm php8.2-cli

# PHP 8.1 설치
sudo apt install php8.1 php8.1-fpm php8.1-cli

# 기본 PHP 버전 변경
sudo update-alternatives --config php
```

## CentOS/RHEL/Fedora 계열

### Apache와 함께 설치
```bash
# CentOS/RHEL 7/8
sudo yum install php php-mysql php-curl php-gd php-mbstring php-xml php-zip php-json

# Fedora/RHEL 9+
sudo dnf install php php-mysql php-curl php-gd php-mbstring php-xml php-zip php-json

# Apache 재시작
sudo systemctl restart httpd
```

### Nginx와 함께 설치 (PHP-FPM)
```bash
# PHP-FPM 설치
sudo yum install php-fpm php-cli  # CentOS/RHEL 7/8
sudo dnf install php-fpm php-cli  # Fedora/RHEL 9+

# PHP 확장 모듈 설치
sudo yum install php-mysql php-curl php-gd php-mbstring php-xml php-zip
# 또는
sudo dnf install php-mysql php-curl php-gd php-mbstring php-xml php-zip

# PHP-FPM 서비스 시작
sudo systemctl start php-fpm
sudo systemctl enable php-fpm
```

### EPEL/Remi 저장소 사용 (최신 PHP 버전)
```bash
# EPEL 저장소 설치
sudo yum install epel-release

# Remi 저장소 설치
sudo yum install https://rpms.remirepo.net/enterprise/remi-release-8.rpm

# PHP 8.2 설치
sudo yum module reset php
sudo yum module enable php:remi-8.2
sudo yum install php php-fpm php-cli
```

## Arch Linux

```bash
# PHP 설치
sudo pacman -S php php-fpm

# 일반적인 확장 모듈
sudo pacman -S php-gd php-intl php-sqlite

# Apache 모듈 (Apache 사용시)
sudo pacman -S php-apache
```

## 설치 확인

```bash
# PHP 버전 확인
php -v

# 설치된 모듈 확인
php -m

# PHP 정보 확인 (웹)
echo "<?php phpinfo(); ?>" | sudo tee /var/www/html/info.php
# 웹 브라우저에서 http://localhost/info.php 접속
```

## PHP-FPM 설정 (Nginx 사용시)

### PHP-FPM 풀 설정
```bash
# 설정 파일 편집
sudo nano /etc/php/8.2/fpm/pool.d/www.conf  # Ubuntu/Debian
sudo nano /etc/php-fpm.d/www.conf           # CentOS/RHEL
```

### 주요 설정 옵션
```ini
; 사용자 및 그룹
user = www-data
group = www-data

; 소켓 또는 TCP 설정
listen = /run/php/php8.2-fpm.sock
;listen = 127.0.0.1:9000

; 프로세스 관리
pm = dynamic
pm.max_children = 50
pm.start_servers = 5
pm.min_spare_servers = 5
pm.max_spare_servers = 35
```

### Nginx에서 PHP 설정
```bash
# Nginx 사이트 설정 편집
sudo nano /etc/nginx/sites-available/default
```

```nginx
server {
    listen 80;
    server_name localhost;
    root /var/www/html;
    index index.php index.html index.htm;

    location / {
        try_files $uri $uri/ =404;
    }

    location ~ \.php$ {
        include snippets/fastcgi-php.conf;
        fastcgi_pass unix:/run/php/php8.2-fpm.sock;
        # 또는 TCP 사용시
        # fastcgi_pass 127.0.0.1:9000;
    }

    location ~ /\.ht {
        deny all;
    }
}
```

## 자주 사용되는 PHP 확장 모듈

```bash
# Ubuntu/Debian
sudo apt install php-mysql php-mysqli php-pdo php-pgsql php-sqlite3
sudo apt install php-curl php-gd php-imagick php-mbstring php-xml
sudo apt install php-zip php-json php-bcmath php-intl php-soap
sudo apt install php-redis php-memcached php-opcache

# CentOS/RHEL/Fedora
sudo yum install php-mysql php-mysqli php-pdo php-pgsql
sudo yum install php-curl php-gd php-mbstring php-xml
sudo yum install php-zip php-json php-bcmath php-intl php-soap
sudo yum install php-redis php-pecl-memcached php-opcache
```

## PHP 설정 최적화

### php.ini 설정
```bash
# php.ini 파일 위치 확인
php --ini

# 설정 파일 편집
sudo nano /etc/php/8.2/apache2/php.ini  # Apache (Ubuntu/Debian)
sudo nano /etc/php/8.2/fpm/php.ini      # PHP-FPM (Ubuntu/Debian)
sudo nano /etc/php.ini                  # CentOS/RHEL
```

### 주요 설정 옵션
```ini
; 메모리 제한
memory_limit = 256M

; 파일 업로드 설정
upload_max_filesize = 64M
post_max_size = 64M
max_file_uploads = 20

; 실행 시간 제한
max_execution_time = 300
max_input_time = 300

; 오류 보고 (개발 환경)
display_errors = On
error_reporting = E_ALL

; 오류 보고 (운영 환경)
display_errors = Off
log_errors = On
error_log = /var/log/php_errors.log

; OPcache 설정
opcache.enable = 1
opcache.memory_consumption = 128
opcache.max_accelerated_files = 4000
opcache.revalidate_freq = 2
```

## 서비스 관리

```bash
# PHP-FPM 서비스 관리
sudo systemctl start php8.2-fpm
sudo systemctl stop php8.2-fpm
sudo systemctl restart php8.2-fpm
sudo systemctl reload php8.2-fpm
sudo systemctl status php8.2-fpm

# 설정 테스트
sudo php-fpm8.2 -t

# PHP-FPM 프로세스 확인
ps aux | grep php-fpm
```

## Composer 설치 (PHP 패키지 관리자)

```bash
# Composer 다운로드 및 설치
curl -sS https://getcomposer.org/installer | php
sudo mv composer.phar /usr/local/bin/composer

# 설치 확인
composer --version

# 프로젝트에서 사용
composer init
composer install
composer require vendor/package
```

## 유용한 명령어

```bash
# PHP 설정 정보 확인
php -i | grep "Configuration File"
php -r "phpinfo();"

# 특정 확장 모듈 확인
php -m | grep mysql
php -m | grep curl

# PHP 문법 검사
php -l script.php

# PHP 코드 실행
php -r "echo 'Hello, World!';"

# 내장 웹서버 실행 (개발용)
php -S localhost:8000 -t /var/www/html
```
