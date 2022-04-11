# docker file

## 1、说明

Dockfile 是一种被Docker程序解释的脚本，Dockerfile由一条一条的指令组成，每条指令对应Linux下面的一条命令。Dcker程序将这些Dockerfile指令翻译真正的Linux命令。Dockerfile有自己书写格式和支持的命令，Docker程序解决这些命令间的依赖关系，类似于Makefile。Docker程序将读取Dockerfile，根据指令生成定制的image。

## 2、转换命令

```shell
# docker build -t mangxiao/jdk-tomcat .
```

## 3、Dockerfile语法

### 3.1、FROM (指定基础image)

> 构建指令，必须指定且需要在Dockerfile其它指令的前面。后续的指令都依赖于该指定的image。FROM指令指定的基础image可以是官方远程仓库中的，也可以位于本地仓库。

```dockerfile
example:
	FROM centos:7.2
	FROM centos
```

### 3.2、MAINTAINER（用来指定镜像创建者信息）

> 构建指令，用于将image的制作者相关的信息写入到image中。当我们对该image执行docker inspect命令时，输出中有相应的字段记录该信息。

```dockerfile
example:
	MAINTAINER mangxiao"mangxiao2018@126.com"
```

### 3.3、RUN（安装软件用）

> 构建指令，RUN可以运行任何被基础image支持的命令。如基础image选择了Centos，那么软件管理部分只能使用Centos的包管理命令。

```dockerfile
example:
	RUN cd /tmp && curl -L'http://archive.apache.org/dist/tomcat/tomcat-7/v7.0.8/bin/apache-tomcat-7.0.8.tar.gz' | tar -xz
	RUN ["/bin/bash","-c","echo hello"]
```

### 3.4、CMD（设置container启动时执行的操作）

> 设置指令，用于container启动时指定的操作。该操作可以是执行自定义脚本，也可以是执行系统命令。该指令只能在文件中存在一次，如果有多个，则只执行最后一条。

```dockerfile
example:
	CMD echo "Hello, World!"
```

### 3.5、ENTRYPOINT(设置container启动时执行的操作)

> 设置指令，指定容器启动时执行的命令，可以多次设置，但是只有最后一个有效。

```dockerfile
example:
	ENTRYPOINT ls -l
```

> 该指令的使用分为两种情况，一种是独自使用，另一种和CMD指令配合使用。当独自使用时，如果你还使用了CMD命令且CMD是一个完整的可执行的命令，那么CMD指令和ENTRYPOINT会互相覆盖只有最后一个CMD或者ENTRYPOINT有效

```dockerfile
# CM指令将不会被执行，只有ENTRYPOINT指令被执行
CMD echo "Hello, World!"
ENTRYPOINT ls -l
```

> 另一种用法和CMD指令配合使用来指定ENTRYPOINT 的默认参数，这时CMD指令不是一个完整的可执行命令， 仅仅是参数部分；ENTRYPOINT指令只能使用JSON方式指定执行命令，而不能指定参数。

```doc
FROM ubuntu
CMD["-l"]
ENTRYPOINT["/usr/bin/ls"]
```

### 3.6、USER(设置container容器的用户)

> 设置指令，设置启动容器的用户，默认是root用户

``` dockerfile
example:
	USER daemon = ENTRYPOINT["memcached","-u","daemon"]
```

### 3.7、EXPOSE(指定容器需要映射到宿主机器的端口)

设置指令，该指令会将容器中的端口映射成宿主机器中的某个端口。当你需要访问容器的时候，可以不是用容器的IP地址而是使用宿主机器的IP地址和映射后的端口。要完成整个操作需要两个步骤，首先在Dockerfile使用EXPOSE设置需要映射的容器端口，然后在运行容器的时候指定-p选项加上EXPOSE设置的端口，这样EXPOSE设置的端口号会被随机映射成宿主机器中的一个端口号。也可以指定需要映射到宿主机器的那个端口，这时要确保宿主机器上的端口号没有被使用。

EXPOSE指令可以一次设置多个端口号，相应的运行容器的时候，可以配套的多次使用-p选项。

```dockerfile
example:
	映射一个端口
	EXPOSE 22
	相应的运行容器使用的命令
	docker run -p port1 image
	映射多个端口
	EXPOSE port1 port2 port3
	相应的运行容器使用的命令
	docker run -p port1 -p port2 -p port3 image
	还可以指定需要映射到宿主机器上的某个端口号
	docker run -p host_port1:port1 -p host_port2:port2 -p host_port3:port3 image
```

### 3.8、ENV(用于设置环境变量)

构建指令，在image中设置一个环境变量

example:

​	设置了后，后续的RUN命令都可以使用，container启动后，可以通过docker inspect查看这个环境变量，也可以通过在docker run --env key=value时设置或修改环境变量。假如你安装了JAVA程序，需要设置JAVA_HOME,那么可以在Dockerfile中这样写：

```dockerfile
# ENV JAVA_HOME/path/to/java/dirent
```

### 3.9、ADD

从src复制文件到container的dest路径

```dockerfile
# example:
# ADD <src> <dest>
# <src>是相对被构建的源目录的相对路径，可以是文件或目录的路径，也可以是一个远程的文件url
# <dest>是container中的绝对路径
```

### 3.10、COPY

从src复制文件到container的dest路径

```dockerfile
# example:
# COPY <src> <dest>
```

### 3.11、VOLUME指定挂载点

设置指令，使容器中的一个目录具有持久化存储数据的功能，该目录可以被容器本身使用，也可以共享给其它容器使用。我们知道容器使用的是AUFS，这种文件系统不能持久化数据，当容器关闭后，所有的更改都会丢失。当容器中的应用有持久化数据的需求时可以在Dockerfile中使用该指令。

```dockerfile
# example:
# FROM base
# VOLUME["/tmp/data"]
```

### 3.12、WORKDIR切换目录

设置指令，可以多次切换(相当于cd命令)，对于RUN，CMD，ENTRYPOINT生效

```dockerfile
# example:
# WORKDIR /p1 WORKDIR /p2 RUN vim a.txt
```

### 3.13、ONBUILD在子镜像中执行

ONBUILD指定的命令在构建镜像时并不执行，而是在它的子镜像中执行。

```dockerfile
# example:
# ONBUILD ADD . /app/src
# ONBUILD RUN /
```

