FROM devopsedu/webapp:latest
COPY website /var/www/html
RUN apt update && \
    apt install -y php \
    rm -rf /var/www/html/*
CMD ["/usr/sbin/apachectl", "-D", "FOREGROUND"]
