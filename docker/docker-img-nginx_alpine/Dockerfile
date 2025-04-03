# Use the official Nginx Alpine image as the base
FROM nginx:alpine

# Copy custom Nginx configuration files into the container
COPY nginx.conf /etc/nginx/nginx.conf

RUN adduser -u 1010 -D -S -G www-data www-data
RUN mkdir -p /var/www/site
ADD index.html /var/www/site/index.html


# Expose NGINX standard http(s) ports
EXPOSE 80/tcp 443/tcp

# Set the entrypoint to run Nginx in the foreground
ENTRYPOINT ["nginx", "-g", "daemon off;"]
