
#=====================或者自行构建镜像并修改docker-compose.yml ================================
#构建 自己的镜像
docker build -t zilaike-canal/otter-server   ./
#运行以及启动
docker-compose up -d 
#测试是否安装成功
curl -X GET "localhost:9200/_cat/nodes?v=true&pretty"

#=====================安装elasticsearch head插件监控管理 ================================
#拉取镜像 
docker pull zilaike-canal/otter-server
#启动otter
curl -fsSL https://raw.githubusercontent.com/alibaba/otter/master/docker/run.sh | bash 



注意点：

1,建议使用otter工程自带的run.sh脚本，会处理好host参数、端口映射、目录挂载等工作.
2,考虑otter docker自身会存储一些mysql、zookeeper的元数据，默认run.sh脚本会通过目录挂载的方式，将数据文件挂载到当前的data/目录下(包含zkData/mysql两个子目录)，
     所以执行curl之前最好进入到一个自己的工作目录，比如cd otter

3,看到successful之后，就代表otter整体启动成功，就可以访问对应manager链接的地址，默认是 http://${host}:8080/