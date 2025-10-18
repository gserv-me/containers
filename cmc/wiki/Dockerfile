FROM mediawiki:stable

# Install MediaWiki extensions using Git.
# You can't store the computed version output between runs, so each layer will need to run set itself.

RUN git clone --depth 1 -b REL$(echo $MEDIAWIKI_MAJOR_VERSION | sed 's/\./_/') \
    https://gerrit.wikimedia.org/r/mediawiki/extensions/ConfirmAccount \
    /var/www/html/extensions/ConfirmAccount

RUN git clone --depth 1 -b REL$(echo $MEDIAWIKI_MAJOR_VERSION | sed 's/\./_/') \
    https://gerrit.wikimedia.org/r/mediawiki/extensions/CommonsMetadata \
    /var/www/html/extensions/CommonsMetadata

# To install PHP extensions, see below.
# Steps should all be done in the same layer, so use one RUN command.

#   => Extract PHP source files
# RUN docker-php-source extract && \

#   => Installing core extensions
#   docker-php-ext-install -j$(nproc) iconv mcrypt && \

#   => Configuring extensions
#   docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \

#   => Installing from PECL
#   pecl install xdebug-2.5.0 && \
#	docker-php-ext-enable redis xdebug && \

#   => Delete extracted source files
#	docker-php-source delete
