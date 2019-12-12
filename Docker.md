# Docker
Ref: https://github.com/jaywcjlove/docker-tutorial/blob/master/docker/redis.md

## Linux 端口转发：
```bash
iptables -t nat -A PREROUTING  -p tcp --dport 80 -j REDIRECT --to-port 8080
```
## Start docker
```bash
systemctl start docker
```

## 启动image:
```bash
# docker内部端口映射到外部，这样外部可访问， -name选项设置image启动后的名字， 最后是image id
docker run -d -p 0.0.0.0:49161:1521 -p 0.0.0.0:49160:22 --name xxx 17664c0bf87e
```

## 进入container：
```bash
docker exec -it 0460b9bdaf3d /bin/bash
```

## 以Oracle为例
```bash
# 进入container
docker exec -it oracle11g(容器名称) /bin/bash
# 切换到oracle用户
su - oracle
# 连接到server
sqlplus / as sysdba
# 修改密码
alter user 用户名 identified by 密码;
# 解锁用户
ALTER USER bosch_rbcn_51_1101 IDENTIFIED BY abc123_ ACCOUNT UNLOCK;

select username from dba_users;

select name from v$database;

create user bosch_rbcn_235 identified by bosch_rbcn_235;

grant connect, resource to bosch_rbcn_235;
grant dba to bosch_rbcn_235;

drop user bosch_rbcn_235;
```

## Docker 基本命令
```bash
docker ps -a
docker start container-id
docker rm container-id
```

## Docker - 实现本地镜像的导出、导入（export、import、save、load）
https://sysadminnightmare.org/docker-save-load-import-export-commit/
```bash
docker commit {container-id} {new image name}
docker save {image-id} -o new_redis_image.tar
docker load < new_redis_image.tar

# 从容器拷贝文件到宿主机
docker cp 容器名：容器中要拷贝的文件名及其路径 要拷贝到宿主机里面对应的路径

# 从宿主机拷贝文件到容器
docker cp 宿主机中要拷贝的文件名及其路径 容器名：要拷贝到容器里面对应的路径

# NOTE: dump文件需要copy到新的container中
```

