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
COPY oracle/instantclient-basiclite-linux.x64-12.1.0.2.0.zip /tmp/
COPY oracle/instantclient-sdk-linux.x64-12.1.0.2.0.zip /tmp/
COPY oracle/instantclient-sqlplus-linux.x64-12.1.0.2.0.zip /tmp/
RUN unzip /tmp/instantclient-basiclite-linux.x64-12.1.0.2.0.zip -d /usr/local/bin/ && \
unzip /tmp/instantclient-sdk-linux.x64-12.1.0.2.0.zip -d /usr/local/bin/ && \
unzip /tmp/instantclient-sqlplus-linux.x64-12.1.0.2.0.zip -d /usr/local/bin/ && \
ln -s /usr/local/bin/instantclient_12_1/sdk/include/*.h /usr/local/include/ && \
ln -s /usr/local/bin/instantclient_12_1/sqlplus /usr/local/bin/ && \
ln -s /usr/local/bin/instantclient_12_1/*.dylib /usr/local/lib/ && \
ln -s /usr/local/bin/instantclient_12_1/*.dylib.12.1 /usr/local/lib/ && \
ln -s /usr/local/lib/libclntsh.dylib.12.1 /usr/local/lib/libclntsh.dylib && \
# https://stackoverflow.com/questions/38206964/compile-php-oci8-extension-under-alpinelinux
docker-php-ext-configure oci8 --with-oci8=instantclient,/usr/local/lib && \
docker-php-ext-install oci8 --with-oci8=instantclient,/usr/local/bin


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
make \
re2c \
sed \
yaml-dev && \
# Update PECL sources before continuing
pecl channel-update pecl.php.net && \
# Install Composer
printf "Downloading and installing Composer ..." && \
curl -sS https://getcomposer.org/installer | \
php -- --install-dir=/usr/local/bin --filename=composer && \
# chmod +x /usr/local/bin/composer
# the last line inside an if or else block requires a semicolon (;)
# string comparison in "sh" is different than "bash"
if [ $(php -r "echo PHP_MAJOR_VERSION;") = "5" ]; then \
echo "${COLOR_YELLOW}PHP version is 5.6.*: $PHP_VERSION" && \
printf "\n" | pecl install apcu-4.0.11 && \
printf "\n" | pecl install yaml-1.3.1 && \
pecl install xdebug-2.5.5; \
else \
echo "${COLOR_GREEN}PHP version is NOT 5.6.*: $PHP_VERSION" && \
pecl uninstall -r apcu && pecl install apcu && \
pecl uninstall -r yaml && pecl install yaml && \
pecl uninstall -r xdebug && pecl install xdebug; \
fi 