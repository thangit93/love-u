FROM nginx:1.19.6-alpine

COPY ./docker/nginx/api.conf /etc/nginx/conf.d/default.conf
COPY ./api /app

WORKDIR /app
