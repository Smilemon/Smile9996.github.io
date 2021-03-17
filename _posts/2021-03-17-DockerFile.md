---
layout:     post
title:      DockerFile
subtitle:   
date:       2021-03-17
author:     Smile
# header-img: img/post-bg-cook.jpg
catalog: true
tags:
    - Docker
---



## DockerFile

​	dockerfile 是用来构建 docker 镜像的文件

​	**DockerFile**：定义了一切的步骤

​	**DockerImages**：通过DockerFile 构建生成的镜像，最终发布和运行的产品

​	**Docker容器**：容器就是镜像运行起来提供服务的

​	构建步骤：

​	1、编写一个 dockerfile 文件

​	2、docker build 构建成为一个镜像

​	3、docker run 运行镜像

​	4、docker push 发布镜像（DockerHub，阿里云镜像仓库）

### DockerFile构建过程

#### 基础知识：

​	1、每个保留关键字（指令）都是必须是大写字母

​	2、执行从上到下顺序执行

​	3、# 表示注释

​	4、每一个指令都会创建提交一个新的镜像层，并提交

### DockerFile指令

```shell
FROM         # 基础镜像，一切从这里开始构建
MAINTAINER   # 镜像是谁写的，姓名+邮箱
RUN          # 镜像构建的时候需要运行的命令
ADD          # 步骤：tomcat镜像，添加内容：tomcat压缩包
WORKDIR      # 镜像的工作目录
VOLUME       # 挂载到目录
EXPOSE       # 指定暴露端口
CMD          # 指定这个容器指定的时候要运行的命令，只有最后一个会生效，可被替代
             # CMD ["ls", "-a"]
             # docker run -it 镜像名:[tag] 会显示文件目录
             # docker run -it 镜像名:[tag] -l 追加参数会报错
             # CMD的情况下 -l 替换了CMD["ls", "-a"]命令，-l 不是命令，所以报错
ENTRYPOINT   # 指定这个容器指定的时候要运行的命令，可以追加命令
             # ENTRYPOINT["ls", "-a"]
             # docker run -it 镜像名:[tag] 会显示文件目录
             # docker run -it 镜像名:[tag] -l 追加参数可以执行
             # 追加的命令，是直接拼接到 ENTRYPOINT 命令的后面
ONBUILD      # 当构建一个被继承 DockerFile 这个时候就会运行 ONBUILD 的指令。触发指令
COPY         # 类似ADD，将我们的文件拷贝到镜像中
ENV          # 构建的时候设置环境变量
```



![image-20210316221105320](https://smile9996.oss-cn-shanghai.aliyuncs.com/github/image/imgDocker/image-20210316221105320.png)

### DockerFile实战测试

>1、创建一个自己的centos
>
>```shell
>[smile@hadoop101 dockerfile]$ cat mydockerfile-centos
>FROM centos
>MAINTAINER smile<wait3124@163.com>
>
>ENV MYPATH /usr/local
>WORKDIR $MYPATH
>
>RUN yum -y install vim
>RUN yum -y install net-tools
>
>EXPOSE 80
>EXPOSE 22
># CMD ["ls","-a"]
>CMD echo $MYPATH
>CMD echo "------end------"
>
>CMD /bin/bash
>```
>
>2、通过这个文件构建镜像
>
>```shell
># 通过这个文件构建镜像
># 命令 docker build if dockerfile文件路径 -t 镜像名称:[TAG] . (不要忘了最后还有一个点)
>docker build -f mydockerfile-centos -t mycentos:1.0 .
># 输出
>...
>Successfully built 578ba964a6e9
>Successfully tagged mycentos:1.0
>
>```
>
>3、启动测试运行
>
>对比之前原生centos
>
>![image-20210316225419662](https://smile9996.oss-cn-shanghai.aliyuncs.com/github/image/imgDocker/image-20210316225419662.png)
>
>**可以用 docker history 镜像id 查看镜像是怎么做的**

