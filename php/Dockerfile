# Single Dockerfile for multiple PHP versions
ARG PHP_VERSION=""

# Sets the base image; if no version is specified, then the latest available is
# used.
# https://docs.docker.com/engine/reference/builder/#environment-replacement
# ${variable:+word} indicates that if variable is set then word will be the
# result, otherwise the result is the empty string
FROM php:${PHP_VERSION:+${PHP_VERSION}-}fpm-alpine



RUN apk update; \
apk upgrade; \
# required for installing PHP packages
apk add --update --no-cache autoconf \
curl \
g++ \
gcc \
make \
sed && \
# Update PECL sources before continuing
pecl channel-update pecl.php.net && \
# the last line inside an if or else block requires a semicolon (;)
# string comparison in "sh" is different than "bash"
if [[ $PHP_VERSION == "5.6"* || $PHP_VERSION == 5.6* || \
$PHP_VERSION == "5.6.38" ]] ; then \
echo "${COLOR_YELLOW}PHP version is 5.6.*: $PHP_VERSION" && \
printf "\n" | pecl install apcu-4.0.11; \
else \
echo "${COLOR_GREEN}PHP version is NOT 5.6.*: $PHP_VERSION" && \
pecl uninstall -r apcu && pecl install apcu && \
pecl uninstall -r yaml && pecl install yaml && \
pecl uninstall -r xdebug && pecl install xdebug; \
fi

# printf "\n" | pecl install yaml-1.3.1 && \
# pecl install xdebug-2.5.5
