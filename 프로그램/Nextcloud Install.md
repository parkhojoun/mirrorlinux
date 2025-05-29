# 데비안에서 Nextcloud 설치 가이드

이 문서는 데비안 시스템에서 Nextcloud를 처음부터 설치하는 완전한 가이드입니다.

## 시작하기 전에

### 요구사항
- 데비안 10 (Buster) 이상
- 최소 512MB RAM (2GB 권장)
- 최소 10GB 디스크 공간
- 루트 또는 sudo 권한

### 예상 소요 시간
- 약 30-45분

## 시스템 준비

먼저 시스템을 최신 상태로 업데이트하고 필수 패키지를 설치합니다.

```bash
# 시스템 업데이트
sudo apt update && sudo apt upgrade -y

# 필수 도구 설치
sudo apt install wget curl gnupg2 software-properties-common apt-transport-https -y
```

## 웹서버 설치

Apache 웹서버를 설치하고 필요한 모듈을 활성화합니다.

```bash
# Apache 설치
sudo apt install apache2 -y

# Apache 서비스 시작 및 자동 시작 설정
sudo systemctl enable apache2
sudo systemctl start apache2

# Nextcloud에 필요한 Apache 모듈 활성화
sudo a2enmod rewrite
sudo a2enmod headers
sudo a2enmod env
sudo a2enmod dir
sudo a2enmod mime
sudo a2enmod ssl
```

설치 확인:
```bash
sudo systemctl status apache2
```

## 데이터베이스 설치

MariaDB를 설치하고 보안 설정을 진행합니다.

```bash
# MariaDB 설치
sudo apt install mariadb-server mariadb-client -y

# MariaDB 서비스 시작 및 자동 시작 설정
sudo systemctl enable mariadb
sudo systemctl start mariadb

# 데이터베이스 보안 설정 (대화형)
sudo mysql_secure_installation
```

### 보안 설정 시 선택사항:
- **Set root password?** → Y (강력한 비밀번호 설정)
- **Remove anonymous users?** → Y
- **Disallow root login remotely?** → Y
- **Remove test database?** → Y
- **Reload privilege tables?** → Y

## PHP 설치

PHP 8.1과 Nextcloud에 필요한 확장 모듈들을 설치합니다.

```bash
# PHP 및 필수 확장 모듈 설치
sudo apt install php php-fpm php-mysql php-xml php-curl php-zip php-gd \
php-mbstring php-intl php-bcmath php-json php-imagick php-bz2 \
php-apcu libapache2-mod-php -y
```

PHP 버전 확인:
```bash
php --version
```

## 데이터베이스 설정

Nextcloud 전용 데이터베이스와 사용자를 생성합니다.

```bash
# MariaDB 접속
sudo mysql -u root -p
```

MariaDB 콘솔에서 다음 명령을 실행:

```sql
-- 데이터베이스 생성
CREATE DATABASE nextcloud CHARACTER SET utf8mb4 COLLATE utf8mb4_general_ci;

-- 사용자 생성 및 권한 부여 (비밀번호는 강력하게 설정)
CREATE USER 'nextclouduser'@'localhost' IDENTIFIED BY 'your_strong_password_here';
GRANT ALL PRIVILEGES ON nextcloud.* TO 'nextclouduser'@'localhost';

-- 권한 적용
FLUSH PRIVILEGES;

-- 종료
EXIT;
```

> **보안 팁**: `your_strong_password_here`를 실제 강력한 비밀번호로 변경하세요.

## Nextcloud 다운로드

최신 버전의 Nextcloud를 다운로드하고 설치합니다.

```bash
# 임시 디렉토리로 이동
cd /tmp

# 최신 Nextcloud 다운로드
wget https://download.nextcloud.com/server/releases/latest.tar.bz2

# 압축 해제
tar -xjf latest.tar.bz2

# 웹 루트로 이동
sudo mv nextcloud /var/www/html/

# 소유권 및 권한 설정
sudo chown -R www-data:www-data /var/www/html/nextcloud
sudo chmod -R 755 /var/www/html/nextcloud
```

## Apache 설정

Nextcloud용 Apache 가상 호스트를 설정합니다.

```bash
# 가상 호스트 설정 파일 생성
sudo nano /etc/apache2/sites-available/nextcloud.conf
```

다음 내용을 파일에 입력:

```apache
<VirtualHost *:80>
    ServerName your-domain.com
    ServerAlias www.your-domain.com
    DocumentRoot /var/www/html/nextcloud
    
    <Directory /var/www/html/nextcloud>
        Options +FollowSymlinks
        AllowOverride All
        Require all granted
        
        <IfModule mod_dav.c>
            Dav off
        </IfModule>
        
        SetEnv HOME /var/www/html/nextcloud
        SetEnv HTTP_HOME /var/www/html/nextcloud
    </Directory>
    
    ErrorLog ${APACHE_LOG_DIR}/nextcloud_error.log
    CustomLog ${APACHE_LOG_DIR}/nextcloud_access.log combined
</VirtualHost>
```

가상 호스트 활성화:
```bash
# Nextcloud 사이트 활성화
sudo a2ensite nextcloud.conf

# 기본 사이트 비활성화
sudo a2dissite 000-default.conf

# Apache 설정 테스트
sudo apache2ctl configtest

# Apache 재시작
sudo systemctl reload apache2
```

> **참고**: `your-domain.com`을 실제 도메인명 또는 서버 IP로 변경하세요.

## PHP 최적화

Nextcloud 성능을 위해 PHP 설정을 최적화합니다.

```bash
# PHP 설정 파일 편집 (버전에 따라 경로가 다를 수 있음)
sudo nano /etc/php/8.1/apache2/php.ini
```

다음 값들을 찾아서 수정:

```ini
# 메모리 제한
memory_limit = 512M

# 파일 업로드 제한
upload_max_filesize = 200M
post_max_size = 200M

# 실행 시간 제한
max_execution_time = 300
max_input_time = 300

# OPcache 설정 (성능 향상)
opcache.enable = 1
opcache.memory_consumption = 128
opcache.max_accelerated_files = 10000
opcache.revalidate_freq = 1
```

설정 적용:
```bash
# Apache 재시작
sudo systemctl restart apache2

# PHP 설정 확인
php -m | grep opcache
```

## 웹 설치 완료

브라우저에서 Nextcloud 설치를 완료합니다.

1. **브라우저에서 접속**
   - `http://your-server-ip` 또는 `http://your-domain.com`

2. **관리자 계정 생성**
   - 관리자 사용자명 입력
   - 강력한 비밀번호 입력

3. **데이터 디렉토리 설정**
   - 기본값 사용 권장: `/var/www/html/nextcloud/data`

4. **데이터베이스 연결 설정**
   - 데이터베이스 유형: **MySQL/MariaDB** 선택
   - 데이터베이스 사용자: `nextclouduser`
   - 데이터베이스 비밀번호: 앞서 설정한 비밀번호
   - 데이터베이스명: `nextcloud`
   - 데이터베이스 호스트: `localhost`

5. **설치 완료**
   - "설치 완료" 버튼 클릭
   - 몇 분 정도 소요될 수 있습니다.

## 보안 강화

### SSL 인증서 설치 (Let's Encrypt)

```bash
# Certbot 설치
sudo apt install certbot python3-certbot-apache -y

# SSL 인증서 발급 및 자동 설정
sudo certbot --apache -d your-domain.com -d www.your-domain.com

# 자동 갱신 테스트
sudo certbot renew --dry-run
```

### 방화벽 설정

```bash
# UFW 방화벽 설치 (설치되지 않은 경우)
sudo apt install ufw -y

# HTTP/HTTPS 트래픽 허용
sudo ufw allow 'Apache Full'

# SSH 접속 허용 (원격 관리용)
sudo ufw allow ssh

# 방화벽 활성화
sudo ufw enable

# 상태 확인
sudo ufw status
```

### 추가 보안 설정

#### fail2ban 설치 (무차별 대입 공격 방지)
```bash
sudo apt install fail2ban -y
sudo systemctl enable fail2ban
sudo systemctl start fail2ban
```

#### Nextcloud 보안 스캔
Nextcloud 관리자 패널에서 "보안 및 설정 경고" 확인하고 권장사항을 따르세요.

## 문제 해결

### 일반적인 문제들

#### 1. "내부 서버 오류" 발생 시
```bash
# Apache 오류 로그 확인
sudo tail -f /var/log/apache2/nextcloud_error.log

# PHP 오류 로그 확인
sudo tail -f /var/log/apache2/error.log
```

#### 2. 파일 업로드 실패 시
```bash
# 디렉토리 권한 확인
ls -la /var/www/html/nextcloud/

# 권한 재설정
sudo chown -R www-data:www-data /var/www/html/nextcloud
sudo chmod -R 755 /var/www/html/nextcloud
```

#### 3. 데이터베이스 연결 오류 시
```bash
# MariaDB 상태 확인
sudo systemctl status mariadb

# 데이터베이스 연결 테스트
mysql -u nextclouduser -p -h localhost nextcloud
```

#### 4. PHP 모듈 누락 오류 시
```bash
# 설치된 PHP 모듈 확인
php -m

# 누락된 모듈 설치 (예: php-zip)
sudo apt install php-zip -y
sudo systemctl restart apache2
```

### 성능 최적화

#### Redis 캐시 설정 (선택사항)
```bash
# Redis 설치
sudo apt install redis-server php-redis -y

# Redis 서비스 시작
sudo systemctl enable redis-server
sudo systemctl start redis-server
```

Nextcloud config.php에 Redis 설정 추가:
```bash
sudo nano /var/www/html/nextcloud/config/config.php
```

## 유지보수

### 정기 업데이트
- Nextcloud 웹 인터페이스를 통한 업데이트
- 또는 `sudo -u www-data php /var/www/html/nextcloud/updater/updater.phar` 명령 사용

### 백업 권장사항
1. **데이터베이스 백업**
   ```bash
   mysqldump -u root -p nextcloud > nextcloud_backup_$(date +%Y%m%d).sql
   ```

2. **파일 백업**
   ```bash
   tar -czf nextcloud_files_$(date +%Y%m%d).tar.gz /var/www/html/nextcloud
   ```

## 마무리

축하합니다! 데비안에서 Nextcloud가 성공적으로 설치되었습니다. 이제 다음과 같은 기능들을 사용할 수 있습니다:

- **파일 동기화 및 공유**
- **캘린더 및 연락처**
- **온라인 오피스 도구**
- **사진 갤러리**
- **비디오 통화**
- **확장 앱 설치**

추가 설정이나 문제가 발생하면 [Nextcloud 공식 문서](https://docs.nextcloud.com/)를 참조하세요.

---

**작성자**: Claude  
**최종 수정**: 2025년 5월  
**버전**: 1.0