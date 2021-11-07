# 获取镜像

docker pull redis:5.0.13

# 构建 自己的镜像

#docker build -t zilaike/redis:5.0.13 ./

# 运行

# docker run -d -p 6379:6379 -v <data-dir>:/data --name redis dockerfile/redis redis-server /etc/redis/redis.conf --requirepass <password>

#docker run -itd --restart="always" -p 6379:6379 -v /usr/local/docker/docker_redis/data:/data -v /usr/local/redis-5.0.2/:
#/etc/redis/ --name redis_official redis:5.0.13 redis-server /etc/redis/redis.conf docker run -d -p 6379:6379 -v
#/usr/local/docker/docker_redis/data:/data -v /usr/local/redis-5.0.2/:/etc/redis/ --name redis_official redis:5.0.13
#redis-server /etc/redis/redis.conf

#docker run   -itd   -p 6379:6379     --name=redis     --volume=/usr/local/redis-5.0.2/:/etc/redis/:rw  redis:5.0.13  redis-server   --requirepass 123456  



docker run   -itd   -p 6379:6379     --name=redis     --volume=/usr/local/software/docker/redis/:/etc/redis/:rw  redis:5.0.13  redis-server   --requirepass 123456  



 

