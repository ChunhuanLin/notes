## 生成和使用镜像  

docker build -t friendlyhello .  
在当前目录下生成一个名为friendlyhello的镜像，该目录下要有Dockerfile及其所依赖的相关文件

docker images  
查看本地的镜像仓库  

docker run -p 4000:80 friendlyhello  
4000是主机端口，80是在Dockerfile里设置的对外开放的容器端口，最终要在主机的localhost:4000访问该服务  
可以在浏览器中输入localhost:4000访问，也可以在终端中输入 curl http://localhost:4000  


docker run -d -p 4000:80 friendlyhello  
在background运行docker  

docker ps  
打印当前正在运行的容器信息  

docker stop 1fa4ab2cf395  
停止某个容器，后面的字符串是该容器的ID  


## 分享镜像
### 注册账户、仓库与镜像的关系
A registry is a collection of repositories, and a repository is a collection of images—sort of like a GitHub repository, except the code is already built. An account on a registry can create many repositories. The docker CLI uses Docker’s public registry by default.

docker login  
登录你的Docker帐号  

docker tag image username/repository:tag  
这个命令标记你本地的Image所要上传的对应的账户和仓库，tag是可选的，但建议使用，可以通过他来给出镜像的版本号，如果不指定tag，那么它默认为latest

docker push username/repository:tag  
上传docker镜像,上传之后就可以在Docker Hub看到这个新的镜像。  


## 从远程仓库pull并运行镜像

docker run -p 4000:80 username/repository:tag  
如果docker在本地找不到这个镜像，那么他就会从远程把这个镜像pull到本地。  

## nvidia-docker  

`nvidia-docker run -it -p hostPort:containerPort TensorFlowGPUImage` 在jupyter上运行tensorflow，这里的containerPort是8888  
`nvidia-docker run -it TensorFlowGPUImage bash` 可以在终端上运行tensorflow  

`-v host_folder:container_floder` 使得本地的文件在jupyter中可见  
`-p 8888:8888 -p 6006:6006` 前一个端口是jupyter的端口，后一个端口是tensorboard的端口  

-----------------我是分割线------------------


## 在服务中使用docker

### 什么是服务？
在分布式应用中，app的不同部分称为服务。比如说前端，后台，他们接管一个应用的不同服务。一个服务只需要跑一个镜像，然而他还要规范通信端口，容器应该跑多少部分等。  

docker-compose.yml  
定义Docker容器在产品中怎么做？  






