# 蓝盾FAQ

\[TOC]

#### Q: 流水线的各个状态代表什么意思？

流水线的状态汇总如下：&#x20;

![](<../../.gitbook/assets/image (2).png>)

#### Q: 蓝盾流水线进度条是怎么计算的？

进度条是蓝盾前端根据流水线相关的数据做出的预估。此进度不是准确的时间，仅供参考。&#x20;

![](<../../.gitbook/assets/image (3).png>)

#### Q: 蓝盾流水线构建出的产物如何支持服务器分发限速配置?

调整分发源的限速，如下图。 对于已经安装agent的机器，可以先移除，再安装。 分发源机器IP: 192.168.5.134&#x20;

![](<../../.gitbook/assets/image (8).png>)

#### Q: 如何获取流水线id？

流水线url中，pipeline后的参数分别为项目id和流水线id。如：http://devops.oa.com/console/pipeline/iccentest/p-8f3d1b399897452e901796cf4048c9e2/history 中，iccentest 为项目id，p-xxx 即为流水线id。

#### Q: 项目名称是否支持修改？

项目名称可在项目管理内更改，项目英文缩写（即项目id）不能更改。 &#x20;

![](../../.gitbook/assets/image.png)

![](<../../.gitbook/assets/image (5).png>)

#### Q: 流水线执行失败了，插件为什么没有重试按钮？

只有最新一次的构建可以重试。

#### Q: 如何通过蓝盾将构建产物自动分发到指定服务器？

有了部署机器，我们可以将构件分发至测试机上了。首先添加一个无编译环境Job 3-1，添加插件作业平台-构件分发并完成配置。&#x20;

![](<../../.gitbook/assets/image (7).png>)

#### Q: 蓝盾有哪些全局变量？

https://iwiki.woa.com/pages/viewpage.action?pageId=26941983

#### Q: 查看日志时，如何查看时间戳？

查看日志页，Show/Hide Timestamp&#x20;

![](<../../.gitbook/assets/image (4).png>)

#### Q: 蓝盾流水线中的视图管理、标签管理有什么用？

对流水线进行分类，当流水线数量较多时，标签、视图会有更大作用。

#### Q: 为什么有时候会出现需要申请流水线权限的情况，但是F5刷新之后恢复？

存在权限冲突，在用户组权限里，是有多个流水线的权限。 但是自定义里面只有一个流水线的权限。 后续更新版本会修复这个问题。解决方案为删除自定义权限。后续会通过版本更新修复该问题。&#x20;

![](<../../.gitbook/assets/image (1).png>)

#### Q: python的环境变量添加后，在job执行的时候未生效。（job报错“系统找不到指定的文件”）

因为蓝盾agent和蓝鲸agent使用的账户是system，所以加到administrator的环境变量不生效 需要把python.exe和pip3.exe pip.exe加入到系统环境变量里，再重启操作系统

#### Q: 可以通过蓝盾流水线上传构建产物到指定私有GitLab仓库吗？

蓝盾git插件暂无push功能。用户可将ssh私钥放置构建机上，在Batch Script插件或者Bash插件里使用git命令push产物达到临时解决方案。
