# 一小时带你玩转蓝盾DevOps - 2

## 第二章：进阶使用 <a href="#e7-ac-ac-e4-ba-8c-e7-ab-a0-ef-bc-9a-e8-bf-9b-e9-98-b6-e4-bd-bf-e7-94-a8" id="e7-ac-ac-e4-ba-8c-e7-ab-a0-ef-bc-9a-e8-bf-9b-e9-98-b6-e4-bd-bf-e7-94-a8"></a>

### 2.1 配置与传递流水线变量

在流水线执行的过程中，流水线变量可以将上游预定义的数据或插件输出的信息往下游步骤传递。 流水线变量有三种定义方式： \
**i. 在流水线编排预设置** \
**ii. Bash/Batch插件内设置** \
**iii. 插件输出** \


#### 2.1.1 预设变量

点击`Job1-1`打开触发变量配置界面，点击新增变量开始配置流水线变量。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600069979\_71\_w1351\_h519.png\&is\_redirect=1)\
这里可以定义不同的变量类型以及变量的默认值。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202008%2F1598534077\_87\_w641\_h1009.png\&is\_redirect=1) \
手动触发时，可根据具体需求，手动更改变量的值。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202008%2F1598534016\_87\_w1339\_h743.png\&is\_redirect=1)

#### 2.1.2 插件定义变量

`Bash`/`Batch`插件可以通过插件预设的函数`setEnv`对变量进行设置（此处以`Bash`插件为例）。可对已在前文设置的变量的值进行覆盖。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202008%2F1598534306\_47\_w639\_h547.png\&is\_redirect=1) \
**p.s.** 你还可以通过`作业平台-脚本执行`插件设置流水线变量，详情请戳：[如何通过【作业平台-脚本执行】设置流水线变量](https://iwiki.woa.com/x/lxALAQ)

#### 2.1.3 插件输出

部分插件执行完成后会输出流水线变量，供下游插件使用。如： \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599622032\_21\_w637\_h814.png\&is\_redirect=1) \
**p.s.** 蓝盾官方也有提供预定义的变量供不同业务场景使用。详情请戳：[预定义变量列表](https://iwiki.woa.com/x/HxqbAQ) 和 [蓝盾代码库变量合集](https://iwiki.woa.com/x/Y4Dm)

#### 2.1.4 引用变量

上述两种方式在变量设置完成后，均可在后续插件的表单中通过`${{变量名}}`引用。 \
如下图，在插件实际执行时，表单的配置会被替换为：`./iccen_artifact` \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202108%2F1629363286\_13\_w639\_h359.png\&is\_redirect=1) \


**注：** \
**1. 这里简单讲下流水线变量引用背后的逻辑。插件在执行前，会先获取所有插件入参，并提取入参中的`字符串${{variable}}`，对`${{variable}}`进行值替换（如`Bash`插件的脚本内容即为插件的入参），尔后再开始执行插件。** \
**2. 流水线构建列表有备注字段，可以通过在`Bash`插件中，执行`setEnv "BK_CI_BUILD_REMARK" "备注信息"`来动态设置备注。** \
**3. 蓝盾流水线变量值最长存储4000字节内容。如果超出，该变量将被丢弃。** \
如下面的例子： \
\#171次构建中，用于设置流水线变量的插件 的配置与执行日志。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600056783\_10\_w2472\_h1016.png\&is\_redirect=1)\
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600056891\_100\_w1551\_h738.png\&is\_redirect=1)\
\#171次构建中，用于打印流水线变量的插件 的配置与执行日志。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600056965\_20\_w1553\_h745.png\&is\_redirect=1)\
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600057022\_45\_w2203\_h738.png\&is\_redirect=1)

### 2.2 流水线的触发方式

#### 2.2.1 手动触发

顾名思义，手动触发允许手动执行流水线。【第一章】中启动流水线的方式即为手动触发。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599135994\_74\_w1351\_h485.png\&is\_redirect=1)\


手动触发支持两项配置： \
**i. 可跳过插件（默认勾选）** \
开启后，手动执行时会进入执行预览界面，在这里可以禁用你在本次执行的构建中，不想运行的插件。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599136476\_17\_w1350\_h850.png\&is\_redirect=1)\
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599137215\_54\_w1350\_h621.png\&is\_redirect=1)\


**ii. 使用最近一次构建参数值** \
开启后，下一次构建的执行预览界面中，流水线变量不再展示默认值，而是取本次构建填入的参数。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599137359\_28\_w1350\_h851.png\&is\_redirect=1)\


#### 2.2.2 定时触发

顾名思义，定时触发可以配置固定时间触发流水线构建。 高级配置支持crontab表达式，让你的定时触发更灵活。 ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599137712\_88\_w1349\_h766.png\&is\_redirect=1)

#### 2.2.3 代码库事件触发

此处以`Git事件触发`为例，事件类型包括四种： \
**i. Push触发：** 监听的代码库有代码提交 \
**ii. Tag Push触发：** 监听的代码库新增/删除Tag \
**iii. Merge Request触发：** 监听的代码库请求代码合入 \
**iv. Merge Request Accept触发：** 监听的代码库完成代码合入 \


**p.s.** 代码库事件触发的构建流程中通常会用到_2.1.3_提到的 [代码库变量](https://iwiki.woa.com/x/Y4Dm)，如拉取代码插件的入参。 \
**注：** 关联代码库的同学需拥有代码库的master权限，才能确保webhook成功回写至工蜂。 \


代码库事件触发的入参配置比较直观，这里不作赘述。可以提下的是，Merge Request触发支持锁定提交。勾选后，流水线状态会回写至工蜂。当流水线执行失败，工蜂的MR会被阻止。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599625907\_86\_w1351\_h1102.png\&is\_redirect=1)\


提交MR后，流水线执行失败，状态回写至工蜂，MR会被阻止。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599625844\_37\_w1343\_h543.png\&is\_redirect=1)\
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599625758\_64\_w1334\_h1012.png\&is\_redirect=1)

#### 2.2.4 远程触发

远程触发与API触发类似，可以在**IDC区域**的机器上触发流水线。 \
复制远程触发指令，在目标机器上执行即可触发对应流水线。body参数包含_2.1.1_的预设流水线变量，我们可以通过更改body参数中变量的值，来传递流水线实际执行的变量值。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599570651\_61\_w1352\_h631.png\&is\_redirect=1)\


另外，如果需要在DevNet机器上远程触发，需要将url中的`devops.oa.com`更改为`devgw.devops.oa.com`。 \
此处以DevNet机器上远程触发为例，将`iccen_key`的值覆盖为`iccen_remote_value`，将`iccen_key2`的值覆盖为`iccen_value_B`。执行指令后即触发对应流水线。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599571467\_65\_w1096\_h108.png\&is\_redirect=1)\
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599571433\_93\_w1731\_h503.png\&is\_redirect=1)\
**注：** \
**1. 远程触发的body参数会随着流水线预设变量变化而变化。所以在修改预设流水线变量之后，需要重新组装远程触发的body参数，或者在流水线里重新获取远程触发的curl指令。** \
**2. 由于远程触发没有用户态，远程触发会以流水线最后编辑人作为启动人。** \
**3. 远程触发不支持代码层面并发触发，流水线启动有互斥锁。**

### 2.3 使用适合自己的构建环境

根据你的业务需求不同，蓝盾可选择的构建环境支持Linux、MacOs、Windows三种。 \
其中，Linux环境蓝盾提供不同的公共构建资源，这里用表格来对比各自的差异： \


|                    | Docker on VM | Docker on DevCloud | Docker on GITCI  | PCG公共构建资源 |
| ------------------ | ------------ | ------------------ | ---------------- | --------- |
| 所在网络区域             | Devnet       | Devnet             | Devnet           | Devnet    |
| 调度机制               | 基于蓝盾实现       | 基于K8s              | 基于K8s            | 基于K8s     |
| 资源限制               | 无限制，容器间互相竞争  | 资源隔离               | 资源隔离             | 资源隔离      |
| 母机依赖               | 依赖           | 无依赖                | 无依赖              | 无依赖       |
| 蓝盾编译加速             | 支持           | 支持                 | 支持               | 支持        |
| 编译环境依赖             | 挂载+镜像支持      | 挂载+镜像支持            | 镜像支持             | 镜像支持      |
| 运行docker build/run | 支持           | 支持                 | 支持               | 不支持       |
| WebConsole登录调试     | 支持           | 支持                 | 不支持              | 不支持       |
| 同一流水线并发构建          | workspace冲突  | 支持                 | 支持               | 支持        |
| 外网代理               | 默认开启         | 默认开启               | 默认开启             | 默认开启      |
| Workspace缓存        | 缓存母机         | 缓存CBS盘             | 缓存CBS盘           | 无缓存       |
| 插件支持               | 几乎所有蓝盾插件     | 几乎所有蓝盾插件           | Yaml schema指定的插件 | 几乎所有蓝盾插件  |
| 镜像仓库支持             | 蓝盾仓库/其他仓库    | 蓝盾仓库/其他仓库          | 蓝盾仓库/其他仓库        | Mita镜像仓库  |

当公共构建资源不能满足你的业务需求时，可以使用以下两种方式自定义构建环境： \
**i. 制作自己的CI镜像** \
**ii. 导入私有构建机** \


#### 2.3.1 自己的CI镜像

点击 [制作CI镜像指引](https://iwiki.oa.tencent.com/x/Lozm) 完成镜像的制作与推送。 \
软件源上创建命名空间和镜像仓库。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599212513\_85\_w1315\_h505.png\&is\_redirect=1)\


编辑Dockerfile，制作镜像并推送至软件源。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599213528\_3\_w1063\_h186.png\&is\_redirect=1)\
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599213589\_84\_w1599\_h557.png\&is\_redirect=1)\


并依照 [镜像发布指引](https://iwiki.woa.com/x/QYFRAQ) 完成容器镜像的发布。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599214861\_72\_w1619\_h461.png\&is\_redirect=1)\


镜像发布完成后就可以在你的构建资源里选择使用了。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599215250\_30\_w1975\_h819.png\&is\_redirect=1)

#### 2.3.2 环境管理-导入构建机

前文_1.7_已经提到部署环境的创建。相似的流程，导入构建机也需要进入`环境管理`服务。选择导入第三方构建机。 \
**注：构建环境处于DevNet区域。建议仅导入DevNet区域的机器，避免公司网络策略导致的网络问题。** \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599450097\_77\_w1385\_h267.png\&is\_redirect=1)\


根据你的构建需求，选择对应的操作系统。此处以Linux为例。 \
地点的选择遵循地区就近原则，可提高文件传输速度，增加机器连接的稳定性。如北京的同学请选择天津。 \
然后复制agent安装指令。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599450223\_5\_w643\_h458.png\&is\_redirect=1) \


登录构建机，创建一个路径作为agent安装目录，如：`/data/iccendemo`。进入安装目录并执行页面所复制的指令。 \
安装完成后，执行`ps -ef | grep devops`确保agent已正常启动。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599452532\_88\_w1095\_h588.png\&is\_redirect=1)\
**注：** \
**1. 蓝盾网关部署在DevNet，agent访问网关的时候走直连模式，不能走代理。如果机器配置了代理，所以需要将蓝盾网关置入环境变量no\_proxy内。** \
**2. 机器上的环境变量更新后，需重启agent才能将更新后的环境变量同步至流水线上的构建资源。执行`./stop.sh`和`./start.sh`完成agent重启。** \


确认agent启动后，刷新节点，并导入。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599556307\_99\_w642\_h454.png\&is\_redirect=1) \


Linux构建资源便可以选择你导入的构建机器了。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599556698\_67\_w1350\_h761.png\&is\_redirect=1)\


**p.s.** Windows机器导入流程与Linux不同，详情请参照指引：[构建Agent导入-Windows版](https://iwiki.woa.com/x/ZNMrAg) \


和导入_1.7_导入部署测试机类似，我们可以创建构建机环境，即构建集群。 \
切换至环境页签，新增环境，环境类型选择构建环境并导入构建机。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600072014\_95\_w1350\_h804.png\&is\_redirect=1)\
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599557008\_41\_w1351\_h272.png\&is\_redirect=1)\


Linux构建资源便可以选择你新增的构建集群了。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599557094\_11\_w1352\_h755.png\&is\_redirect=1)\
构建集群的调度遵循以下规则： \


1. 最高优先级的构建机:
   1. 最近构建机中使用过这个构建机
   2. 当前没有任何构建机任务
2. 次高优先级的构建机:
   1. 最近构建机中使用过这个构建机
   2. 当前有构建任务，但是构建任务数量没有达到当前构建机的最大并发数
3. 第三优先级的构建机:
   1. 当前没有任何构建机任务
4. 第四优先级的构建机:
   1. 当前有构建任务，但是构建任务数量没有达到当前构建机的最大并发数
5. 最低优先级：
   1. 没有满足以上条件的构建机

### 2.4 流程控制

Stage、Job、Task都能配置流程控制，让你的流水线有更多的玩法。以下以插件的流程控制为例： \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599140012\_37\_w577\_h591.png\&is\_redirect=1) \


**i. 启用本插件** \
功能与_2.2.1_中的可跳过插件类似。关闭后，插件不会再在构建中运行。在手动执行的预览界面也无法开启。 \
**ii. 失败时继续** \
当插件执行失败时，忽略失败状态（插件的状态表现上仍会标红），构建流程继续往下走。如果同时开启了”**失败时自动重试**“，流水线会优先尝试重试，当所有重试都失败后构建流程才会继续。 \
**iii. 失败时自动重试** \
可配置插件的重试次数n，在插件执行失败后，会自动重新执行插件n次。 \
**iv. 插件执行超时时间** \
当插件执行的时间超过配置时长，插件自动失败。 \
**v. 何时运行本插件** \


* 所有前置插件运行成功时运行：默认选项。同一Job下，前面的插件全部执行成功时才运行。如果前置插件配置了失败时继续，当前插件也会运行。
* 即使前面有插件运行失败也运行，除非被取消才不运行：同一Job下，除非流水线被终止，否则无论如何都运行。
* 只有前面有插件运行失败时才运行：同一Job下，只有前面有插件失败了才运行。
* 自定义变量全部满足时才运行：同一Job下，配置的所有变量值同时满足才运行。注意变量间是"and"关系。\
  ![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599208798\_47\_w639\_h852.png\&is\_redirect=1)
* 自定义变量全部满足时不运行：类似"**自定义变量全部满足时才运行**"，当配置的所有变量值同时满足时，当前插件不运行。

### 2.5 互斥组

互斥组，顾名思义就是一个组内所有Job单位成员不能并发执行。互斥组的设计初衷是为了解决并发构建时，使用同一构建机资源所产生的冲突问题。对同一项目内不同流水线的不同Job也可以设置为同一个互斥组。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599559723\_3\_w1352\_h1072.png\&is\_redirect=1)\


启用互斥组排队可以将并发执行的Job加入到队列当中。等当前Job执行完毕，即可启动队列首位的Job。 \
下图为`Job 2-3`的执行日志，红框为互斥组排队的日志输出。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599559854\_66\_w1351\_h771.png\&is\_redirect=1)

### 2.6 人工审核

CI/CD流程会存在需要人工介入的场景，如：需负责人确认是否可以执行后续操作；需负责人确认流水线的控制参数是否正确等。 流水线目前提供两种流程审核的方式：

#### 2.6.1 人工审核插件

无编译环境下添加`人工审核`插件，配置审核人。 \
**p.s.** `人工审核`插件只能在无编译环境下使用。如果支持构建环境，会造成不必要的构建资源开销。审核时长最长等待7天，超过7天流水线会视为超时结束。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599639616\_9\_w1709\_h848.png\&is\_redirect=1)\


流水线触发后，流水线处于执行中状态，流程卡在`人工审核`插件。负责人会收到审核通知，并需在流水线上对其进行同意或驳回。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599640785\_17\_w433\_h180.png\&is\_redirect=1) \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599640645\_91\_w1286\_h577.png\&is\_redirect=1)\


审核完成后，可查看审核日志。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599640728\_11\_w1285\_h571.png\&is\_redirect=1)

#### 2.6.2 Stage准入（推荐）

点击Stage的准入icon，进入Stage准入配置页面，配置审核人。 \
**p.s.** Stage的审核最长可等待60天。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599641238\_58\_w1709\_h576.png\&is\_redirect=1)\


流水线触发后，流水线处于Stage Success状态。负责人需在流水线上对Stage进行审核：“继续执行” / “取消执行，立即标记为Stage成功状态”。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599641364\_95\_w1712\_h574.png\&is\_redirect=1)\
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599641895\_70\_w1708\_h571.png\&is\_redirect=1)

### 2.7 子流水线的调用

部分业务场景需要在流水线执行过程中，唤起另一条流水线。蓝盾官方提供的`子流水线调用`插件可满足这一需求。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599644059\_65\_w1352\_h1161.png\&is\_redirect=1)\


**i. 运行方式**  \
同步：插件调起子流水线后，轮询获取子流水线的执行状态，直到子流水线执行结束且成功，父流水线才会开始后续步骤。 \
异步：插件调起子流水线后，父流水线直接开始后续步骤。 \
**ii. 子流水线参数** \
插件会自动同步子流水线的预设流水线变量，可以在此处变更实际启动子流水线的变量值。 \
**iii. 子流水线输出变量命名空间** \
相当于变量前缀，用于解决子流水线输出变量的变量名冲突问题。 \
**iv. 子流水线输出变量** \
需要在父流水线上引用的 子流水线输出变量，多个变量用逗号分隔。通过`${{命名空间+下划线+输出变量}}`来完成变量引用。如下图（其中`devops_key`是子流水线的`Bash`插件中通过`setEnv`定义的）： \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202108%2F1629363791\_43\_w1352\_h688.png\&is\_redirect=1)\
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599647202\_29\_w1350\_h568.png\&is\_redirect=1)\
**v. 轮询间隔** \
轮询子流水线执行状态的时间间隔。 \


**注：使用`子流水线调用`插件，需确保父流水线的最后编辑人拥有子流水线的执行权限。子流水线的执行权限与子流水线的最后编辑人权限无关。**

### 2.8 流水线通用设置

除了流水线的编排以外，还可以对流水线属性进行设置：

#### 2.8.1 通知

流水线构建成功/失败均支持通知发送。通知页签下可以配置通知方式、通知接收人以及通知内容。这里的配置项可以引用流水线变量。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599652047\_44\_w1281\_h709.png\&is\_redirect=1)\


部分业务场景需要发送通知至企业微信群，可以通过启用群通知来实现。 \
其中群ID可通过在企业微信群中手动@CI-Notice并输入关键字"会话id"获取。 \
配置完成后，流水线的构建结果会发送至群聊。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599797220\_32\_w603\_h460.png\&is\_redirect=1)

#### 2.8.2 权限

流水线的操作支持权限控制。 \
给用户添加权限前，需先进入`权限中心`服务，将目标人员添加进蓝盾项目，并分配入权限组。 \
**注：** 如果分配入管理员/CI管理员组，会默认拥有所有流水线操作权限。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599812880\_34\_w1352\_h761.png\&is\_redirect=1)\


回到流水线编辑页面，进入权限页签。里面有三种类型的角色，他们权限大小为：拥有者 > 执行者 > 查看者。每一个角色的用户项可以配置项目成员，用户组项则可以配置上文提到的权限组。配置保存后，权限组内的所有成员均会生效对应的操作权限。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599814697\_3\_w1350\_h916.png\&is\_redirect=1)\


如果执行者、拥有者、查看者的权限组合不能满足你们业务场景，则可以按功能给项目成员分配权限： \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599815197\_90\_w1351\_h1065.png\&is\_redirect=1)

#### 2.8.3 基础设置

基础设置页签可以修改流水线名称以及配置流水线描述。流水线构建的执行状态徽章可以通过下图的链接获取。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599817238\_62\_w1352\_h553.png\&is\_redirect=1)\


运行锁定中重点讲下**可同时运行多个构建任务**和**同一时间最多只能运行一个构建任务**两个执行策略。 \


新建的流水线默认选择**可同时运行多个构建任务**策略。但部分场景下业务流水线可能不能支持多个构建任务同时执行，如流水线的构建资源没有作资源隔离（如私有构建机），并发运行的构建任务可能会对机器构建目录同时写入，破坏构建环境。这种情况下选择**同一时间最多只能运行一个构建任务**策略可以解决。 \
该策略可以配置最大排队数量（即队列长度）：队列满的情况下，队首的构建任务会被抛弃掉，被抛弃的构建任务状态设置为`用户取消`。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599820666\_50\_w1352\_h359.png\&is\_redirect=1)\
如下图，队列长度设置为2，#134 开始执行后，#135、#136、#137 相继触发。#135 和 #136 在队列中时，#137 触发，将队首的 #135 抛弃，#137 进入排队队列。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599820594\_22\_w1343\_h502.png\&is\_redirect=1)\


同时，该策略还可以配置最大排队时长：队列中等待时长超过最大排队时长的构建任务会直接判定执行失败，状态设置为`排队超时`。如下图的 #136 和 #137。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1599820624\_60\_w1343\_h502.png\&is\_redirect=1)

### 2.9 流水线模板

当团队中需要配置很多一模一样编排的流水线，流水线间的区别又仅仅是部分参数不同时，我们可以通过流水线模板功能来解决重复创建流水线的问题。 \
流水线模板可以一键实例化多条相同的流水线，并支持一键更新多条实例。不同的参数可使用_2.1.1_的自定义变量来控制。 \
**注：通过模板实例化的流水线，编排不允许在实例中修改。**

#### 2.9.1 创建流水线模板

编辑流水线，对需要用变量控制的插件入参，配置流水线变量。如： \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202108%2F1629363991\_85\_w1350\_h759.png\&is\_redirect=1)\


并在`Job1-1`处完成流水线变量的配置。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600140443\_18\_w1351\_h1105.png\&is\_redirect=1)\


完成编辑后，将流水线另存为模板。 \
若`应用设置`勾选了`是`，流水线会将_2.8_提及到的通用设置同步至模板。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600088968\_82\_w1351\_h524.png\&is\_redirect=1)\


回到流水线列表界面，进入模板管理，查看成功创建的模板。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600089123\_41\_w1351\_h436.png\&is\_redirect=1)\
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600159029\_34\_w1351\_h375.png\&is\_redirect=1)

#### 2.9.2 实例化流水线

点进模板名称进入模板配置页面。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600089205\_57\_w1351\_h477.png\&is\_redirect=1)\


切换至设置页签可对模板的通用设置统一配置。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600089351\_88\_w1351\_h1038.png\&is\_redirect=1)\


切换至实例管理页签，进行实例化流水线。 \
首先选择模板版本，点击`+新增流水线实例`，并输入流水线实例名称。一次实例化操作可新增多个流水线实例。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600089427\_49\_w1350\_h459.png\&is\_redirect=1)\


修改各个流水线实例当中用于控制流水线行为的变量默认值，并实例化。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600159485\_24\_w1351\_h684.png\&is\_redirect=1)\


完成实例化。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600159521\_34\_w1351\_h435.png\&is\_redirect=1)\


回到流水线列表，模板实例化出来的流水线名称前会带有模字icon。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600159568\_100\_w1351\_h664.png\&is\_redirect=1)\


#### 2.9.3 更新模板和流水线实例

回到模板管理的编辑页签，可以对模板编排进行修改。保存时需指定模板版本。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600159689\_8\_w1351\_h655.png\&is\_redirect=1)\


模板完成更新后，对需要更新至最新版本的流水线实例完成批量更新。 \
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600159775\_49\_w1351\_h437.png\&is\_redirect=1)\
![](https://km.woa.com/gkm/api/img/cos-file-url?url=https%3A%2F%2Fkm-pro-1258638997.cos.ap-guangzhou.myqcloud.com%2Ffiles%2Fphotos%2Fpictures%2F202009%2F1600159838\_83\_w1351\_h686.png\&is\_redirect=1)

### 小语
