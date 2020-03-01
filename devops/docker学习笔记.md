# Docker学习笔记



### dockerFile最佳实践

Docker映像由只读层组成，每个只读层代表一个Dockerfile指令。 这些层是堆叠的，每个层都是上一层的变化的增量。运行图像并生成容器时，可以在基础层之上添加一个新的可写层（“容器层”）。 对运行中的容器所做的所有更改（例如写入新文件，修改现有文件和删除文件）都将写入此可写容器层中。

1. build时的指定目录会作为构建上下文被发送到docker守护程序，目录内的文件应该尽可能少
2. 使用.dockerignore过滤不需要发送到上下文的文件
3. 使用多阶段的构建来减小image大小(不需要减少中间层和文件，利用生成缓存最小化image，image在构建阶段的最后才会生成)
4. 最小化image的层 只有指令RUN，COPY，ADD会创建图层。 其他说明创建临时的中间映像，并且不会增加构建的大小
5. dockerFile中添加大文件的操作应该放在最后 最好使用muti stage来进行创建[multi-stage](https://docs.docker.com/v17.12/develop/develop-images/multistage-build/)

### 常用命令

可以将image push到私有的仓库,docker的cli默认是push到docker hub，可以参考https://docs.docker.com/datacenter/dtr/2.2/guides/进行配置私有仓库
``` dockerfile
docker build -t friendlyhello .  # Create image using this directory's Dockerfile
docker run -p 4000:80 friendlyhello  # Run "friendlyname" mapping port 4000 to 80
docker run -d -p 4000:80 friendlyhello         # Same thing, but in detached mode
docker container ls                                # List all running containers
docker container ls -a             # List all containers, even those not running
docker container stop <hash>           # Gracefully stop the specified container
docker container kill <hash>         # Force shutdown of the specified container
docker container rm <hash>        # Remove specified container from this machine
docker container rm $(docker container ls -a -q)         # Remove all containers
docker image ls -a                             # List all images on this machine
docker image rm <image id>            # Remove specified image from this machine
docker image rm $(docker image ls -a -q)   # Remove all images from this machine
docker login             # Log in this CLI session using your Docker credentials
docker tag <image> username/repository:tag  # Tag <image> for upload to registry
docker push username/repository:tag            # Upload tagged image to registry
docker run username/repository:tag                   # Run image from a registry
```

### 其他调试命令

```dockerfile
docker history --no-trunc image_id 查看各个layer的大小
```

