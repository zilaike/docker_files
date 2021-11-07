# 获取镜像

docker pull zookeeper

# 单机
docker run --name zookeeper-standalone -d -p 2181:2181  zookeeper

# 构建 自己的镜像

#docker build -t zilaike/redis:5.0.13 ./

#集群
docker-compose up -d



 

