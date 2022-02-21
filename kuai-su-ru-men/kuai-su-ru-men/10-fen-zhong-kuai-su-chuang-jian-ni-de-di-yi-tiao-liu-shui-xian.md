# 《10分钟快速创建你的第一条流水线》

### 目的

通过step-by-step 的演示，能够帮助用户在蓝盾体验环境上快速创建一条流水线，进而了解蓝盾的基础功能及操作流程。

### 准备事项

* 开通蓝盾体验环境权限

详情见：[【服务说明】蓝盾体验环境](https://doc.weixin.qq.com/doc/w3\_AOkAbAbdAFwKorZFQATRDSOBB31Pz?scode=AJEAIQdfAAoyfU7volAOkAbAbdAFw) " **三、权限开通** "部分

* 体验环境访问配置

详情见：[【服务说明】蓝盾体验环境](https://doc.weixin.qq.com/doc/w3\_AOkAbAbdAFwKorZFQATRDSOBB31Pz?scode=AJEAIQdfAAo4L119mbAOkAbAbdAFw) " **四、配置指引** "部分

### 创建你的第一条流水线

完成上述准备事项后，我们开始配置如下流水线：

**场景：**

利用公共构建机从gitlab上"拉取文件"，并对文件做"打包压缩"处理，然后将压缩包做"上传操作"；再利用另外一台公共构建机"下载压缩包"，然后对下载结果进行”验证“

![](../../.gitbook/assets/0)

1. **打开蓝盾**，输入用户名和密码

http://devops-ef.bktencent.com/console/

![](../../.gitbook/assets/1)

2、选择“**服务--流水线**”

![](../../.gitbook/assets/2)

3、第一次使用，请**“新建项目”**

![](../../.gitbook/assets/3)

4、切换到你的项目，点击“**创建流水线**”

![](../../.gitbook/assets/4)

5、**编辑流水线**：

* **setp 1-流水线取名**：输入流水线名称“demo”，点击“**新建**”；系统会自动添加Trigger（触发）job

_<即流水线构建的触发方式，目前包括：手动触发、定时触发、代码库事件触发以及远程触发。>_

![](../../.gitbook/assets/5)

![](../../.gitbook/assets/6)

* **setp 2-添加新stage**：选择job类型：linux（以linux为例）

![](../../.gitbook/assets/7)

![](../../.gitbook/assets/8)

* **setp 3-拉文件**：点击“新增插件”，搜索插件“Checkout gitlab”，点击选择

![](../../.gitbook/assets/9)

![](../../.gitbook/assets/10)

**选择代码库**，如果第一次使用，需要关联代码库

![](../../.gitbook/assets/11)

* **setp 4-打包压缩**：继续添加“插件”，我们选择“shell Script”

![](../../.gitbook/assets/12)

我们在Content里，添加shell 压缩脚本

\#!/usr/bin/env bash

ls -l ${WORKSPACE}

zip -r ShortUrl.zip ShortUrl

![](../../.gitbook/assets/13)

* **setp 5-上传文件**：继续添加“插件”，我们选择“Upload artifacts”

![](../../.gitbook/assets/14)

配置上传路径为：./ShortUrl.zip

![](../../.gitbook/assets/15)

* **setp 6-添加新"Stage"** : 我们继续选择“linux”

![](../../.gitbook/assets/16)

* **setp 7-下载压缩包**：添加新“插件”，我们选择“Downlad artifacts”

![](../../.gitbook/assets/17)

选择Pipeline名称，配置path（./ShortUrl.zip）、local path（./build） 参数

![](../../.gitbook/assets/18)

* **setp 8-验证结果**：验证是否有拉取到压缩包：ShortUrl.zip

编写如下shell脚本

\#!/usr/bin/env bash

cd /data/devops/workspace/build

ls -l

![](../../.gitbook/assets/19)

6、点击**保存并执行**

![](../../.gitbook/assets/20)

**7、**你的构建已经**成功运行**，请耐心**等待结束**

查看验证结果：是否有压缩文件“ShortUrl.zip”

### 常见问题

详情见：[【服务说明】蓝盾体验环境](https://doc.weixin.qq.com/doc/w3\_AOkAbAbdAFwKorZFQATRDSOBB31Pz?scode=AJEAIQdfAAoyfU7volAOkAbAbdAFw) " **六、FAQ** "部分

### 结语

在使用蓝盾体验环境过程中，如遇到任何问题，欢迎随时咨询我们。我们背后是蓝盾的整个开发、产品和支撑团队，没有解决不了的问题，所以请放心地咨询。但由于咨询量很大，有时候我们需要同时处理多个客户的问题，可能会无法及时回复，但你所反馈的问题，我们都会在24小时内给予答复，希望大家多点包容和理解。

更多指引欢迎查阅蓝盾官方文档中心：https://docs.bkci.net/tutorials/create-first-pipeline
