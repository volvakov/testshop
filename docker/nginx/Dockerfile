FROM nginx:1.21

ARG ENV_TYPE

COPY nginx/nginx_$ENV_TYPE.template /etc/nginx/conf.d/default.template
COPY nginx/ssl/ /etc/nginx/ssl/
COPY nginx/cors /etc/nginx/cors
COPY nginx/run_nginx.sh /src/run_nginx.sh
