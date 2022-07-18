FROM amazonlinux:2

RUN yum -y update

RUN rm -rf /etc/localtime
RUN cp /usr/share/zoneinfo/Japan /etc/localtime

# Tools
RUN yum install -y \
  sudo \
  procps \
  wget \
  tar \
  unzip \
  yum-utils \
  git \
  vim \
  gettext \
  crontabs

# PHP
RUN amazon-linux-extras install php8.0
RUN yum -y --enablerepo=amzn2extra-php8.0 install \
  php \
  php-cli \
  php-common \
  php-devel \
  php-fpm \
  php-gd \
  php-intl \
  php-json \
  php-json \
  php-mbstring \
  php mcrypt \
  php-mysqlnd \
  php-pdo \
  php-xml

# composer
RUN curl -sS https://getcomposer.org/installer | php \
  && mv composer.phar /usr/local/bin/composer

# Nginx
RUN sudo amazon-linux-extras install nginx1.12 \
  && systemctl enable nginx.service \
  && systemctl enable php-fpm.service

# nvm & node.js
ENV NODE_VERSION v14.16.0
ENV NVM_DIR /root/.nvm

RUN curl -o- https://raw.githubusercontent.com/creationix/nvm/v0.33.8/install.sh | bash \
  && [ -s $NVM_DIR/nvm.sh ] && \. $NVM_DIR/nvm.sh \
  && nvm install $NODE_VERSION \
  && nvm alias default $NODE_VERSION \
  && nvm use default
ENV PATH $NVM_DIR/versions/node/$NODE_VERSION/bin:$PATH

# MySQL
RUN yum install -y https://dev.mysql.com/get/mysql80-community-release-el7-3.noarch.rpm
RUN rpm --import https://repo.mysql.com/RPM-GPG-KEY-mysql-2022
RUN yum -y --enablerepo=mysql80-community install mysql-community-client

# config
RUN mv /etc/nginx/nginx.conf /etc/nginx/nginx.conf.org
RUN mv /etc/php.ini /etc/php.ini.org

COPY conf/www.conf    /etc/php-fpm.d/
COPY conf/conf.d/     /etc/nginx/conf.d/
COPY conf/nginx.conf  /etc/nginx/
COPY conf/php.ini     /etc/

CMD [ "/sbin/init" ]
