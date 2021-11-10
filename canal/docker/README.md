
mysql -uroot -proot
# 创建账号
CREATE USER canal IDENTIFIED BY 'canal';
# 授予权限
GRANT SELECT, REPLICATION SLAVE, REPLICATION CLIENT ON *.* TO 'canal'@'%';
#-- GRANT ALL PRIVILEGES ON *.* TO 'canal'@'%' ;
# 刷新并应用
FLUSH PRIVILEGES;

#查看binlog是否开启
show variables like 'log_bin';
#binlog位置
show master status;

# Dockerfile
# Dockerfile文件：https://github.com/alibaba/canal/blob/master/docker/Dockerfile 注意点：

# 基于centos6.7最小镜像进行构建，安装一些必须的工具，比如tar/dstat/nc/man等，大概400MB
# 默认安装jdk 1.8，build.sh脚本里会自动下载jdk然后copy到docker里，大概400MB
# 自带日志清理脚本，会识别硬盘超过80%时，自动清理
# 因此，canal整个docker镜像在900MB左右，有一定的优化空间，比如使用jre、减少一些非必须的命令等

# 获取Docker
# 远程拉取
# 访问docker hub获取最新的版本 访问：https://hub.docker.com/r/canal/canal-server/tags/

# 下载对应的版本，比如最新版为1.1.5

docker pull canal/canal-server:v1.1.5
# 本地编译
git clone git@github.com:alibaba/canal.git
cd docker && sh build.sh
# 启动Docker
docker目录下自带了一个run.sh脚本: https://github.com/alibaba/canal/blob/master/docker/run.sh

#sh run.sh 
#Usage:
#  run.sh [CONFIG]
#example:
#  run.sh -e canal.instance.master.address=127.0.0.1:3306 \
#         -e canal.instance.dbUsername=canal \
#         -e canal.instance.dbPassword=canal \
#         -e canal.instance.connectionCharset=UTF-8 \
#         -e canal.instance.tsdb.enable=true \
#         -e canal.instance.gtidon=false \
#         -e canal.instance.filter.regex=.*\\..* 
#实际运行的例子：

# 下载脚本
wget https://raw.githubusercontent.com/alibaba/canal/master/docker/run.sh 

# 构建一个destination name为test的队列
sh run.sh -e canal.auto.scan=false \
		  -e canal.destinations=test \
		  -e canal.instance.master.address=127.0.0.1:3306  \
		  -e canal.instance.dbUsername=canal  \
		  -e canal.instance.dbPassword=canal  \
		  -e canal.instance.connectionCharset=UTF-8 \
		  -e canal.instance.tsdb.enable=true \
		  -e canal.instance.gtidon=false
# 注意点：

# -e参数里可以指定以前canal.properties/instance.properties里的所有配置的key和value，canal-server启动时会有限读取-e指定的变量，具体可参考AdminGuide
# docker模式下，单docker实例只能运行一个instance，主要为配置问题。如果需要运行多instance时，可以自行制作一份docker镜像即可
 
# 看到successful之后，就代表canal-server启动成功，可以启动canal-client链接上来进行binlog订阅了




#======================================================================================管理端============================================================================
#远程拉取
#访问docker hub获取最新的版本 访问：https://hub.docker.com/r/canal/canal-admin/tags/

#下载对应的版本，比如最新版为1.1.5

docker pull canal/canal-admin:v1.1.5
#本地编译
sh build.sh admin


# docker目录下自带了一个run_admin.sh脚本: https://github.com/alibaba/canal/blob/master/docker/run_admin.sh

# Usage:
# # run_admin.sh [CONFIG]
# example :
# run_admin.sh -e server.port=8089 \
# -e canal.adminUser=admin \
# -e canal.adminPasswd=admin
# 实际运行的例子：

# 下载脚本
wget https://raw.githubusercontent.com/alibaba/canal/master/docker/run_admin.sh

# 以8089端口启动canal-admin
sh  run_admin.sh -e server.port=8089 \
-e canal.adminUser=admin \
-e canal.adminPasswd=admin

# 指定外部的mysql作为admin的库
sh  run_admin.sh -e server.port=8089 \
 -e canal.adminUser=admin \
 -e canal.adminPasswd=6BB4837EB74329105EE4568DDA7DC67ED2CA2AD9 \
 -e spring.datasource.address=192.168.33.2:3306 \
 -e spring.datasource.database=canal_manager \
 -e spring.datasource.username=root \
 -e spring.datasource.password=root
# 注意点：

# -e参数里可以指定以前application.yml里所有配置的key和value，springboot启动时会读取-e指定的变量
# 运行效果


# 看到successful之后，就代表canal-admin启动成功，可以访问 http://127.0.0.1:8089/ ，默认账号密码: admin/123456

# 配套启动Canal-Server Docker
# 首先请参考：Canal-Server的Docker启动方式 Docker-QuickStart

# 下载脚本
wget https://raw.githubusercontent.com/alibaba/canal/master/docker/run.sh

# 以单机模式启动
run.sh -e canal.admin.manager=127.0.0.1:8089 \
-e canal.admin.port=11110 \
-e canal.admin.user=admin \
-e canal.admin.passwd=4ACFE3202A5FF5CF467898FC58AAB1D615029441

# 自动加入test集群
run.sh -e canal.admin.manager=127.0.0.1:8089 \
-e canal.admin.port=11110 \
-e canal.admin.user=admin \
-e canal.admin.passwd=4ACFE3202A5FF5CF467898FC58AAB1D615029441
-e canal.admin.register.cluster=test
# 注意点：

# canal.admin.manager 代表需要链接的canal-admin地址
# canal.admin.user/passwd/port 请参考canal-admin的配置指导文档
# canal.admin.register.cluster 表示默认注册的集群
# 看到successful之后，就代表canal-server启动成功，然后就可以在canal-admin上进行任务分配了