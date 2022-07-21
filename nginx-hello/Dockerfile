
# Using the official nginx unpriviledged version so it runs
# as a normal user
# Pinning the version so it doesn't break unexpectedly
# Using alpine to minimize image size
FROM nginxinc/nginx-unprivileged:1.23.0-alpine


# Because we run as non-root, we can't use ports under 1024
# setting a parameter and a default incase we need to change it
# The base image also defaults to 8080
ARG nginx_listener=8080
ENV NGINX_PORT=$nginx_listener

  
# Copy over configs and html
COPY static/index.html /usr/share/nginx/html/index.html


# Using the built-in nginx templates to use env variables
# Much cleaner and robust then sed/awk/etc...
COPY templates/default.conf.template /etc/nginx/templates/default.conf.template


EXPOSE ${NGINX_PORT}
