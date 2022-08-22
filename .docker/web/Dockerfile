FROM php:8.1-apache-bullseye
LABEL maintainer="Sébastien TIMONER - offroadLabs (https://www.linkedin.com/in/timoner/)"

RUN echo "Europe/Paris" > /etc/timezone && dpkg-reconfigure -f noninteractive tzdata

ARG PROJECT_NAME
ENV PROJECT_ROOT_PATH /var/www/${PROJECT_NAME}
ENV PROJECT_PUBLIC_PATH ${PROJECT_ROOT_PATH}/public

RUN apt-get update && apt-get install -y git

COPY 000-default.conf /etc/apache2/sites-available/
RUN echo "ServerName ${PROJECT_NAME}.localhost" >> /etc/apache2/apache2.conf

RUN /usr/sbin/a2enmod rewrite && /usr/sbin/a2enmod headers && /usr/sbin/a2enmod expires
RUN apt-get install -y libzip-dev zip && docker-php-ext-install zip
RUN docker-php-ext-install pdo pdo_mysql mysqli
RUN apt-get install -y libtidy-dev && docker-php-ext-install tidy && docker-php-ext-enable tidy

#composer installation
RUN curl -sS https://getcomposer.org/installer | php -- --install-dir=/usr/local/bin --filename=composer

# xdebug installation
RUN pecl install xdebug && docker-php-ext-enable xdebug
COPY docker-php-ext-xdebug.ini /usr/local/etc/php/conf.d/

# intl installation
RUN apt-get install -y libicu-dev \
  && docker-php-ext-configure intl \
  && docker-php-ext-install intl \
  && docker-php-ext-enable intl

#opcache installation
RUN docker-php-ext-install opcache \
    && docker-php-ext-enable opcache

# git configuration (symfony-cli requirement)
ARG GIT_EMAIL
ARG GIT_USERNAME
RUN git config --global user.email "${GIT_EMAIL}" && git config --global user.name "${GIT_USERNAME}"

# symfony-cli installation
RUN curl -1sLf 'https://dl.cloudsmith.io/public/symfony/stable/setup.deb.sh' | bash -- 
RUN apt-get install symfony-cli

WORKDIR /var/www

COPY web-foreground /usr/local/bin/
RUN chmod +x /usr/local/bin/web-foreground

CMD ["web-foreground"]