#centos 上 安装docker-compose
yum install -y docker-compose 
#===================== 从官方镜像构建 ================================
#当前路径下运行安装以及启动
docker-compose up -d


#追加两行跨域
cluster.name: "docker-cluster"
network.host: 0.0.0.0
http.cors.enabled: true
http.cors.allow-origin: "*"

#=====================或者自行构建镜像并修改docker-compose.yml ================================
#构建 自己的镜像
docker build -t zilaike/elasticsearch:7.14.1   ./
#运行以及启动
docker-compose up -d 
#测试是否安装成功
curl -X GET "localhost:9200/_cat/nodes?v=true&pretty"

#=====================安装elasticsearch head插件监控管理 ================================
#拉取镜像 
docker pull mobz/elasticsearch-head:5
#直接运行
docker run -d -p 9100:9100 docker.io/mobz/elasticsearch-head:5