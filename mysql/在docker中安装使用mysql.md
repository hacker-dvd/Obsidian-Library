
1. 将用户添加到docker组中，以避免每次运行Docker命令时都需要输入sudo: `sudo usermod -aG docker $USER`
2. 拉取指定版本的 mysql：`sudo docker pull mysql`
3. 查看镜像：`docker images`
4. 启动一个容器实例：`docker run --name lc_mysql -e MYSQL_ROOT_PASSWORD=123456 -d mysql`
5. 连接`docker exec -it lc_mysql mysql -uroot -p`