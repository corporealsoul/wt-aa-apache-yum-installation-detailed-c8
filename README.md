### Minimum Requirements,

1. Linux kernel version 2.6 or higher, glibc2 version 2.5 or higher

2. 256 Mbytes RAM (512 MB recommended)

3. 400 Mbytes available disk space


### Installation,

`[anup@apache-server ~]$ yum --showduplicates list httpd`

`[anup@apache-server ~]$ yum whatprovides httpd`

`[anup@apache-server ~]$ sudo yum install httpd`

`[anup@apache-server ~]$ rpm -qi httpd`


### Version,

`[anup@apache-server ~]$ httpd -v`


### Service Control,

`[anup@apache-server ~]$ sudo systemctl status httpd.service`

`[anup@apache-server ~]$ sudo systemctl enable httpd.service`

`[anup@apache-server ~]$ sudo systemctl start httpd.service`


### Logs,

`[anup@apache-server ~]$ sudo ls -ltr /var/log/httpd/`

`[anup@apache-server ~]$ ls -lZ /var/www/anuniqstvsite.com/log`

<br>

`[anup@apache-server ~]$ sudo lsof -i -P -n | grep LISTEN`

`[anup@apache-server ~]$ sudo netstat -tulpn | grep LISTEN`

<br>

`[anup@nginxwebhost ~]$ ps -aux | grep httpd`


### Directory,

`[anup@apache-server ~]$ ls -ltr /var/www/`


### Firewall,

`[anup@apache-server ~]$ sudo firewall-cmd --list-all`

`[anup@apache-server ~]$ sudo firewall-cmd --zone=public --add-service=http --permanent`

`[anup@apache-server ~]$ sudo firewall-cmd --zone=public --add-service=https --permanent`

`[anup@apache-server ~]$ sudo firewall-cmd --reload`

`[anup@apache-server ~]$ sudo firewall-cmd --list-all`

<br>

`[anup@apache-server ~]$ sudo firewall-cmd --zone=public --add-port=80/tcp --permanent`

`[anup@apache-server ~]$ sudo firewall-cmd --zone=public --add-port=443/tcp --permanent`

`[anup@apache-server ~]$ sudo firewall-cmd --reload`

`[anup@apache-server ~]$ sudo firewall-cmd --list-all`


### Configuration,

`[anup@apache-server ~]$ ls -ltr /etc/httpd/conf`

`[anup@apache-server ~]$ cat /etc/httpd/conf/httpd.conf`

`[anup@apache-server ~]$ ls -ltr /etc/httpd/conf.d/`


### Verify and Access,

**Open your favorite browser :** http://192.168.56.100/


### Load,

`[anup@apache-server ~]$ htop`

<br>
