# dockerfile构建镜像

常用的dockerfile存放在一个路径下，一般文件名就叫**/dockerfile,文件内容基本如下：

```bash
FROM # 基础镜像， 一切从这里开始构建
MAINTAINER # 镜像是谁写的， 姓名+邮箱
RUN# 镜像构建的时候需要运行的命令
ADD# 拷贝文件（支持正则表达式）到镜像,并自动解压（如果是压缩包）
WORKDIR # 镜像的工作目录
VOLUME # 挂载的目录
EXPOSE # 保留端口配置
CMD # 指定这个容器启动的时候要运行的命令， 只有最后一个会生效，可被替代。 ENTRYPOINT # 指定这个容器启动的时候要运行的命令， 可以追加命令
COPY # 类似ADD， 将我们文件拷贝到镜像中 
ENV # 构建的时候设置环境变量！
```

**（1）FROM**

基础镜像，必须是可以下载下来的，定制的镜像都是基于 FROM 的镜像，这里的 centos就是定制需要的基础镜像。后续的操作都是基于centos镜像。

**（2）MAINTAINER**

指定镜像的作者信息

**（3）RUN：指定在当前镜像构建过程中要运行的命令**

包含两种模式

1、Shell

`RUN <command> (shell模式，这个是最常用的，需要记住)`

`RUN echo hello`

2、exec模式

`RUN [“executable”，“param1”，“param2”]`

`RUN [“/bin/bash”,”-c”,”echo hello”] 等价于/bin/bash -c echo hello`

**（4）EXPOSE指令**

仅仅只是声明端口。

作用：

1、帮助镜像使用者理解这个镜像服务的守护端口，以方便配置映射。

2、在运行时使用随机端口映射时，也就是 docker run -P 时，会自动随机映射 EXPOSE 的端口。

3、可以是一个或者多个端口，也可以指定多个EXPOSE

格式：EXPOSE <端口1> [<端口2>...]

**（5）CMD**

类似于 RUN 指令，用于运行程序，但二者运行的时间点不同:

1. CMD 在docker run 时运行。

2. RUN 是在 docker build构建镜像时运行的

**作用**：为启动的容器指定默认要运行的程序，程序运行结束，容器也就结束。CMD 指令指定的程序可被 docker run 命令行参数中指定要运行的程序所覆盖。

`CMD[“executable”，“param1”，“param2”]  #（exec模式）`

`CMD command （shell模式）`

`CMD [“param1”,”param2”] # (作为ENTRYPOINT指令的默认参数)`

**例：**

```bash
cd /root/dockerfile/test

cat dockerfile

#first dockerfile

FROM centos

MAINTAINER xianchao

RUN yum clean all

RUN  yum  install  nginx -y

EXPOSE 80

CMD ["/usr/sbin/nginx","-g","daemon off;"]
```

**构建镜像：**

```bash
docker build -t="dockerfile/test-cmd:v1" .
```

**基于上面构建的镜像运行一个容器**

```bash
docker run -p 80 --name cmd_test2 -d dockerfile/test-cmd:v1
```

**（不需要跟nginx  -g “daemon off；”了）**

```bash
docker ps
```

**可以看到下面信息**

`b903d5a71279    dockerfile/test-cmd:v1               "/usr/sbin/nginx -g …"  7 seconds ago    Up 6 seconds    0.0.0.0:32770->80/tcp  cmd_test2`

**（6）ENTERYPOINT**

类似于 CMD 指令，但其不会被 docker run 的命令行参数指定的指令所覆盖，而且这些命令行参数会被当作参数送给 ENTRYPOINT 指令指定的程序。

但是, 如果运行 docker run 时使用了 --entrypoint 选项，将覆盖 entrypoint指令指定的程序。

优点：在执行 docker run 的时候可以指定 ENTRYPOINT 运行所需的参数。

注意：如果 Dockerfile 中如果存在多个 ENTRYPOINT 指令，仅最后一个生效。

**格式：**

`ENTERYPOINT [“executable”,“param1”,“param2”] # (exec模式)`

`ENTERYPOINT command （shell模式）`

可以搭配 CMD 命令使用：一般是变参才会使用 CMD ，这里的 CMD 等于是在给 ENTRYPOINT 传参，以下示例会提到。

**示例：**

假设已通过 Dockerfile 构建了 nginx:test 镜像：

`FROM nginx`

`ENTRYPOINT ["nginx", "-c"] # 定参`

`CMD ["/etc/nginx/nginx.conf"] # 变参`

**1、不传参运行**

`$ docker run nginx:test`

容器内会默认运行以下命令，启动主进程。

`nginx -c /etc/nginx/nginx.conf`

**2、传参运行**

`$ docker run nginx:test -c /etc/nginx/new.conf`

容器内会默认运行以下命令，启动主进程(/etc/nginx/new.conf:假设容器内已有此文件)

`nginx -c /etc/nginx/new.conf`

**（7）COPY**

`COPY<src>..<dest>`

`COPY[“<src>”...“<dest>”]`

**复制指令，从上下文目录中复制文件或者目录到容器里指定路径。**

**格式：**

`COPY [--chown=<user>:<group>] <源路径1>... <目标路径>`

`COPY [--chown=<user>:<group>] ["<源路径1>",... "<目标路径>"]`

[--chown=<user>:<group>]：可选参数，用户改变复制到容器内文件的拥有者和属组。

<源路径>：源文件或者源目录，这里可以是通配符表达式，其通配符规则要满足 Go 的 filepath.Match 规则。例如：

`COPY hom* /mydir/`

`COPY hom?.txt /mydir/`

<目标路径>：容器内的指定路径，该路径不用事先建好，路径不存在的话，会自动创建。

**（8）ADD**

`ADD <src>...<dest>`

`ADD [“<src>”...“<dest>”]`

**ADD 指令和 COPY 的使用格式一致（同样需求下，官方推荐使用 COPY）。功能也类似，不同之处如下：**

ADD 的优点：在执行 <源文件> 为 tar 压缩文件的话，压缩格式为 gzip, bzip2 以及 xz 的情况下，会自动复制并解压到 <目标路径>。

ADD 的缺点：在不解压的前提下，无法复制 tar 压缩文件。会令镜像构建缓存失效，从而可能会令镜像构建变得比较缓慢。具体是否使用，可以根据是否需要自动解压来决定。

**ADD vs COPY**

ADD包含类似tar的解压功能

如果单纯复制文件，dockerfile推荐使用COPY

**例;替换/usr/share/nginx下的index.html**

```bash
cd /root/dockerfile/test1

cat  dockerfile

FROM centos

MAINTAINER xianchao

RUN yum install wget -y

RUN yum install nginx -y

COPY index.html /usr/share/nginx/html/

EXPOSE 80

ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]

vim index.html

<html>

<head>

<title>page added to dockerfile</title>

</head>

<body>

<h1>i am in df_test </h1>

</body>

</html>

docker build -t="dockerfile/copy:v1" .

docker run -d -p 80 --name html3 dockerfile/copy:v1

docker ps | grep html3
```

**显示如下：**

```bash
478868402ac4        dockerfile/copy:v1                                  "/usr/sbin/nginx -g …"   15 seconds ago      Up 12 seconds       0.0.0.0:32771->80/tcp   html3

curl 192.168.40.180:32771
```

**显示的就是替换后的页面**

```bash
<html>

<head>

<title>page added to dockerfile</title>

</head>

<body>

<h1>i am in df_test </h1>

</body>

</html>
```

**（9）VOLUME**

定义匿名数据卷。在启动容器时忘记挂载数据卷，会自动挂载到匿名卷。

作用：

1、避免重要的数据，因容器重启而丢失，这是非常致命的。

2、避免容器不断变大。

**格式：**

`VOLUME ["<路径1>", "<路径2>"...]`

**VOLUME <路径>**

在启动容器 docker run 的时候，我们可以通过 -v 参数修改挂载点。

`VOLUME [“/data”]`

**（10）WORKDIR**

指定工作目录。用 WORKDIR 指定的工作目录，会在构建镜像的每一层中都存在。（WORKDIR 指定的工作目录，必须是提前创建好的）。

docker build 构建镜像过程中的，每一个 RUN 命令都是新建的一层。只有通过 WORKDIR 创建的目录才会一直存在。

**格式：**

**WORKDIR <工作目录路径>**

WORKDIR /path/to/workdir（填写绝对路径）

**(11 )ENV**

设置环境变量

``ENV <key> <value>``

``ENV <key>=<value>...``

以下示例设置 NODE_VERSION =6.6.6， 在后续的指令中可以通过 $NODE_VERSION

**引用：**

```bash
ENV NODE_VERSION 6.6.6
RUN curl -SLO "<https://nodejs.org/dist/v$NODE_VERSION/node-v$NODE_VERSION-linux-x64.tar.xz>" \\

&& curl -SLO "<https://nodejs.org/dist/v$NODE_VERSION/SHASUMS256.txt.asc>"
```

**（12）USER**

用于指定执行后续命令的用户和用户组，这边只是切换后续命令执行的用户（用户和用户组必须提前已经存在）。

**格式：**

```**USER <用户名>[:<用户组>]**
USER daemon

USER nginx

USER user

USER uid

USER user:group

USER uid:gid

USER user:gid

USER uid:group
```

**（13）ONBUILD**

用于延迟构建命令的执行。简单的说，就是 Dockerfile 里用 ONBUILD 指定的命令，在本次构建镜像的过程中不会执行（假设镜像为 test-build）。当有新的 Dockerfile 使用了之前构建的镜像 FROM test-build ，这时执行新镜像的 Dockerfile 构建时候，会执行 test-build 的 Dockerfile 里的 ONBUILD 指定的命令。

**格式：**

**ONBUILD <其它指令>**

为镜像添加触发器

当一个镜像被其他镜像作为基础镜像时需要写上OBNBUILD

会在构建时插入触发器指令

**例：演示ONBUILD指令**

```bash
cat index.html

<html>

<head>

<title>page added to dockerfile</title>

</head>

<body>

<h1>i am in df_test </h1>

</body>

</html>
vim dockerfile

FROM centos

MAINTAINER xianchao

RUN yum install wget -y

RUN yum install nginx -y

ONBUILD COPY index.html /usr/share/nginx/html/

EXPOSE 80

ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]

docker build -t="onbuild-nginx:v1" .

docker run -d --name html4 -p 80 onbuild-nginx:v1

docker ps | grep html4
```

**显示如下：**

```bash
65f4a5be9355        onbuild-nginx:v1                                    "/usr/sbin/nginx -g …"   14 seconds ago      Up 11 seconds       0.0.0.0:32772->80/tcp   html4

curl 192.168.40.180:32772
```

显示还是以前nginx默认的内容，没有被替换，表示ONBUILD这个指令后面的COPY没有生效

还是在刚在路径下构建新的镜像

```bash
vim dockerfile

FROM onbuild-nginx:v1

MAINTAINER xianchao

ENTRYPOINT ["/usr/sbin/nginx","-g","daemon off;"]

EXPOSE 80

docker build -t="onbuild-nginx1" .

docker run -d --name htm5 -p 80 onbuild-nginx1

docker ps | grep htm5
```

**显示如下：**

```bash
**e56542310692        onbuild-nginx1                                      "/usr/sbin/nginx -g …"   12 seconds ago      Up 8 seconds        0.0.0.0:32773->80/tcp   htm5**

**curl 192.168.40.180:32773**
```

**显示如下：**

```html
<html>

<head>

<title>page added to dockerfile</title>

</head>

<body>

<h1>i am in df_test </h1>

</body>

</html>
```

显示的就是已经重新构建的镜像，页面就是替换之后的了，说明我们基于ONBUILD指令的镜像作为基础镜像，在构建镜像，会触发ONBUILD后面的COPY命令运行

**（14）LABEL**

LABEL 指令用来给镜像添加一些元数据（metadata），以键值对的形式，语法格式如下：

`LABEL <key>=<value> <key>=<value> <key>=<value> ...`

比如我们可以添加镜像的作者：

`LABEL org.opencontainers.image.authors="xianchao"`

**（15）HEALTHCHECK**

用于指定某个程序或者指令来监控 docker 容器服务的运行状态。

格式：

`HEALTHCHECK [选项] CMD <命令>：设置检查容器健康状况的命令`

`HEALTHCHECK NONE：如果基础镜像有健康检查指令，使用这行可以屏蔽掉其健康检查指令`

`HEALTHCHECK [选项] CMD <命令> : 这边 CMD 后面跟随的命令使用，可以参考 CMD 的用法。`

**（16）ARG**

构建参数，与 ENV 作用一至。不过作用域不一样。ARG 设置的环境变量仅对 Dockerfile 内有效，也就是说只有 docker build 的过程中有效，构建好的镜像内不存在此环境变量。

构建命令 docker build 中可以用` --build-arg <参数名>=<值> `来覆盖。

格式：

`ARG <参数名>[=<默认值>]`