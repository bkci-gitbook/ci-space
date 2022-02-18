# CI镜像

bk-ci 提供了默认的 Ubuntu 镜像，但不一定能满足所有编译场景，你可以通过这篇文章基于默认镜像制作自定义镜像。

* 默认镜像： [bkci/ci:latest](https://github.com/ci-plugins/base-images)​

### &#x20;<a href="#zhun-bei-cai-liao" id="zhun-bei-cai-liao"></a>

* ​[docker build](https://docs.docker.com/engine/reference/commandline/build/)相关 知识
* 一台 linux 构建机
* 一个可以在机器上成功构建出镜像的 Dockerfile 工程

1.  1\.

    登录构建机，将 Dockerfile 工程同步到构建机，进入 Dockerfile 工程目录

Dockerfile 示例：

1

FROM bkci/ci:latest

2

​

3

RUN yum install -y mysql-devel

Copied!

1.  1\.

    执行 docker build

> 重要提示：
>
> * 因为流水线里面的容器是通过 CMD，使用/bin/sh 启动的，因此必须保证镜像里面存在/bin/sh 以及 curl 命令（用来下载 Agent）
> * 不要设置 ENTRYPOINT
> * 确保为 64 位镜像
> * 用户用 root，如需普通用户可以在 bash 里面切换，否则流水线任务启动不了

1

docker build -t XXX.com/XXX/YYY:latest -f Dockerfile .

Copied!

1.  1\.

    执行 docker login

1

docker login XXX.com

Copied!

1.  1\.

    执行 docker push

1

docker push XXX.com/XXX/YYY:latest

Copied!

### &#x20;<a href="#jie-xia-lai-ni-ke-neng-xu-yao" id="jie-xia-lai-ni-ke-neng-xu-yao"></a>

* ​发布一个容器镜像​
