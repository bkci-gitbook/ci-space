# 凭证管理

在流水线运行期间，你可能需要很多种凭据类型在拉取代码库、调用代码库 API、通过账号密码访问第三方服务时使用。

### &#x20;<a href="#chuang-jian-ping-zheng" id="chuang-jian-ping-zheng"></a>

![](https://589213227-files.gitbook.io/\~/files/v0/b/gitbook-28427.appspot.com/o/assets%2F-MZIuzLgCmrIqRRM\_hhk%2F-MZNYhrHcPnRv4WdAwXh%2F-MZNYtcV3FHKQ9OB2NuH%2Fimage.png?alt=media\&token=671a2499-acbd-48d2-9647-34ce8a5b26c5)

目前支持以下几种凭据类型：

类型

描述

密码

定义后会作为流水线变量在各种插件中引用

用户名+密码

一般在关联 SVN 代码库时用到

SSH 私钥

关联 GitLab 代码库时（不需要 GitLab 事件触发）使用

SSH 私钥+私有 Token

关联 GitLab 代码库时（需要 GitLab 事件触发）使用

用户名密码+私有 token

关联 GitLab 代码库时（需要 GitLab 事件触发）使用

AccessToken

关联 GitLab 代码库时（需要 GitLab 事件触发）使用

### &#x20;<a href="#cha-kan-ping-ju" id="cha-kan-ping-ju"></a>

可以在这里查看所有有权限的已关联凭据。

![](https://589213227-files.gitbook.io/\~/files/v0/b/gitbook-28427.appspot.com/o/assets%2F-MZIuzLgCmrIqRRM\_hhk%2F-MZNYhrHcPnRv4WdAwXh%2F-MZNZ3yNAklah\_3wkIom%2Fimage.png?alt=media\&token=33ff5d08-5ee2-48f9-bb71-a1578f5c5ae1)

### &#x20;<a href="#jie-xia-lai-ni-ke-neng-xu-yao" id="jie-xia-lai-ni-ke-neng-xu-yao"></a>

* ​[创建你的第一条流水线](broken-reference)​
