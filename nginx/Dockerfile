FROM nginx:1.18.0
MAINTAINER whb whbjzzwjxq@gmail.com
EXPOSE 8000 8001 8002
VOLUME /root/volume/nginx/log /var/log/nginx
COPY metaknew.conf /etc/nginx/conf.d/
CMD ["nginx", "daemon off"]