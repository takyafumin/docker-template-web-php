#FROM httpd:2.4.51-bullseye
FROM amazonlinux:2

ARG APR_VER="1.7.0"
ARG APR_UTIL_VER="1.6.1"
ARG HTTPD_VER="2.4.51"
ARG PHP_VER="7.4.20"


COPY ./resources/httpd-${HTTPD_VER}.tar.gz /usr/local/src
COPY ./resources/apr-${APR_VER}.tar.gz /usr/local/src
COPY ./resources/apr-util-${APR_UTIL_VER}.tar.gz /usr/local/src
COPY ./resources/php-${PHP_VER}.tar.gz /usr/local/src


# install base packages
RUN amazon-linux-extras install -y epel \
 && yum install -y \
    shadow-utils \
    tar gzip gcc libtool make expat-devel pcre-devel libxml2-devel sqlite-devel \
    bzip2-devel libcurl-devel openssl-devel libpng-devel libjpeg-devel gettext \
    oniguruma-devel postgresql-devel readline-devel libsodium-devel libxslt-devel libgd-devel
ENV PATH $PATH:/usr/local/apache2/bin:/usr/bin


# mkdir DocumentRoot
RUN mkdir -p /var/www/app/public


# Add apache user/group
RUN /usr/sbin/groupadd www-data
RUN useradd -g www-data www-data


# install apache
# see https://httpd.apache.org/docs/2.4/ja/install.html
WORKDIR /usr/local/src
RUN tar xfz apr-${APR_VER}.tar.gz \
 && cd /usr/local/src/apr-${APR_VER} \
 && sed -i -e 's/$RM "$cfgfile"/$RM -f "$cfgfile"/' configure \
 && ./configure --prefix=/usr/local/apr && make && make install
RUN tar xfz apr-util-${APR_UTIL_VER}.tar.gz \
 && cd /usr/local/src/apr-util-${APR_UTIL_VER} \
 && ./configure --with-apr=/usr/local/apr --prefix=/usr/local/apr-util && make && make install
RUN tar xfz httpd-${HTTPD_VER}.tar.gz \
 && cd /usr/local/src/httpd-${HTTPD_VER} \
 && ./configure --with-apr=/usr/local/apr --with-apr-util=/usr/local/apr-util && make && make install


# install php
WORKDIR /usr/local/src
RUN tar xfz php-${PHP_VER}.tar.gz \
 && cd /usr/local/src/php-${PHP_VER} \
 && ./configure \
    --with-apxs2 \
    --with-bz2 \
    --enable-calendar \
    --with-curl=/usr \
    --enable-exif \
    --enable-ftp \
    --enable-gd \
    --with-gettext \
    --enable-mbstring \
    --with-openssl \
    --enable-pcntl \
    --with-pdo-pgsql \
    --with-pgsql \
    --disable-posix \
    --with-readline \
    --enable-sockets \
    --with-sodium \
    --with-xsl \
    --with-zlib \
 && make && make install


# php composer
COPY --from=composer /usr/bin/composer /usr/bin/composer


# copy resources
COPY ./files/usr/local/bin/docker-php-entrypoint /usr/local/bin/docker-php-entrypoint
COPY ./files/usr/local/bin/httpd-foreground /usr/local/bin/httpd-foreground
COPY ./files/usr/local/apache2/conf/httpd.conf /usr/local/apache2/conf/httpd.conf
COPY ./files/usr/local/apache2/conf/extra/my-app.conf /usr/local/apache2/conf/extra/my-app.conf
COPY ./files/usr/local/lib/php.ini /usr/local/lib/php.ini


EXPOSE 80
WORKDIR /var/www/app/public
ENTRYPOINT [ "docker-php-entrypoint" ]
CMD ["httpd-foreground"]
