
- Создаем VM с помощью Vagrant - Vagrantfile_DZ_19_20
- Устанавливаем Docker на VM с помощью Ansible  - playbook_docker.yml

vagrant@node1:~$ docker ps
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES
vagrant@node1:~$ docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
vagrant@node1:~$ docker --version
Docker version 28.0.4, build b8034c0
vagrant@node1:~$

==========================================
Создаем образ nginx на базе alpine - Dockerfile, nginx.conf, index.html

vagrant@node1:~$ mkdir docker-img-nginx_alpine

vagrant@node1:~/docker-img-nginx_alpine$ nano Dockerfile
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

vagrant@node1:~/docker-img-nginx_alpine$ nano index.html
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Ansible + Docker Example</title>
  </head>
  <body>
    <h1>Hello Docker!</h1>
    <p><em>- Built with Ansible</em></p>
  </body>
</html> 

vagrant@node1:~/docker-img-nginx_alpine$ nano nginx.conf
user  www-data www-data;
worker_processes 1;

events {
        worker_connections 1024;
}

http {
  server {
    listen 80;
    root /var/www/site;
  }
}

vagrant@node1:~/docker-img-nginx_alpine$ docker build -t alpine_nginx .
vagrant@node1:~/docker-img-nginx_alpine$ docker images
REPOSITORY     TAG       IMAGE ID       CREATED          SIZE
alpine_nginx   latest    dc77630d8b2a   27 seconds ago   47.9MB
vagrant@node1:~/docker-img-nginx_alpine$ 

=====================================================================
Выполним push образа на Docker hub

vagrant@node1:~/docker-img-nginx_alpine$ docker login 
Login Succeeded

vagrant@node1:~/docker-img-nginx_alpine$ docker tag alpine_nginx rg0255/otus1:alpine_nginx
vagrant@node1:~/docker-img-nginx_alpine$ docker images
REPOSITORY     TAG            IMAGE ID       CREATED         SIZE
alpine_nginx   latest         dc77630d8b2a   8 minutes ago   47.9MB
rg0255/otus1   alpine_nginx   dc77630d8b2a   8 minutes ago   47.9MB
vagrant@node1:~/docker-img-nginx_alpine$

vagrant@node1:~/docker-img-nginx_alpine$ docker push rg0255/otus1:alpine_nginx
The push refers to repository [docker.io/rg0255/otus1]
f5ef3189932e: Pushed 
e6e766b024d3: Pushed 
397a0186943f: Pushed 
921fac770a9f: Pushed 
c18897d5e3dd: Mounted from library/nginx 
9af9e76ea07f: Mounted from library/nginx 
f1f70b13aacc: Mounted from library/nginx 
252b6db79fae: Mounted from library/nginx 
c9ce8cb4e76a: Mounted from library/nginx 
8f3c313eb124: Mounted from library/nginx 
c1761f3c364a: Mounted from library/nginx 
08000c18d16d: Mounted from library/nginx 
alpine_nginx: digest: sha256:d5178f226eeedc0c9a2104683eaa12911bce7d5f235b6c431670ea5e6f4b1761 size: 2817
vagrant@node1:~/docker-img-nginx_alpine$

==============================================================================================
Удалим локальные образы

vagrant@node1:~/docker-img-nginx_alpine$ docker images
REPOSITORY     TAG            IMAGE ID       CREATED          SIZE
alpine_nginx   latest         dc77630d8b2a   12 minutes ago   47.9MB
rg0255/otus1   alpine_nginx   dc77630d8b2a   12 minutes ago   47.9MB
vagrant@node1:~/docker-img-nginx_alpine$ 

vagrant@node1:~/docker-img-nginx_alpine$ docker rmi alpine_nginx
vagrant@node1:~/docker-img-nginx_alpine$ docker rmi rg0255/otus1:alpine_nginx

vagrant@node1:~/docker-img-nginx_alpine$ docker images
REPOSITORY   TAG       IMAGE ID   CREATED   SIZE
vagrant@node1:~/docker-img-nginx_alpine$

===============================================================================================
Выполним pull образа с Docker hub

vagrant@node1:~/docker-img-nginx_alpine$ docker pull rg0255/otus1:alpine_nginx
alpine_nginx: Pulling from rg0255/otus1
f18232174bc9: Already exists 
ccc35e35d420: Already exists 
43f2ec460bdf: Already exists 
984583bcf083: Already exists 
8d27c072a58f: Already exists 
ab3286a73463: Already exists 
6d79cc6084d4: Already exists 
0c7e4c092ab7: Already exists 
e3f6a33a8e5f: Already exists 
a2457c2b804e: Already exists 
78f5c147920e: Already exists 
b15637b90e79: Already exists 
Digest: sha256:d5178f226eeedc0c9a2104683eaa12911bce7d5f235b6c431670ea5e6f4b1761
Status: Downloaded newer image for rg0255/otus1:alpine_nginx
docker.io/rg0255/otus1:alpine_nginx
vagrant@node1:~/docker-img-nginx_alpine$ 
vagrant@node1:~/docker-img-nginx_alpine$ 
vagrant@node1:~/docker-img-nginx_alpine$ 
vagrant@node1:~/docker-img-nginx_alpine$ docker images
REPOSITORY     TAG            IMAGE ID       CREATED          SIZE
rg0255/otus1   alpine_nginx   dc77630d8b2a   16 minutes ago   47.9MB
vagrant@node1:~/docker-img-nginx_alpine$

==========================================================
Выполнем запуск Docker

vagrant@node1:~/docker-img-nginx_alpine$ docker run -dt --name alpine_nginx -p 80:80 rg0255/otus1:alpine_nginx
924337a15a6f77a27ae29123992591a60d219d6d7c8c6b0f33eb94d0436b5e8a
vagrant@node1:~/docker-img-nginx_alpine$ 
vagrant@node1:~/docker-img-nginx_alpine$ docker ps
CONTAINER ID   IMAGE                       COMMAND                  CREATED          STATUS         PORTS                                          NAMES
924337a15a6f   rg0255/otus1:alpine_nginx   "nginx -g 'daemon of…"   10 seconds ago   Up 9 seconds   0.0.0.0:80->80/tcp, [::]:80->80/tcp, 443/tcp   alpine_nginx
vagrant@node1:~/docker-img-nginx_alpine$

vagrant@node1:~/docker-img-nginx_alpine$ sudo netstat -plntu
Active Internet connections (only servers)
Proto Recv-Q Send-Q Local Address           Foreign Address         State       PID/Program name    
tcp        0      0 0.0.0.0:80              0.0.0.0:*               LISTEN      24035/docker-proxy  
tcp        0      0 127.0.0.53:53           0.0.0.0:*               LISTEN      604/systemd-resolve 
tcp        0      0 0.0.0.0:22              0.0.0.0:*               LISTEN      873/sshd: /usr/sbin 
tcp6       0      0 :::80                   :::*                    LISTEN      24040/docker-proxy  
tcp6       0      0 :::22                   :::*                    LISTEN      873/sshd: /usr/sbin 
udp        0      0 127.0.0.53:53           0.0.0.0:*                           604/systemd-resolve 
udp        0      0 10.0.2.15:68            0.0.0.0:*                           1823/systemd-networ 
vagrant@node1:~/docker-img-nginx_alpine$

vagrant@node1:~/docker-img-nginx_alpine$ docker network ls
NETWORK ID     NAME      DRIVER    SCOPE
7a75ffad1338   bridge    bridge    local
f7fe51abc4c3   host      host      local
ba2e8ba3cb9c   none      null      local
vagrant@node1:~/docker-img-nginx_alpine$

vagrant@node1:~/docker-img-nginx_alpine$ curl localhost:80
<!DOCTYPE html>
<html lang="en">
  <head>
    <meta charset="utf-8">
    <title>Ansible + Docker Example</title>
  </head>
  <body>
    <h1>Hello Docker!</h1>
    <p><em>- Built with Ansible</em></p>
  </body>
</html> 
vagrant@node1:~/docker-img-nginx_alpine$

=============================================================================    
 2.	Определена разница между контейнером и образом, написан вывод.
 
Разница между контейнером и образом в контексте Docker заключается в их функциях и характеристиках.

Образ — это неизменяемый шаблон, который содержит всё необходимое для запуска приложения.
Он представляет собой статичный снимок системы в определённый момент времени.
Образ можно скопировать, распространить, но нельзя напрямую модифицировать. Любое изменение требует создания нового образа.

Контейнер — это запущенный экземпляр образа, который представляет собой динамическую, изменяемую среду.
К образу, который выступает основой, добавляется слой, доступный для записи.
Это позволяет изменять настройки, устанавливать новые приложения, настраивать систему под конкретные задачи, не затрагивая при этом исходный образ.

Таким образом, образ — это неизменяемая основа, шаблон, а контейнер — это динамически изменяемая инстанция, созданная на основе образа.
При этом образы и контейнеры тесно связаны: образы могут существовать без контейнеров, а для существования контейнеров необходимо запустить образ.
 
 3.	Написан ответ на вопрос: Можно ли в контейнере собрать ядро?
 
 Собрать ядро возможно, запустить нет.