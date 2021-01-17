# TP1 Prise en main de Docker

## I. Prise en main

### 1. Lancer des conteneurs

* Liste des conteneurs actif : 

```
[vagrant@centos7 ~]$ docker container ls -a
CONTAINER ID   IMAGE         COMMAND     CREATED          STATUS                      PORTS     NAMES
158b9bad0ee8   alpine        "/bin/sh"   3 minutes ago    Exited (0) 3 minutes ago              determined_lederberg
b27243d658d8   hello-world   "/hello"    11 minutes ago   Exited (0) 11 minutes ago             compassionate_panini

[vagrant@centos7 ~]$ docker container ls
CONTAINER ID   IMAGE     COMMAND        CREATED         STATUS         PORTS     NAMES
07ee46af03cc   alpine    "sleep 9000"   2 minutes ago   Up 2 minutes             dazzling_sinoussi
```

Sous le name du conteneur il est bien précisé que le conteneur sleep. 

Son : 
    nom : dazzling_sinoussi
    Id : 07ee46af03cc
    Status : Up 2 minutes

Pour la mise en évidence voici les infos de ma machine centos : 

* Quand on fait regarde les processus : 

```
[vagrant@centos7 ~]$ ps -a
  PID TTY          TIME CMD
 2995 pts/1    00:00:00 docker
26269 pts/0    00:00:00 docker
26334 pts/2    00:00:00 ps
```

* Quand on regarde les Ip : 

```
 [vagrant@centos7 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:19:b4:38 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic eth0
       valid_lft 83309sec preferred_lft 83309sec
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:67:70:0c:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
11: vethf3723f9@if10: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
    link/ether 9e:c1:c3:08:ec:57 brd ff:ff:ff:ff:ff:ff link-netnsid 0
```

* Quand on regarde les utilisateurs : 

```
  [vagrant@centos7 ~]$ cat /etc/passwd | awk -F: '{print $ 1}'
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
operator
games
ftp
nobody
systemd-network
dbus
polkitd
sshd
postfix
chrony
vagrant
vboxadd
```

Maintenant du côté du container : 

* Quand on fait regarde les processus : 
```
/ # ps -a
PID   USER     TIME  COMMAND
    1 root      0:00 sleep 9000
    8 root      0:00 sh
   19 root      0:00 ps -a
```


* Quand on regarde les Ip : 
```
/ # ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
10: eth0@if11: <BROADCAST,MULTICAST,UP,LOWER_UP,M-DOWN> mtu 1500 qdisc noqueue state UP
    link/ether 02:42:ac:11:00:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.2/16 brd 172.17.255.255 scope global eth0
       valid_lft forever preferred_lft forever

       [vagrant@centos7 ~]$ ip a
1: lo: <LOOPBACK,UP,LOWER_UP> mtu 65536 qdisc noqueue state UNKNOWN group default qlen 1000
    link/loopback 00:00:00:00:00:00 brd 00:00:00:00:00:00
    inet 127.0.0.1/8 scope host lo
       valid_lft forever preferred_lft forever
2: eth0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc pfifo_fast state UP group default qlen 1000
    link/ether 08:00:27:19:b4:38 brd ff:ff:ff:ff:ff:ff
    inet 10.0.2.15/24 brd 10.0.2.255 scope global noprefixroute dynamic eth0
       valid_lft 83309sec preferred_lft 83309sec
3: docker0: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue state UP group default
    link/ether 02:42:67:70:0c:02 brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
11: vethf3723f9@if10: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc noqueue master docker0 state UP group default
    link/ether 9e:c1:c3:08:ec:57 brd ff:ff:ff:ff:ff:ff link-netnsid 0

```

* Quand on regarde les utilisateurs : 
```
/ # cat /etc/passwd | awk -F: '{print $ 1}'
root
bin
daemon
adm
lp
sync
shutdown
halt
mail
news
uucp
operator
man
postmaster
cron
ftp
sshd
at
squid
xfs
games
cyrus
vpopmail
ntp
smmsp
guest
nobody

```
On voit biens que tout est radicalement différent.
Maintenant apres avoir fait tout ça, détruisons tout.

```
[vagrant@centos7 ~]$ docker rm 07ee46af03cc
07ee46af03cc
[vagrant@centos7 ~]$ ps -a
  PID TTY          TIME CMD
26424 pts/0    00:00:00 ps
[vagrant@centos7 ~]$ docker ps -a
CONTAINER ID   IMAGE         COMMAND       CREATED          STATUS                      PORTS     NAMES
bbbafdc57579   alpine        "sleep 120"   23 minutes ago   Exited (0) 21 minutes ago             eager_brown
158b9bad0ee8   alpine        "/bin/sh"     30 minutes ago   Exited (0) 30 minutes ago             determined_lederberg
b27243d658d8   hello-world   "/hello"      38 minutes ago   Exited (0) 38 minutes ago             compassionate_panini

```

* Maintenant lancant un conteneur ngnix :
```
[vagrant@centos7 ~]$ docker run -d nginx -p 80
Unable to find image 'nginx:latest' locally
latest: Pulling from library/nginx
a076a628af6f: Pull complete
0732ab25fa22: Pull complete
d7f36f6fe38f: Pull complete
f72584a26f32: Pull complete
7125e4df9063: Pull complete
Digest: sha256:10b8cc432d56da8b61b070f4c7d2543a9ed17c2b23010b43af434fd40e2ca4aa
Status: Downloaded newer image for nginx:latest
e43f527e03aad017df2b42328054f500a17e1ca53d2c8745ad028d8bece4a26d
```

### 2. Gestion d'images
* Maintenant récupérons une image apache : 

```
root@centos7 vagrant]# docker pull httpd:2.2
2.2: Pulling from library/httpd
f49cf87b52c1: Downloading [============================>                      ]  30.01MB/52.6MB
24b1e09cbcb7: Download complete
8a4e0d64e915: Download complete
bcbe0eb4ca51: Download complete
16e370c15d38: Download complete
```
* Création de l'image web Python : 

```
FROM alpine:3.7

RUN apk add --no-cache python3

EXPOSE  8888

WORKDIR /web/
COPY ./http/ /web/

CMD python3 -m http.server 8888
```

* Pour le lancé :

```
docker run -p 80:8888 alpython
```

* Résultas : 

(copier image)
```
```

* Le partage de volume : 

```
 docker run -v http:/web/ alpython
```
On voit que ça à bien marché : 

```
[vagrant@centos7 ~]$ docker run -v http:/web/ alpython

CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                  NAMES
0fc955d6e631   alpython   "/bin/sh -c 'python3…"   23 seconds ago   Up 22 seconds   8888/tcp               vigilant_rosalind
02c041785b16   alpython   "/bin/sh -c 'python3…"   12 minutes ago   Up 12 minutes   0.0.0.0:80->8888/tcp   youthful_newton
```
## II. docker-compose

### Write your own
* Maintenant créons le docker-compose : 

```
version: "3.9"
services:
  web:
    build: ../alpython
    ports:
      - "80:8888"
```
