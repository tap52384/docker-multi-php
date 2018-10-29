# Single Dockerfile for multiple PHP versions
ARG PHP_VERSION=""

# Sets the base image; if no version is specified, then the latest available is
# used.
# https://docs.docker.com/engine/reference/builder/#environment-replacement
# ${variable:+word} indicates that if variable is set then word will be the
# result, otherwise the result is the empty string
FROM php:${PHP_VERSION:+${PHP_VERSION}-}fpm-alpine

# Set up the Oracle environment variables
ENV LD_LIBRARY_PATH /usr/lib/oracle/12.1/client64/lib/
ENV ORACLE_HOME /usr/lib/oracle/12.1/client64/lib/

# Install the Oracle InstantClient libraries for Linux x64
# The <src> is relative to the location of this Dockerfile
COPY oracle/12.1/instantclient-basiclite-linux.x64-12.1.0.2.0.zip /tmp/
COPY oracle/12.1/instantclient-sdk-linux.x64-12.1.0.2.0.zip /tmp/
COPY oracle/12.1/instantclient-sqlplus-linux.x64-12.1.0.2.0.zip /tmp/
# Version 18.3
# COPY oracle/18.3/instantclient-basic-linux.x64-18.3.0.0.0dbru.zip /tmp/
# COPY oracle/18.3/instantclient-sdk-linux.x64-18.3.0.0.0dbru.zip /tmp/
# COPY oracle/18.3/instantclient-sqlplus-linux.x64-18.3.0.0.0dbru.zip /tmp/

# To reduce the amount of layers, try to use a single RUN instruction
RUN apk update; \
apk upgrade; \
# required for installing PHP packages
apk add --update --no-cache \
autoconf \
curl \
g++ \
gcc \
libaio \
libnsl \
make \
re2c \
yaml-dev && \
# Install Oracle InstantClient 12.1.0.2 for Linux x64
# https://www.oracle.com/technetwork/topics/linuxx86-64soft-092277.html
mkdir -p ${ORACLE_HOME} && \
unzip /tmp/instantclient-basiclite-linux.x64-12.1.0.2.0.zip -d /usr/local/bin/ && \
unzip /tmp/instantclient-sdk-linux.x64-12.1.0.2.0.zip -d /usr/local/bin/ && \
unzip /tmp/instantclient-sqlplus-linux.x64-12.1.0.2.0.zip -d /usr/local/bin/ && \
# https://stackoverflow.com/questions/38206964/compile-php-oci8-extension-under-alpinelinux
# echo "instantclient,${ORACLE_HOME}" | pecl install -f oci8-2.0.12 && \
# docker-php-ext-configure oci8 --with-oci8="instantclient,${ORACLE_HOME}" && \
# docker-php-ext-install oci8 --with-oci8="instantclient,${ORACLE_HOME}" && \
# Update PECL sources before continuing
pecl channel-update pecl.php.net && \
# Install Composer
printf "Downloading and installing Composer ..." && \
curl -sS https://getcomposer.org/installer | \
php -- --install-dir=/usr/local/bin --filename=composer && \
# the last line inside an if or else block requires a semicolon (;)
# string comparison in "sh" is different than "bash"
if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
echo "${COLOR_YELLOW}PHP version is 5.6.*: $PHP_VERSION" && \
printf "\n" | pecl install apcu-4.0.11 && \
printf "\n" | pecl install yaml-1.3.1 && \
pecl install xdebug-2.5.5; && \
# echo 'instantclient,/usr/lib/oracle/12.1/client64/lib' | pecl install -f oci8-2.0.8; \
else \
echo "${COLOR_GREEN}PHP version is NOT 5.6.*: $PHP_VERSION" && \
pecl uninstall -r apcu && pecl install apcu && \
pecl uninstall -r yaml && pecl install yaml && \
pecl uninstall -r xdebug && pecl install xdebug; \
fi 