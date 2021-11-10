
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
# 用脚本启动docker-compose
# 启动 start_admin.sh 
# 启动 start_server.sh (先修改数据库)