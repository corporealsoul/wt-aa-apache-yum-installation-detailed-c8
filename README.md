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

<br>

### Setting Up Virtual Hosts,

### Creating a project directory,

`[anup@apache-server ~]$ sudo mkdir -p /var/www/anuniqstvsite.com/html`


### Create a log directory for the site,

`[anup@apache-server ~]$ sudo mkdir -p /var/www/anuniqstvsite.com/log`

`[anup@apache-server ~]$ ls -ltr /var/www/anuniqstvsite.com/`


### Changing Ownership and Permission,

`[anup@apache-server ~]$ sudo chown -R $USER:$USER /var/www/anuniqstvsite.com/html`

`[anup@apache-server ~]$ sudo chmod -R 755 /var/www`

`[anup@apache-server ~]$ sudo nano /var/www/anuniqstvsite.com/html/index.html`

    <html>
        <head>
            <title> HTTPD Server</title>
        </head>
        <body>
            <h1>Hey ! from <em> anuniqsTV</em>. </h1>
    <p>Works fine !</p>
        </body>
    </html>


### Create sites-available and sites-enabled directory,

`[anup@apache-server ~]$ ls -ltr /etc/httpd/`

`[anup@apache-server ~]$ sudo mkdir /etc/httpd/sites-available /etc/httpd/sites-enabled`

`[anup@apache-server ~]$ ls -ltr /etc/httpd/`


### Edit Apache’s main configuration file, for additional configuration files,

`[anup@apache-server ~]$ sudo nano /etc/httpd/conf/httpd.conf`

    IncludeOptional conf.d/*.conf
    IncludeOptional sites-enabled/*.conf

`[anup@apache-server ~]$ sudo nano /etc/httpd/conf/httpd.conf`

    ServerName 192.168.56.100

`[anup@apache-server ~]$ apachectl configtest`


### Create a configuration file,

`[anup@apache-server ~]$ sudo nano /etc/httpd/sites-available/anuniqstvsite.com.conf`


    <VirtualHost *:80>
        ServerName 192.168.56.100
        ServerAlias 192.168.56.100
        DocumentRoot /var/www/anuniqstvsite.com/html
        ErrorLog /var/www/anuniqstvsite.com/log/error.log
        CustomLog /var/www/anuniqstvsite.com/log/requests.log combined
    </VirtualHost>

`[anup@apache-server ~]$ ls -ltr /etc/httpd/sites-available/`

`[anup@apache-server ~]$ apachectl configtest`


### Create a SymLink for each virtual host in the site-enabled,

`[anup@apache-server ~]$ sudo ln -s /etc/httpd/sites-available/anuniqstvsite.com.conf /etc/httpd/sites-enabled/anuniqstvsite.com.conf`

`[anup@apache-server ~]$ ls -ltr /etc/httpd/sites-enabled/`

`[anup@apache-server ~]$ apachectl configtest`


### SELinux,

#### Adjusting Apache Policies Universally,

`[anup@apache-server ~]$ sestatus`

`[anup@apache-server ~]$ ls -ltr /etc/selinux`

`[anup@apache-server ~]$ sudo semanage boolean -l | less`

`[anup@apache-server ~]$ sudo semanage boolean -l | grep -i "httpd"`

`[anup@apache-server ~]$ sudo semanage boolean -l | grep -i "httpd_unified"`


### Apply,

`[anup@apache-server ~]$ sudo getsebool httpd_unified`

`[anup@apache-server ~]$ sudo setsebool -P httpd_unified 1`

`[anup@apache-server ~]$ sudo getsebool httpd_unified`

`[anup@apache-server ~]$ sudo semanage boolean -l | grep -i "httpd_unified"`


### Port,

`[root@apache-server ~]# sestatus`

`[root@apache-server ~]# sudo semanage port -l`

`[root@apache-server ~]# sudo semanage port -l | grep http_port_t`


### If not defined,

`[root@apache-server ~]# sudo semanage port -a -t http_port_t -p tcp 80`


### Adjusting Apache Policies on a Directory,

`[anup@apache-server ~]$ sudo ls -dlZ /var/www/anuniqstvsite.com/log/`

    drwxr-xr-x. 2 root root unconfined_u:object_r:httpd_sys_content_t:s0 43 Jan 22 05:51 /var/www/anuniqstvsite.com/log/


### Change current context type from httpd_sys_content_t to httpd_log_t,

`[anup@apache-server ~]$ sudo semanage fcontext -a -t httpd_log_t "/var/www/anuniqstvsite.com/log(/.*)?"`


### Apply changes,

`[anup@apache-server ~]$ sudo restorecon -R -v /var/www/anuniqstvsite.com/log`

`[anup@apache-server ~]$ sudo ls -dlZ /var/www/anuniqstvsite.com/log/`

    drwxr-xr-x. 2 root root unconfined_u:object_r:httpd_log_t:s0 43 Jan 22 05:51 /var/www/anuniqstvsite.com/log/


### Restart Apache,

`[anup@apache-server ~]$ sudo systemctl restart httpd.service`

`[anup@apache-server ~]$ sudo systemctl status httpd.service`


### Then ,

**Open your favorite browser :** http://192.168.56.100/

`[anup@apache-server ~]$ curl -Is http://192.168.56.100/ | head -1`

<br>
