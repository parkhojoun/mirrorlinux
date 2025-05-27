# Nginx 재시작과 재구동

## 개요

Nginx를 재시작하거나 재구동하는 방법은 운영 체제에 따라 다를 수 있습니다. 아래에서 Nginx를 재시작하고 재구동하는 방법을 안내해드리겠습니다.

## Nginx 재시작

Nginx를 재시작하는 것은 설정 변경을 적용할 때 주로 사용됩니다. 재시작하면 Nginx는 새로운 설정을 로드하고 이전 연결을 닫은 후 새로운 연결을 받아들입니다.

### Ubuntu 및 Debian

```shell
sudo service nginx restart
```

### CentOS 및 RHEL

```shell
sudo systemctl restart nginx
```

### macOS (Homebrew)

```shell
brew services restart nginx
```

### Windows

Nginx 서비스를 재시작하거나 Nginx 실행 파일을 다시 실행합니다.

## Nginx 재구동

Nginx를 재구동하는 것은 주로 Nginx 서비스 자체를 다시 시작할 때 사용됩니다. 이는 Nginx 프로세스를 중단하고 다시 시작하여 모든 설정 변경을 적용합니다.

### Ubuntu 및 Debian

```shell
sudo service nginx stop
sudo service nginx start
```

### CentOS 및 RHEL

```shell
sudo systemctl stop nginx
sudo systemctl start nginx
```

### macOS (Homebrew)

```shell
brew services stop nginx
brew services start nginx
```

### Windows

Nginx 서비스를 중지하고 다시 시작합니다.

```shell
nssm stop nginx
nssm start nginx
```

## 결론

Nginx를 재시작하거나 재구동함으로써 변경된 설정을 적용할 수 있습니다. 재시작은 설정 변경 시에 사용되며, 재구동은 Nginx 서비스 자체를 다시 시작할 때 사용됩니다. 이를 통해 Nginx의 동작을 업데이트하고 웹 서버를 안정적으로 운영할 수 있습니다.
