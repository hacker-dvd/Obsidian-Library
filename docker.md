# 安装

```bash
sudo pacman -S docker
sudo systemctl enable --now docker.socket # 在首次需要使用 Docker 时才启动它

# 验证是否安装成功
sudo docker info
```

# 基础配置

+ 将自己添加到docker用户组中：`sudo usermod -aG docker your_username`

# 基本命令

+ 查看现在有哪些活动的容器：`docker ps`
	查看所有的容器：`docker ps -a`
+ 删除一个 docker 容器：
	先使用`docker stop 容器名`停用容器
	然后使用`docker rm 容器名`删除容器
+ docker删除镜像：`docker rmi ID`