# Hadoop Components Setup And Get Start

## OS
Win10 Linux Sub-system(WLS)  
chrzha@chrzha:/$ cat /proc/version  
Linux version 4.4.0-18362-Microsoft (Microsoft@Microsoft.com) (gcc version 5.4.0 (GCC) ) #10000-Microsoft Fri Jun 21 11:23:00 PST 2019

chrzha@chrzha:~$ lsb_release  -a  
No LSB modules are available.  
Distributor ID: Ubuntu  
Description:    Ubuntu 14.04.5 LTS  
Release:        14.04  
Codename:       trusty

## Software with version
http://archive.cloudera.com/cdh5/cdh/5/hadoop-2.6.0-cdh5.9.3.tar.gz  
http://archive.cloudera.com/cdh5/cdh/5/sqoop-1.4.6-cdh5.9.3.tar.gz  
http://archive.cloudera.com/cdh5/cdh/5/hive-1.1.0-cdh5.9.3.tar.gz  
http://archive.cloudera.com/cdh5/cdh/5/hue-3.9.0-cdh5.9.3.tar.gz

## Sources
```bash
chrzha@chrzha:/etc/apt$ more sources.list  
deb http://cz.archive.ubuntu.com/ubuntu trusty main  
deb-src http://archive.ubuntu.com/ubuntu xenial main restricted #Added  by software-properties  
deb http://mirrors.aliyun.com/ubuntu/ xenial main restricted  
deb-src http://mirrors.aliyun.com/ubuntu/ xenial main restricted multi verse universe #Added by software-properties  
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted  
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-updates main restricted multiverse universe #Added by software-properties  
deb http://mirrors.aliyun.com/ubuntu/ xenial universe  
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates universe  
deb http://mirrors.aliyun.com/ubuntu/ xenial multiverse  
deb http://mirrors.aliyun.com/ubuntu/ xenial-updates multiverse  
deb http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse  
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-backports main restricted universe multiverse #Added by software-properties  
deb http://archive.canonical.com/ubuntu xenial partner  
deb-src http://archive.canonical.com/ubuntu xenial partner  
deb http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted  
deb-src http://mirrors.aliyun.com/ubuntu/ xenial-security main restricted multiverse universe #Added by software-properties  
deb http://mirrors.aliyun.com/ubuntu/ xenial-security universe  
deb [arch=amd64] https://download.docker.com/linux/ubuntu trusty stable  
# deb-src [arch=amd64] https://download.docker.com/linux/ubuntu trusty stable  
deb http://mirrors.aliyun.com/ubuntu/ xenial-security multiverse  
```

## Profile
/etc/profile
```bash
export JAVA_HOME=/mnt/c/jdk1.8.0_181  
export JRE_HOME=$JAVA_HOME/jre  
export HADOOP_HOME=/var/cdh/hadoop-2.6.0-cdh5.9.3  
export SQOOP_HOME=/var/cdh/sqoop-1.4.6-cdh5.9.3  
export HIVE_HOME=/var/cdh/hive-1.1.0-cdh5.9.3  
export CLASSPATH=$JAVA_HOME/lib:$JRE_HOME/lib:$CLASSPATH  
export PATH=$JAVA_HOME/bin:$JRE_HOME/bin:$SQOOP_HOME/bin:$HADOOP_HOME/bin:$HIVE_HOME/bin:$PATH  
```
## Hot Linux Command
```bash
#unzip
tar -zxvf xxxx.tar.gz
#change permission
chmod -R 777 [path]
#change owner
chown -R [user]:[user] [path]
#install
apt-get install [software]
#ssh
service ssh [start|stop|status]
```

## Hadoop
core-site.xml  
```xml
<configuration>
    <property>
        <name>hadoop.tmp.dir</name>
        <value>/var/cdh/hadoop-2.6.0-cdh5.9.3/hadoop-tmp/</value>
    </property>
    <property>
        <name>dfs.name.dir</name>
        <value>/var/cdh/hadoop-2.6.0-cdh5.9.3/hadoop-tmp/name</value>
    </property>
    <property>
        <name>fs.default.name</name>
        <value>hdfs://localhost:9000</value>
    </property>
    <property>
        <name>hadoop.proxyuser.hue.hosts</name>
        <value>*</value>
    </property>
    <property>
        <name>hadoop.proxyuser.hue.groups</name>
        <value>*</value>
    </property>
</configuration>
```
hadoop-env.sh  
```xml
export JAVA_HOME=/mnt/c/jdk1.8.0_181
```

### HDFS
hdfs-site.xml  
```xml
<configuration>
    <property>
        <name>dfs.data.dir</name>
        <value>/var/cdh/hadoop-2.6.0-cdh5.9.3/hadoop-tmp/data</value>
    </property>
    <property>
        <name>dfs.webhdfs.enabled</name>
        <value>true</value>
    </property>
    <property>
      <name>httpfs.proxyuser.hue.hosts</name>
      <value>*</value>
    </property>
    <property>
      <name>httpfs.proxyuser.hue.groups</name>
      <value>*</value>
    </property>
</configuration>
```
Commands
```bash
#upload file to hdfs
./hdfs dfs -put /mnt/d/setup-files/ideaIU-2017.1.3.exe /tmp-20191020
#create dir
./hdfs dfs -mkdir /tmp-20191020
```

### MapReduce
mapred-site.xml  
```xml
<configuration>
    <property>
        <name>mapreduce.framework.name</name>
        <value>yarn</value>
    </property>
    <property>
        <name>mapreduce.admin.user.env</name>
        <value>HADOOP_MAPRED_HOME=$HADOOP_COMMON_HOME</value>
    </property>
    <property>
        <name>yarn.app.mapreduce.am.env</name>
        <value>HADOOP_MAPRED_HOME=$HADOOP_COMMON_HOME</value>
    </property>
    <property>
      <name>mapreduce.map.memory.mb</name>
      <value>1024</value>
    </property>
    <property>
      <name>mapreduce.reduce.memory.mb</name>
      <value>1024</value>
    </property>
    <property>
        <name>mapreduce.map.java.opts</name>
        <value>-Xmx1024M</value>
    <property>
        <name>mapreduce.reduce.java.opts</name>
        <value>-Xmx2560M</value>
    </property>
    </property>
</configuration>
```
### YARN
yarn-site.xml  
```xml
<configuration>
    <property>
        <name>yarn.nodemanager.aux-services</name>
        <value>mapreduce_shuffle</value>
    </property>
    <property>
      <name>yarn.nodemanager.vmem-check-enabled</name>
      <value>false</value>
    </property>
    <property>
     <name>yarn.nodemanager.vmem-pmem-ratio</name>
      <value>4</value>
      <description>Ratio between virtual memory to physical memory when setting memory limits for containers</description>
    </property>
</configuration>
```
### Start and run
```bash
#format name node before start
hadoop namenode -format
chrzha@chrzha:/var/cdh/hadoop-2.6.0-cdh5.9.3$ sbin/start-dfs.sh   
chrzha@chrzha:/var/cdh/hadoop-2.6.0-cdh5.9.3$ sbin/start-yarn.sh   
```
Overview: http://127.0.0.1:50070/dfshealth.html#tab-overview

### Issues
ssh login without auth.
see details to config

## Hive
reference:  
[Bilibili video](https://www.bilibili.com/video/av50804138)

hive-site.xml
```xml
<configuration>
<property>
<name>hive.metastore.warehouse.dir</name>
<value>/var/cdh/hive-1.1.0-cdh5.9.3/warehouse</value>
<description>location of default database for the warehouse in HDFS</description>
</property>
<property>
  <name>javax.jdo.option.ConnectionURL</name>
  <value>jdbc:postgresql://127.0.0.1:5432/cdhmetastore?createDatabaseIfNotExist=true</value>
</property>
<property>
  <name>javax.jdo.option.ConnectionDriverName</name>
  <value>org.postgresql.Driver</value>
</property>
<property>
  <name>javax.jdo.option.ConnectionUserName</name>
  <value>postgres</value>
</property>
<property>
  <name>javax.jdo.option.ConnectionPassword</name>
  <value>admin</value>
</property>
<property>
  <name>hive.server2.thrift.port</name>
  <value>10000</value>
</property>
<property>
  <name>hive.server2.thrift.bind.host</name>
  <value>localhost</value>
</property>
<property>
  <name>hive.metastore.uris</name>
  <value>thrift://localhost:9083</value>
</property>
</configuration>
```
hive-en.sh
```bash
export HIVE_CONF_DIR=/var/cdh/hive-1.1.0-cdh5.9.3/conf
```

Note:
Put rdbms driver lib to lib folder, like postgres-42.2.8.jar



## sqoop
reference:   
[Install](https://www.jianshu.com/p/a088713ba26b)

sqoop-env.sh 
```bash
export HADOOP_COMMON_HOME=/var/cdh/hadoop-2.6.0-cdh5.9.3
export HADOOP_MAPRED_HOME=/var/cdh/hadoop-2.6.0-cdh5.9.3
```
Note:
[Error](http://stackoverflow.com/questions/27504508/java-lang-noclassdeffounderror-org-json-jsonobject)  
Put rdbms driver lib to lib folder, like postgres-42.2.8.jar
Put java-json.jar to lib folder

```bash
#list tables
${SQOOP_HOME}\bin:
./sqoop list-databases --connect jdbc:postgresql://127.0.0.1:5432/liferay-portal?characterEncoding=UTF-8 --username postgres --password 'admin'
```
import ref:
https://blog.csdn.net/qq_15103205/article/details/72903894
https://stackoverflow.com/questions/33658439/sqoop-import-fails-due-to-classnotfoundexception
```bash
#import RDBMS data to HDFS
sqoop import -m 1 --connect jdbc:postgresql://127.0.0.1:5432/liferay-portal?characterEncoding=UTF-8 --username postgres --password 'admin' --table user_ --target-dir /tmp-20191020/sqoop/datatest --check-column userid --incremental append
```
#### export
TBD

## Hue
We need build hue before install/start it.
```bash
chrzha@chrzha:/var/cdh/hue-3.9.0-cdh5.9.3$ make apps 
```
Note: Python dev env is required, [More dependencies]()

```bash
#start hue  
chrzha@chrzha:/var/cdh/hue-3.9.0-cdh5.9.3/build/env$ ./bin/supervisor  
```
Integrate with Hive
```bash
#Start hiveserver2  
chrzha@chrzha:/var/cdh/hive-1.1.0-cdh5.9.3/bin$ hiveserver2  
#Start hive metastore service  
chrzha@chrzha:/var/cdh/hive-1.1.0-cdh5.9.3$ bin/hive --service metastore  
```
Web Hue:
http://127.0.0.1:8888  hue/abc123_

## To be done
Flume (File to Hadoop)