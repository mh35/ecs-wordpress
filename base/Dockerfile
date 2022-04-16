ARG BASE_IMAGE=8.1-apache
FROM php:$BASE_IMAGE
ARG WP_VERSION=5.9.3
RUN apt-get update && apt-get upgrade -y && \
apt-get install -y libbz2-dev libenchant-2-dev libjpeg-dev \
libpng-dev libgd-dev libgmp-dev libc-client-dev libkrb5-dev \
libicu-dev libldap-dev libpspell-dev libsnmp-dev libxml2-dev \
libtidy-dev libxslt1-dev libzip-dev libmagick++-dev && \
rm -rf /var/lib/apt/lists/*
RUN docker-php-ext-configure imap --with-kerberos --with-imap-ssl
RUN docker-php-ext-install bcmath bz2 calendar enchant exif \
gd gettext gmp imap intl ldap mysqli opcache pcntl pdo_mysql \
pspell shmop snmp soap sockets sysvmsg sysvsem sysvshm tidy \
xsl zend_test zip
RUN pecl install imagick && docker-php-ext-enable imagick
RUN curl -o /tmp/wordpress.tar.gz \
https://wordpress.org/wordpress-${WP_VERSION}.tar.gz
WORKDIR /tmp
RUN tar xzf wordpress.tar.gz && mv wordpress/* /var/www/html && \
rmdir wordpress && rm wordpress.tar.gz
WORKDIR /var/www/html
RUN curl -o /usr/local/bin/wp \
https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar
RUN chmod a+x /usr/local/bin/wp
COPY wp-config.php /var/www/html/
COPY .htaccess /var/www/html/
ENV WP_DB_NAME=wordpress
ENV WP_DB_USER=wp
ENV WP_DB_PASSWORD=password
ENV WP_DB_HOST=localhost
ENV WP_AUTH_KEY="-bySyI%dq%S%1GpKHAc%.1!AHgw<Gzy73D5p|e!4E]l%l)_kS6/0%zX.Tycm3~2l"
ENV WP_SECURE_AUTH_KEY="uaCuK7P!fMoJ*%J_gL-^p-Z+1jyFC6VPS&FR=x<,>cBYoU=|*Mcu~T6JDkVMzPUO"
ENV WP_LOGGED_IN_KEY="|.lS``iFouxQru6waxs}>iU^L~n,4:}0I(p:dO+0n#DOr1v9f}+{=^I%O:dGvr2n"
ENV WP_NONCE_KEY="*bewM54wyM(U(Nx<+^|o*4WdZA%#=dy)H%|N_:-CU3ag|E2?u,},xMj%f]STzzbV"
ENV WP_AUTH_SALT=";7<Dz(wz8d]tqkh76#aY4ZZ)hu~_]@VNoE6:[Ai!}cUbQ<YmXSLi1YKhnSn )?)L"
ENV WP_SECURE_AUTH_SALT="L+Ij1A+gbsrxFic,@Ok?<Bl~mn-T(SR?p^C{O#|BKF%SkOjzWlgV~^}mBp9+ty(7"
ENV WP_LOGGED_IN_SALT="=bW75QF&b)TY#543f8BJ-Sa{^ojA@P]Bf7cn%EX;|TUwPJ0<%s?:E{&.*vMPw>&^"
ENV WP_NONCE_SALT="ud/(?e88J.wDFKAFJ^XCd/<~Wl:{o(:wQD8az*!y|FN{G_?!-<w<UwB>CeACGuFQ"
ENV WP_IS_DEBUG="0"
