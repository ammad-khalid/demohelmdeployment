FROM ubuntu:18.04
ENV TZ=Asia/Karachi
RUN ln -snf /usr/share/zoneinfo/$TZ /etc/localtime && echo $TZ > /etc/timezone
RUN apt-get update && apt-get upgrade -y && apt-get install software-properties-common -y && \
    add-apt-repository ppa:ondrej/php && \
    apt-get update
RUN apt-get update && \
    apt-get install -y \
    vim \
    curl \
    git \
    apache2 \
    php7.0 \
    php7.0-cli \
    libapache2-mod-php7.0 \
    php7.0-gd \
    php7.0-json \
    php7.0-ldap \
    php7.0-mbstring \
    php7.0-mysql \
    php7.0-pgsql \
    php7.0-sqlite3 \
    php7.0-xml \
    php7.0-xsl \
    php7.0-zip \
    php7.0-soap && \
    curl -sS https://getcomposer.org/installer -o setup.php && \
    php setup.php --install-dir=/usr/local/bin --filename=composer && \
    rm setup.php
COPY run.sh /usr/local/bin/
COPY mysql_test.php /opt
RUN chmod 700 /usr/local/bin/run.sh
RUN a2enmod rewrite

EXPOSE 80
ENTRYPOINT ["run.sh"]
