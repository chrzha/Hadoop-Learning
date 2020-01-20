# Nginx
## Install:
ref: https://www.tecmint.com/install-nginx-on-centos-7/
add repo: sudo yum install epel-release
install: yum install nginx

## 查看是否安装服务
root@boschib bin]# systemctl | grep httpd
root@boschib bin]# systemctl | grep nginx

## 重启 nginx
start: sudo service nginx restart

## 查看端口占用：
lsof
sudo fuser -k 80/tcp

以下是Nginx的默认路径： 

(1) Nginx配置路径：/etc/nginx/

(2) PID目录：/var/run/nginx.pid

(3) 错误日志：/var/log/nginx/error.log 

(4) 访问日志：/var/log/nginx/access.log 

(5) 默认站点目录：/usr/share/nginx/html 

```bash
--access config--
location / {
    deny  192.168.1.1;
    allow 192.168.1.0/24;
    allow 10.1.1.0/16;
    allow 2001:0db8::/32;
    deny  all;
}
```