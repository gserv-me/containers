FROM mediawiki:stable

# Add dependencies.
RUN apt update && apt install -y libzip-dev

# Add PHP extensions.
RUN docker-php-source extract && \
	docker-php-ext-install -j$(nproc) zip && \
	docker-php-source delete

#   => Configure extensions
#   docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \

#   => Install from PECL
#   pecl install xdebug-2.5.0 && \
#	docker-php-ext-enable redis xdebug && \


# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN composer update

# Install MediaWiki extensions using Composer.
RUN cp composer.local.json-sample composer.local.json
RUN COMPOSER=composer.local.json composer require --no-update mediawiki/sub-page-list:~3.0
RUN composer update --no-dev -o

# Install MediaWiki extensions using Git.
# You can't store the computed version output between runs, so each layer will need to run set itself.

RUN git clone --depth 1 -b REL$(echo $MEDIAWIKI_MAJOR_VERSION | sed 's/\./_/') \
    https://gerrit.wikimedia.org/r/mediawiki/extensions/CommonsMetadata \
    /var/www/html/extensions/CommonsMetadata

RUN git clone --depth 1 -b REL$(echo $MEDIAWIKI_MAJOR_VERSION | sed 's/\./_/') \
    https://gerrit.wikimedia.org/r/mediawiki/extensions/ApprovedRevs \
    /var/www/html/extensions/ApprovedRevs

RUN git clone --depth 1 \
    https://github.com/jaydenkieran/mw-discord \
    /var/www/html/extensions/Discord

# Same deal for skins, just a different directory.

RUN git clone --depth 1 -b v3.9.0 \
    https://github.com/StarCitizenTools/mediawiki-skins-Citizen/ \
    /var/www/html/skins/Citizen
