FROM centos:7

RUN yum -y update && yum clean all

RUN yum reinstall -y glibc-common && yum clean all
RUN yum -y install epel-release
RUN rpm --import http://dl.fedoraproject.org/pub/epel/RPM-GPG-KEY-EPEL-7
RUN rpm --import http://mirror.centos.org/centos/RPM-GPG-KEY-CentOS-7
RUN rpm -Uvh http://rpms.famillecollet.com/enterprise/remi-release-7.rpm

ENV LANG=ja_JP.UTF-8

RUN localedef -f UTF-8 -i ja_JP ja_JP.UTF-8
RUN yum reinstall -y glibc-common && yum clean all

RUN yum install -y --enablerepo=epel httpd httpd-tools openssh-server openssh-clients openssl vim sudo which && yum clean all
RUN yum install -y --enablerepo=remi gd-last && yum clean all
RUN yum install -y --enablerepo=epel libmcrypt supervisor && yum clean all
RUN yum install -y --enablerepo=remi,remi-php72 httpd mod_ssl
RUN yum install -y --enablerepo=remi,remi-php72 php php-devel php-mbstring php-pdo php-gd php-xml php-mcrypt php-mysql php-zip

RUN php -r "copy('https://getcomposer.org/installer', 'composer-setup.php');"
RUN php -r "if (hash_file('SHA384', 'composer-setup.php') === '544e09ee996cdf60ece3804abc52599c22b1f40f4323403c44d44fdfdd586475ca9813a858088ffbc1f233e9b180f061') { echo 'Installer verified'; } else { echo 'Installer corrupt'; unlink('composer-setup.php'); } echo PHP_EOL;"
RUN php composer-setup.php
RUN php -r "unlink('composer-setup.php');"
RUN mv composer.phar /usr/local/bin/composer

ENV COMPOSER_ALLOW_SUPERUSER=1
ENV COMPOSER_HOME=/composer
ENV PATH=/usr/local/sbin:/usr/local/bin:/usr/sbin:/usr/bin:/sbin:/bin:/composer/vendor/bin
RUN composer global require "laravel/installer"

EXPOSE 80
