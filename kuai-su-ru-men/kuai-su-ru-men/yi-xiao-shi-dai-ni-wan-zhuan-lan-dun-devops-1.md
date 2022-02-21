# 一小时带你玩转蓝盾DevOps - 1

## 序章：基本概念 <a href="#e5-ba-8f-e7-ab-a0-ef-bc-9a-e5-9f-ba-e6-9c-ac-e6-a6-82-e5-bf-b5" id="e5-ba-8f-e7-ab-a0-ef-bc-9a-e5-9f-ba-e6-9c-ac-e6-a6-82-e5-bf-b5"></a>

首先给大家普及下蓝盾流水线的基本概念，这里能帮助大家在后面的章节更好地理解描述的内容。&#x20;

![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202004%2F1586945643\_43\_w1240\_h572.png\&is\_redirect=1)



蓝盾提供可视化的流水线编排页面，里面包含了 Stage、Job、Task结构。\
**Task（插件）**：单独的任务，如拉取Gitlab仓库代码。\
**Job（作业）**：运行在一个特定构建环境里。由多个Task组成，且当一个Task失败，则该Job失败，其余Task不再执行。\
**Stage（阶段）**：由多个Job组成。同个Stage下的Job并行执行，且Job与Job之间相互独立。当一个Job失败，则该Stage失败。\
**Pipeline（流水线）**：由多个Stage组成。同个Pipeline下的Stage串行执行，一个Stage失败，将不会执行后续Stage。当一个Stage失败，则该Pipeline失败。\
**Materials（源材料）**：即用于构建的代码。流水线中添加拉取代码库的插件后，流水线便有了材料。\
**Triggers（触发）**：流水线构建的触发方式，目前包括：手动触发、定时触发、代码库事件触发以及远程触发。后文_2.2_会详细讲解。\
**Artifact（构件）**：编译或打包等操作后产生的文件，如镜像、版本压缩包、ipa包、apk包等等，可被归档至版本仓库。\
**Node（节点）**：分为构建节点和服务器节点。构建节点用于任务构建，包括源代码拉取、编译和打包等功能；服务器节点用于构件部署，进行功能验证或性能测试等。\
**Workspace（工作空间）**：构建机上的工作目录，也是所有与构建机相关的插件执行时的相对目录。

## 第一章：基础操作

### 1.1 创建流水线

打开[http://devops.oa.com](http://devops.oa.com)\
选择`流水线服务`，完成项目创建后，点击`创建流水线`。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202007%2F1593698120\_99\_w804\_h654.png\&is\_redirect=1)![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202111%2F1638180896-5121-61a4a8207d085-682218.png\&is\_redirect=1)

选择空白流水线模板，输入流水线名称并选择`自由模式`，创建流水线。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202004%2F1587377974\_40\_w1359\_h740.png\&is\_redirect=1)

恭喜你，一条最简单的流水线创建完成！ ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202004%2F1587377338\_83\_w448\_h281.png\&is\_redirect=1)

### 1.2 选择构建环境

构建环境分为编译环境（DevNet区域）和无编译环境（IDC区域）。简单来说，依赖构建资源的插件需要运行在编译环境下（如拉代码、bash插件等）；不必要耗费构建资源的插件则在无编译环境下运行（如发送企业微信通知、人工审核插件等）。\
点击`Job 1-1`右侧的+号，选择Linux Job类型添加`Job 2-1`。

#### 1.2.1 编译环境

编译环境的Job需要选择构建资源。以Linux为例，目前蓝盾提供的资源类型有：`Docker on VM`、`Docker on DevCloud`、`单构建机`以及`构建集群`。各种类型具体的区别及使用会在后文_2.3_详细讲解，这里，我们选择公共构建机`Docker on DevCloud`，镜像默认`tlinux_ci`。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1591263566\_59\_w1350\_h759.png\&is\_redirect=1)

#### 1.2.2 无编译环境

无编译环境不依赖构建资源。无编译环境下的插件执行环境处于IDC区域，作业平台相关的部署插件、消息的发送插件、人工审核插件等都在无编译环境下执行。

### 1.3 创建/使用凭证

蓝盾提供凭证托管的服务，凭证类型包括`密码`、`用户名+密码`、`SSH私钥`等等，不同的凭证类型能应用在不同的第三方服务当中。比如，通过http方式拉取工蜂代码，需要用到`用户名密码+私有token`凭证类型；而推送镜像到某个镜像仓库，则可能会用到`用户名+密码`凭证类型。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202005%2F1589970552\_89\_w1275\_h668.png\&is\_redirect=1)![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202005%2F1589970850\_40\_w1004\_h479.png\&is\_redirect=1)

### 1.4 关联代码库

流水线中想通过插件拉取代码，需要先将代码库关联至平台。

#### 1.4.1 关联代码库

进入代码库服务，点击`关联Git代码库`，勾选`Oauth`的拉取方式后，便可以从代码库列表中选择需关联的repo。\
**注意：列表仅会展示当前OAuth鉴权能访问的代码库，且关联人必须是该代码库的成员。建议关联人同时拥有代码库的master权限，以确保流水线状态和webhook钩子能正常回写至工蜂。**\
除了OAuth的拉取方式，还可以通过_1.3_凭证管理中所托管的凭证，来使用ssh或http来拉取代码。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202005%2F1589965199\_30\_w708\_h427.png\&is\_redirect=1)

#### 1.4.2 拉取代码库

代码库关联好后，便可以回到流水线服务，进入你前面建好的流水线内，在_1.2.1_创建好的`Job2-1`下，添加你的第一个插件：`拉取GIT（命令行）`；然后配置好基础信息代码库和分支名称，拉取代码步骤就大功告成了。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1591264522\_97\_w1949\_h1050.png\&is\_redirect=1)![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1591263963\_78\_w1340\_h1099.png\&is\_redirect=1)**注意：不推荐使用的插件平台已经不维护，功能潜在不稳定的问题。如果流水线配置了不推荐使用的插件，请尽快切换至同名新版插件。**

### 1.5 添加构建所需的插件

拉取完代码，便可以开始对代码进行编译构建了。点击`拉取GIT（命令行）`下的+号，搜索`Bash`插件，添加并编辑构建脚本。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1591264265\_69\_w1340\_h572.png\&is\_redirect=1)![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1591265438\_73\_w1338\_h559.png\&is\_redirect=1)

完成基本的代码拉取和编译脚本的配置，就可以执行流水线查看构建结果了。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1591266377\_86\_w1293\_h408.png\&is\_redirect=1)![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1591266467\_6\_w1296\_h400.png\&is\_redirect=1)

点击`Bash`插件查看插件的执行日志。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1591266551\_18\_w1076\_h258.png\&is\_redirect=1)

**注：Bash插件执行是否成功，是通过判断每一条指令的返回值是否为0来判断的。当Bash插件执行的指令返回码非0时，便会终止插件并判定失败。**

### 1.6 归档构件

蓝盾提供制品库（版本仓库），可以让你将ipa、apk、maven、docker image、二进制包等产物归档并分享，结合流水线，会让你的持续交付变得更加简单。\
_1.5_中构建出来的二进制包`iccen_demo`，我们可以将它归档至制品库，便于日后查看或用于流水线中的其他流程：如将该构件拉取至另一个构建环境中、又如将该构件分发至目标部署测试机。 添加`归档构件`插件，将`Bash`插件中打出来的包路径配入`归档构件`插件。\
**注：待归档的文件路径应为`${WORKSPACE}`的相对路径，因为上一插件构建出来的包路径是`${WORKSPACE}/iccen_demo`，所以此处填入iccen\_demo即可** ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1593439841\_16\_w1319\_h696.png\&is\_redirect=1)

查看归档成功日志。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1593526628\_52\_w1271\_h443.png\&is\_redirect=1)

### 1.7 环境管理-导入部署测试机

有构建自然有部署。在导入目标部署机器前，我们先来讲讲`环境管理`服务。\
导入目标部署机器，我们需要使用到蓝盾提供的`环境管理`服务。`环境管理`服务托管用户的构建环境（会在后续章节_2.3.2_详细讲解）以及服务器环境。\
环境由一组节点组成，节点即为构建节点和服务器节点。构建节点用于编译构建；服务器节点用于发布，完成软件的功能验证或性能测试。\
由多个服务器节点组成的服务器环境被用于部署时，可以将完成构建的包同时分发至环境内的所有节点。\
下面讲下导入服务器节点并创建服务器环境的流程。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1593501268\_24\_w1282\_h508.png\&is\_redirect=1)

**注：导入机器人必须是机器的主负责人或唯一备份负责人** ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1593501628\_10\_w906\_h663.png\&is\_redirect=1)

点击[(安装Agent)](https://iwiki.oa.tencent.com/x/WtMrAg)，依照教程完成GSE Agent的安装。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1593525915\_32\_w1271\_h232.png\&is\_redirect=1)![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1593526216\_32\_w940\_h850.png\&is\_redirect=1)

Agent状态上报会有1分钟延迟左右，等待一分钟并手动刷新界面。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202006%2F1593526246\_30\_w1271\_h234.png\&is\_redirect=1)导入部署测试机成功！&#x20;

然后我们在左侧边栏切换至环境页签，点击创建，导入前面的服务器节点。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202007%2F1594299729\_86\_w1356\_h791.png\&is\_redirect=1)点击提交，环境导入完成。

### 1.8 构件的分发部署

有了部署机器，我们可以将构件分发至测试机上了。首先添加一个无编译环境`Job 3-1`，添加插件`作业平台-构件分发`并完成配置。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202007%2F1594299855\_100\_w1355\_h726.png\&is\_redirect=1)

添加`作业平台-脚本执行`，给目标部署文件添加执行权限。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202007%2F1594299888\_3\_w638\_h1226.png\&is\_redirect=1)

执行流水线，查看流水线是否执行成功。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202007%2F1594284785\_62\_w1301\_h462.png\&is\_redirect=1)

登录到部署机器，查看构件是否分发成功。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202007%2F1594285256\_19\_w435\_h120.png\&is\_redirect=1) 部署完成！

### 1.9 流水线执行失败

#### 1.9.1 登录调试

公共构建机提供登录调试的方式，协助你登录到失败的Job进行调试。\
我们把编译指令注释掉来模拟流水线执行失败的场景，执行流水线，并查看报错的日志。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202007%2F1594285989\_30\_w986\_h535.png\&is\_redirect=1)![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202007%2F1594286860\_41\_w1041\_h142.png\&is\_redirect=1)

点击`登录调试`，登录至公共构建机。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202007%2F1594287023\_70\_w1297\_h466.png\&is\_redirect=1)

登录调试会保留你最后一次构建的环境，在里面执行指令协助调试。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202007%2F1594287710\_83\_w1287\_h365.png\&is\_redirect=1)**注：满足以下三个条件才能对失败的构建环境进行登录调试**\
**i: 最新一次构建**\
**ii: Job为失败状态**\
**iii: 构建完成后，流水线没有被编辑过**

#### 1.9.2 重试

当某个插件因网络环境不稳定而导致执行失败时，可以使用rebuild功能。rebuild会保留所有构建的启动参数，在构建环境中重新跑一次流水线（部分构建环境如mac公共构建机、pcg公共构建机会存在母机飘移的情况，遂不建议使用rebuild）。尤其对于代码触发的流水线来说，测试流水线或重跑流水线则不再需要重新push代码或合入代码来触发流水线了。

###
