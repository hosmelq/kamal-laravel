FROM serversideup/php:8.4-fpm-nginx-alpine as base

ENV AUTORUN_ENABLED=true
ENV HEALTHCHECK_PATH="/up"
ENV PHP_OPCACHE_ENABLE=1

FROM node:lts as npm-build

COPY . /usr/src/app

WORKDIR /usr/src/app

RUN npm install && npm run build

FROM base

COPY --chmod=755 ./docker/entrypoint.d/ /etc/entrypoint.d/

COPY --chown=www-data:www-data . $APP_BASE_DIR
COPY --chown=www-data:www-data --from=npm-build /usr/src/app/public/build $APP_BASE_DIR/public/build

RUN composer install --no-dev --no-interaction --optimize-autoloader --prefer-dist
