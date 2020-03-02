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

location 匹配规则
https://juejin.im/post/5e3fd6e1e51d452729093ba3?utm_source=gold_browser_extension

= 表示精准匹配（完全相等时，才会命中规则）。
~ 表示区分大小写的正则匹配。
~* 表示不区分大小写的正则匹配。
^~ 表示最佳匹配。
空，匹配以 url 开头的字符串，只能是普通字符串。

匹配过程：
1、Nginx 首先根据 url 检查最长匹配前缀字符串，即会判断【=】、【^~】、【空】修饰符定义的内容。 
如果匹配到最长匹配前缀字符串。 
如果最长匹配前缀字符串被【=】修饰符匹配，则立即响应。 
如果没有被【=】修饰符匹配，则执行第 2 步判断。 
如果没有匹配到最长匹配前缀字符串，则执行第 3 步判断。 
2、Nginx 继续检查最长匹配前缀字符串，即判断【^~】、【空】修饰符定义的内容。  
如果最长匹配前缀字符串被【^~】修饰符匹配，则立即响应。 
如果被【空】修饰符匹配，则将该匹配保存起来，并执行第 3 步判断。 
3、Nginx 找到 nginx.conf 中定义的所有正则匹配（~ 和 ~*），并按顺序进行匹配。 
如果有任何正则表达式匹配成功，则立即响应。 
如果没有任何正则匹配成功，则响应第 2 步中存储的【空】匹配。 