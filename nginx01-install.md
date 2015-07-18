# GET THE CODE

```
ganiks ➜  nginx-1.8.0  wget http://nginx.org/download/nginx-1.8.0.tar.gz 
ganiks ➜  nginx-1.8.0  tar xzvf nginx-1.8.0.tar.gz
ganiks ➜  nginx-1.8.0  pwd
/home/ganiks/learn/nginx/nginx180_source/nginx_180_Compile/nginx-1.8.0
```
# CONFIGURE

```
ganiks ➜  nginx-1.8.0  ./configure --prefix=~/home/ganiks/learn/nginx/Nginx
checking for OS
 + Linux 3.13.0-43-generic x86_64
checking for C compiler ... found
 + using GNU C compiler
 + gcc version: 4.8.2 (Ubuntu 4.8.2-19ubuntu1) 
checking for gcc -pipe switch ... found
checking for gcc builtin atomic operations ... found
bla bla bla ...
bla bla bla ...
bla bla bla ...
checking for sha1 in system OpenSSL crypto library ... found
checking for zlib library ... found
creating objs/Makefile 

Configuration summary  
  + using system PCRE library
  + OpenSSL library is not used
  + md5: using system crypto library
  + sha1: using system crypto library
  + using system zlib library

  nginx path prefix: "~/learn/nginx/Nginx"
  nginx binary file: "~/learn/nginx/Nginx/sbin/nginx"
  nginx configuration prefix: "~/learn/nginx/Nginx/conf"
  nginx configuration file: "~/learn/nginx/Nginx/conf/nginx.conf"
  nginx pid file: "~/learn/nginx/Nginx/logs/nginx.pid"
  nginx error log file: "~/learn/nginx/Nginx/logs/error.log"
  nginx http access log file: "~/learn/nginx/Nginx/logs/access.log"
  nginx http client request body temporary files: "client_body_temp"
  nginx http proxy temporary files: "proxy_temp"
  nginx http fastcgi temporary files: "fastcgi_temp"
  nginx http uwsgi temporary files: "uwsgi_temp"
  nginx http scgi temporary files: "scgi_temp"
```

# MAKE && MAKE INSTALL

```
ganiks ➜  nginx-1.8.0  make && sudo make install
```
> 这时才发现上面的`--prefix`配置错误， 必须用绝对路径，不能用形如`~`符号

# MAKE CLEAN

```
ganiks ➜  nginx-1.8.0  sudo rm -rf \~
ganiks ➜  nginx-1.8.0  make clean
```
> 卸载重装Nginx的正确姿势

# CONFIGURE && INSTALL AGAIN

```
ganiks ➜  nginx-1.8.0  ./configure --prefix=/home/ganiks/learn/nginx/Nginx

  nginx path prefix: "/home/ganiks/learn/nginx/Nginx"
  nginx binary file: "/home/ganiks/learn/nginx/Nginx/sbin/nginx"
  nginx configuration prefix: "/home/ganiks/learn/nginx/Nginx/conf"
  nginx configuration file: "/home/ganiks/learn/nginx/Nginx/conf/nginx.conf"
  nginx pid file: "/home/ganiks/learn/nginx/Nginx/logs/nginx.pid"
  nginx error log file: "/home/ganiks/learn/nginx/Nginx/logs/error.log"
  nginx http access log file: "/home/ganiks/learn/nginx/Nginx/logs/access.log"
  nginx http client request body temporary files: "client_body_temp"
  nginx http proxy temporary files: "proxy_temp"
  nginx http fastcgi temporary files: "fastcgi_temp"
  nginx http uwsgi temporary files: "uwsgi_temp"
  nginx http scgi temporary files: "scgi_temp"

ganiks ➜  nginx-1.8.0  make; make install

ganiks ➜  nginx  tree Nginx
Nginx
├── conf
│   ├── fastcgi.conf
│   ├── fastcgi.conf.default
│   ├── fastcgi_params
│   ├── fastcgi_params.default
│   ├── koi-utf
│   ├── koi-win
│   ├── mime.types
│   ├── mime.types.default
│   ├── nginx.conf
│   ├── nginx.conf.default
│   ├── scgi_params
│   ├── scgi_params.default
│   ├── uwsgi_params
│   ├── uwsgi_params.default
│   └── win-utf
├── html
│   ├── 50x.html
│   └── index.html
├── logs
└── sbin
    └── nginx

ganiks ➜  Nginx  pwd
/home/ganiks/learn/nginx/Nginx
```

# VERSION

> 查看当前Nginx版本版本信息

```
ganiks ➜  Nginx  ./sbin/nginx -V
nginx version: nginx/1.8.0
built by gcc 4.8.2 (Ubuntu 4.8.2-19ubuntu1)
configure arguments: --prefix=/home/ganiks/learn/nginx/Nginx

ganiks ➜  Nginx  ./sbin/nginx -V 2>&1 > some_file
```
> 这里要重定向才可以输出到文件哦


# START NGINX
```
ganiks ➜  Nginx  ./sbin/nginx -p .
nginx: [emerg] bind() to 0.0.0.0:80 failed (13: Permission denied)
ganiks ➜  Nginx  vim conf/nginx.conf && listen 8765
ganiks ➜  Nginx  ./sbin/nginx -p .  
ganiks ➜  Nginx  sudo ps -ef | grep nginx             
ganiks   21174     1  0 15:32 ?        00:00:00 nginx: master process ./sbin/nginx -p .
ganiks   21175 21174  0 15:32 ?        00:00:00 nginx: worker process
ganiks   21182 20842  0 15:32 pts/4    00:00:00 grep nginx


ganiks ➜  Nginx  ./sbin/nginx -t
nginx: the configuration file /home/ganiks/learn/nginx/Nginx/conf/nginx.conf syntax is ok
nginx: configuration file /home/ganiks/learn/nginx/Nginx/conf/nginx.conf test is successful
ganiks ➜  Nginx  ./sbin/nginx -s reload
ganiks ➜  Nginx  sudo ps -ef | grep nginx
ganiks   21174     1  0 15:32 ?        00:00:00 nginx: master process ./sbin/nginx -p .
ganiks   21290 21174  0 15:35 ?        00:00:00 nginx: worker process
ganiks   21294 20842  0 15:35 pts/4    00:00:00 grep nginx
ganiks ➜  Nginx  ./sbin/nginx -s reload   
ganiks ➜  Nginx  sudo ps -ef | grep nginx 
ganiks   21174     1  0 15:32 ?        00:00:00 nginx: master process ./sbin/nginx -p .
ganiks   21308 21174  0 15:35 ?        00:00:00 nginx: worker process
ganiks   21313 20842  0 15:35 pts/4    00:00:00 grep nginx
```
> nginx reload 之后， worker 进程是重新创建了的
```
ganiks ➜  Nginx  cat logs/nginx.pid
21174
```
> nginx.pid 里面保存的是nginx的主进程号


