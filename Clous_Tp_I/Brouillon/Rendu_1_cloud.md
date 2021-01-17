# TP1 - Prise en main de Docker

# 1. Lancer des conteneurs : 

S'ajouter au gorupe docker :
sudo usermod -aG docker vagrant 

[vagrant@centos7 ~]$ docker run alpine
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
801bfaa63ef2: Pull complete
Digest: sha256:3c7497bf0c7af93428242d6176e8f7905f2201d8fc5861f45be7a346b5f23436
Status: Downloaded newer image for alpine:latest

[vagrant@centos7 ~]$ docker container ls
CONTAINER ID   IMAGE     COMMAND   CREATED   STATUS    PORTS     NAMES

[vagrant@centos7 ~]$ docker container ls -a
CONTAINER ID   IMAGE         COMMAND     CREATED          STATUS                      PORTS     NAMES
158b9bad0ee8   alpine        "/bin/sh"   3 minutes ago    Exited (0) 3 minutes ago              determined_lederberg
b27243d658d8   hello-world   "/hello"    11 minutes ago   Exited (0) 11 minutes ago             compassionate_panini


Manipulation du conteneur : 
[vagrant@centos7 ~]$ docker container ls
CONTAINER ID   IMAGE     COMMAND        CREATED         STATUS         PORTS     NAMES
07ee46af03cc   alpine    "sleep 9000"   2 minutes ago   Up 2 minutes             dazzling_sinoussi

[vagrant@centos7 ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND        CREATED         STATUS         PORTS     NAMES
07ee46af03cc   alpine    "sleep 9000"   3 minutes ago   Up 3 minutes             dazzling_sinoussi

[vagrant@centos7 ~]$ docker ps
CONTAINER ID   IMAGE     COMMAND        CREATED         STATUS         PORTS     NAMES
07ee46af03cc   alpine    "sleep 9000"   3 minutes ago   Up 3 minutes             dazzling_sinoussi
[vagrant@centos7 ~]$ docker exec -it 07ee46af03cc sh
/ # echo " coucou"
 coucou

/ # ps -a
PID   USER     TIME  COMMAND
    1 root      0:00 sleep 9000
    8 root      0:00 sh
   19 root      0:00 ps -a

[vagrant@centos7 ~]$ ps -a
  PID TTY          TIME CMD
 2995 pts/1    00:00:00 docker
26269 pts/0    00:00:00 docker
26334 pts/2    00:00:00 ps

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



[root@centos7 vagrant]# docker pull httpd:2.2
2.2: Pulling from library/httpd
f49cf87b52c1: Downloading [============================>                      ]  30.01MB/52.6MB
24b1e09cbcb7: Download complete
8a4e0d64e915: Download complete
bcbe0eb4ca51: Download complete
16e370c15d38: Download complete

vagrant@centos7 ~]$ mkdir http
[vagrant@centos7 ~]$ docker build -t alpython .
Sending build context to Docker daemon  10.24kB
Step 1/6 : FROM alpine:3.7
 ---> 6d1ef012b567
Step 2/6 : RUN apk add --no-cache python3
 ---> Using cache
 ---> 45487bbb8c4a
Step 3/6 : EXPOSE  8888
 ---> Using cache
 ---> 3b03a8eb2449
Step 4/6 : WORKDIR /web/
 ---> Using cache
 ---> 6178ee6cac80
Step 5/6 : COPY ./http/ /web/
 ---> 5084d1093f4a
Step 6/6 : CMD python -m http.server 8888
 ---> Running in 7a7f8475f6ae
Removing intermediate container 7a7f8475f6ae
 ---> 32035a22b882
Successfully built 32035a22b882
Successfully tagged alpython:latest
[vagrant@centos7 ~]$

docker run -p 80:8888 alpythonw



[vagrant@centos7 ~]$ docker run -v http:/web/ alpython

CONTAINER ID   IMAGE      COMMAND                  CREATED          STATUS          PORTS                  NAMES
0fc955d6e631   alpython   "/bin/sh -c 'python3…"   23 seconds ago   Up 22 seconds   8888/tcp               vigilant_rosalind
02c041785b16   alpython   "/bin/sh -c 'python3…"   12 minutes ago   Up 12 minutes   0.0.0.0:80->8888/tcp   youthful_newton