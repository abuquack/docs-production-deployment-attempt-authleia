# nginx production image for docs
FROM nginxinc/nginx-unprivileged:1.26-alpine

# Copy custom nginx configs
COPY ./conf.d /etc/nginx/conf.d

# (Optional) Add SSL certs, static assets, or custom entrypoints here
# COPY ./ssl /etc/nginx/ssl
# COPY ./static /usr/share/nginx/html/static

# Use unprivileged user by default
USER 1000

CMD ["nginx", "-g", "daemon off;"]
