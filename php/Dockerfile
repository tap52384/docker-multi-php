# Single Dockerfile for multiple PHP versions
ARG PHP_VERSION=""

# Sets the base image; if no version is specified, then the latest available is
# used.
# https://docs.docker.com/engine/reference/builder/#environment-replacement
# ${variable:+word} indicates that if variable is set then word will be the
# result, otherwise the result is the empty string
FROM php:${PHP_VERSION:+${PHP_VERSION}-}fpm-alpine

RUN apk update; \
    apk upgrade;