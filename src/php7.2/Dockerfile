FROM wordpress:php7.2

# Install Mailhog as the mail server
RUN curl --location --output /usr/local/bin/mhsendmail https://github.com/mailhog/mhsendmail/releases/download/v0.2.0/mhsendmail_linux_amd64 && \
    chmod +x /usr/local/bin/mhsendmail
RUN echo 'sendmail_path="/usr/local/bin/mhsendmail --smtp-addr=mailhog:1025 --from=no-reply@docker.dev"' > /usr/local/etc/php/conf.d/mailhog.ini

# Install Xdebug
ENV XDEBUG_VERSION 2.6.1
ENV XDEBUG_SHA256 dae691d6c052073b886e0c6e1306a707bca9fd18a1e3485384ef6c4aacf1bd77
RUN set -x \
	&& curl -SL "http://www.xdebug.org/files/xdebug-$XDEBUG_VERSION.tgz" -o xdebug.tgz \
	&& echo "$XDEBUG_SHA256 xdebug.tgz" | sha256sum -c - \
	&& mkdir -p /usr/src/xdebug \
	&& tar -xf xdebug.tgz -C /usr/src/xdebug --strip-components=1 \
	&& rm xdebug.* \
	&& cd /usr/src/xdebug \
	&& phpize \
	&& ./configure --enable-xdebug \
	&& make -j"$(nproc)" \
	&& make install \
	&& make clean

# Install extra php extensions
RUN apt-get update && \
	apt-get install -y libcurl4-gnutls-dev && docker-php-ext-configure curl && docker-php-ext-install curl && \
	apt-get install -y zlib1g-dev libicu-dev g++ && docker-php-ext-configure intl && docker-php-ext-install intl && \
	apt-get install -y libxml++2.6-dev && docker-php-ext-install xml && \
	docker-php-ext-install json && \
	docker-php-ext-install mbstring && \
	docker-php-ext-install soap

# Install WP-CLI
RUN curl -O https://raw.githubusercontent.com/wp-cli/builds/gh-pages/phar/wp-cli.phar && \
	chmod +x wp-cli.phar && \
	mv wp-cli.phar /usr/local/bin/wp && \
	mkdir /var/www/.wp-cli && \
	chown www-data:www-data /var/www/.wp-cli

# Enable SSL
RUN openssl req -subj '/C=US/ST=None/L=None/O=OrgName/OU=IT Department/CN=*.localhost' -x509 -newkey rsa:4096 -nodes -keyout /etc/ssl/private/ssl-cert-snakeoil.key -out /etc/ssl/certs/ssl-cert-snakeoil.pem -days 9000

RUN a2enmod ssl && \
	a2enmod headers && \
	a2ensite default-ssl && \
	service apache2 restart

WORKDIR /var/www/html
VOLUME /var/www/html
