# TKEX Demo

**本篇文章将指引你通过蓝盾构建 docker 镜像并推送至 TKEX 容器服务。**

**（注：如果不需要使用到 TKEX 容器服务，只需构建镜像并推送至某个仓库，准备前3项材料，执行前4步步骤即可）**

### 准备材料： <a href="#tkexdemo-zhun-bei-cai-liao" id="tkexdemo-zhun-bei-cai-liao"></a>

1. 一个可以成功 docker build 的 Git 工程：[https://git.code.oa.com/ci-demo/docker](https://git.code.oa.com/ci-demo/docker)
2. 一个源镜像 host 的账号密码
3. 一个目标镜像地址的账号密码
4. 一个[TKEX容器服务](http://kubernetes.oa.com)

### 详细步骤: <a href="#tkexdemo-xiang-xi-bu-zhou" id="tkexdemo-xiang-xi-bu-zhou"></a>

**1. 配置**[**构建并推送Docker镜像**](http://devops.oa.com/console/store/atomStore/detail/atom/DockerBuildAndPushImage)**的流水线**

**2. 添加3-1的** [**无编译环境Job**](http://iwiki.oa.com/pages/viewpage.action?pageId=10718789)

**3. 在 Job 内添加如下插件**

6.1 [STKE-操作](http://devops.oa.com/console/store/atomStore/detail/atom/stkeAtom)：更新 STKE 容器服务实例&#x20;

![](<../../.gitbook/assets/image (5).png>)

**4. 运行流水线，观察结果**

![](<../../.gitbook/assets/image (28).png>)

**5. 到 STKE 容器服务查看实例状态是否正常运行**

![](<../../.gitbook/assets/image (7).png>)

![](<../../.gitbook/assets/image (23).png>)
