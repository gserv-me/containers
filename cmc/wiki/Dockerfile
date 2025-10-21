FROM mediawiki:stable AS dependencies-step

# Add dependencies.
RUN apt update && apt install -y libzip-dev unzip wget

# Add Font Awesome

RUN wget https://use.fontawesome.com/releases/v7.1.0/fontawesome-free-7.1.0-web.zip -v -O ./fa.zip && \
	mkdir ./fa && \
    unzip ./fa.zip -d ./fa && \
    rm ./fa.zip

RUN bash -c "cp -r ./fa/fontawesome-*/* ./fa/" && \
    bash -c "rm -rf ./fa/fontawesome-*/"

# Add PHP extensions.
# RUN docker-php-source extract && \
#   docker-php-ext-install -j$(nproc) zip && \
#   docker-php-source delete

# Configure extensions.
# docker-php-ext-configure gd --with-freetype-dir=/usr/include/ --with-jpeg-dir=/usr/include/ && \

# Install from PECL.
# pecl install xdebug-2.5.0 && \
# docker-php-ext-enable xdebug && \

# Install Composer.
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer
RUN cp composer.local.json-sample composer.local.json

FROM dependencies-step AS extensions-step

# Require MediaWiki extensions using Composer.
RUN COMPOSER=composer.local.json composer require --no-update mediawiki/sub-page-list:~3.0

# Install any extensions and package updates.
RUN composer update --no-dev -o

# Install MediaWiki extensions using Git.
# You can't store the computed version output between runs, so each layer will need to run set itself.

RUN git clone --depth 1 -b REL$(echo $MEDIAWIKI_MAJOR_VERSION | sed 's/\./_/') \
    https://gerrit.wikimedia.org/r/mediawiki/extensions/CommonsMetadata \
    /var/www/html/extensions/CommonsMetadata

RUN git clone --depth 1 -b REL$(echo $MEDIAWIKI_MAJOR_VERSION | sed 's/\./_/') \
    https://gerrit.wikimedia.org/r/mediawiki/extensions/ApprovedRevs \
    /var/www/html/extensions/ApprovedRevs

# Same deal for skins, just a different directory.
# Some extensions don't have a branch for the current MediaWiki version.

RUN git clone --depth 1 -b v3.9.0 \
    https://github.com/StarCitizenTools/mediawiki-skins-Citizen/ \
    /var/www/html/skins/Citizen
