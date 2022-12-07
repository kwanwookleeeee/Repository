# APM 소스설치 (Apache, PHP, MySQL)

- 사용버전

|CentOS 7.9|openssl|pcre|apache|apr|apr-util|mysql|php|
|--------|-------|----|------|---|--------|-----|---|
|2009|1.1.1b|8.43|2.4.54|1.6.5|1.6.1|8.0.31|7.3.3|


## MySQL 설치  

#
MySQL 다운로드 링크 = https://dev.mysql.com/downloads/mysql
```
# mkdir -p /usr/local/src/mysql8
```
```
# cd /usr/local/src/mysql8
```
```
# wget 해당링크
```
```
# tar -xvf mysql-8.0.15-1.el7.x86_64.rpm-bundle.tar
```
```
# yum erase mariadb*
```
```
# yum install openssl-devel
# yum install net-tools
```
```
# rpm -ivh mysql-community-common-8.0.15-1.el7.x86_64.rpm
# rpm -ivh mysql-community-libs-8.0.15-1.el7.x86_64.rpm
# rpm -ivh mysql-community-devel-8.0.15-1.el7.x86_64.rpm
# rpm -ivh mysql-community-client-8.0.15-1.el7.x86_64.rpm
# rpm -ivh mysql-community-server-8.0.15-1.el7.x86_64.rpm
```
```
# systemctl start mysqld
```
```
# vi /var/log/mysqld.log 
> (비밀번호 확인 generated for root@localhost:)
```
```
# mysql -uroot -p
Enter password:(비밀번호 입력)
```
```
mysql> ALTER USER 'root'@'localhost' IDENTIFIED BY '비밀번호 입력'; (비밀번호 변경 : 대문자,특수문자,영어,숫자)
```

## Apache 설치
#
```
필수 설치
# yum install gcc-c++
# yum install expat-devel
```
pcre 다운로드 링크 = https://www.pcre.org/
```
# mkdir -p /usr/local/src/apache
```
```
# cd /usr/local/src/apache
```
```
# wget https://ftp.pcre.org/pub/pcre/pcre-8.43.tar.gz
```
```
# tar -xvzf pcre-8.43.tar.gz
```
```
# cd pcre-8.43
```
```
# ./configure
```
```
# make

# make install
```

Apache 설치 링크 = http://apache.mirror.cdnetworks.com/

```
# cd ..
```
```
# wget http://apache.mirror.cdnetworks.com/httpd/httpd-2.4.38.tar.gz 
# wget http://apache.mirror.cdnetworks.com/apr/apr-1.6.5.tar.gz
# wget http://apache.mirror.cdnetworks.com/apr/apr-util-1.6.1.tar.gz
```
```
# tar -xvzf httpd-2.4.38.tar.gz
# tar -xvzf apr-1.6.5.tar.gz
# tar -xvzf apr-util-1.6.1.tar.gz
```
```
# mv apr-1.6.5 httpd-2.4.38/srclib/apr
# mv apr-util-1.6.1 httpd-2.4.38/srclib/apr-util
```
```
# cd httpd-2.4.38 
```
```
# ./configure --prefix=/usr/local/apache --enable-so --enable-ssl=shared --with-ssl=/usr/local/ssl --enable-rewrite
```
```
# make
# make install
```
```
# vi /usr/lib/systemd/system/httpd.service (새로운 파일 작성)

[Unit]

Description=The Apache HTTP Server

 

[Service]

Type=forking

PIDFile=/usr/local/apache/logs/httpd.pid

ExecStart=/usr/local/apache/bin/apachectl start

ExecReload=/usr/local/apache/bin/apachectl graceful

ExecStop=/usr/local/apache/bin/apachectl stop

KillSignal=SIGCONT

PrivateTmp=true

 

[Install]

WantedBy=multi-user.target
```
```
# systemctl httpd start
```

## PHP 설치
#

다운로드 링크 = http://mirror.cogentco.com/pub/php/

```
필수설치
# yum -y install gcc* cpp* compat-gcc* flex* \
libjpeg* libpng* freetype* gd-* ncurses* libtermcap* \
libxml* libjpeg-devel libpng-devel libxml2-devel

# yum -y install curl-devel libpng \
libpng-devel libjpeg libjpeg-devel libwebp \
libwebp-devel libXpm libXpm-devel openssl \
openssl-devel autoconf curl zlib zlib-devel \
freetype freetype-devel gd gd-devel \
libjpeg libjpeg-devel libmcrypt libmcrypt-devel \
libtool-ltdl-devel libzip libzip-devel \
oniguruma-devel cmake gcc-c++ gcc \
libxml2-devel libxml2 libcurl libcurl-devel \
bzip2-devel sqlite-devel gmp gmp-devel
```
```
# cd /usr/local/src
# wget 해당 링크
# tar -xvzf php-7.3.3.tar.gz
```
```
# cd php-7.3.3

# ./configure --prefix=/usr/local/php --with-mysqli --with-openssl=/usr/local/ssl --with-pdo-mysql=mysqlnd --with-apxs2=/usr/local/apache/bin/apxs --with-config-file-path=/usr/local/apache/conf --with-zlib --disable-debug --enable-calendar --enable-ftp --enable-sockets --enable-sysvsem --with-gd --with-jpeg-dir=/usr/lib64
```
```
# make
# make install
```
```
# cp /usr/local/src/php-7.3.3/php.ini-development /usr/local/apache/conf/php.ini
```
```
# vi /usr/local/apache/conf/php.ini

; Default socket name for local MySQL connects.  If empty, uses the built-in
; MySQL defaults.
; http://php.net/mysqli.default-socket
mysqli.default_socket = /var/lib/mysql/mysql.sock   <--- 추가
```
```
# systemctl start httpd
```
```
# vi /usr/local/apache/conf/httpd.conf

.

.

<IfModule dir_module>

    DirectoryIndex index.html index.php index.jsp <--- 추가

</IfModule>

.

.

    AddType application/x-compress .Z

    AddType application/x-gzip .gz .tgz

    AddType application/x-httpd-php .php .html .htm .inc  <--- 추가

    AddType application/x-httpd-php-source .phps  <--- 추가

.
```
```
# vi /usr/local/apache/htdocs/php.php

<?php phpinfo(); ?>
```
```
# systemctl start httpd
```