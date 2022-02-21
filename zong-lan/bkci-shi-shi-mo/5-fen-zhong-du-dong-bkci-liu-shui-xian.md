# 5分钟读懂 BKCI 流水线



蓝盾提供了可视化的流水线编排页面，里面包含了**S**tage、**J**ob、**T**ask结构，阅读本文有助于你快速上手流水线。

我们先用一张图来了解下流水线中的相关概念：

![](<../../.gitbook/assets/image (24).png>)

### Task（插件） <a href="#id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-task-cha-jian" id="id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-task-cha-jian"></a>

通常是一个单独的任务，如拉取Git仓库代码等，所以也称之为插件。

![](<../../.gitbook/assets/image (26).png>)

### Job（作业） <a href="#id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-job-zuo-ye" id="id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-job-zuo-ye"></a>

作业，可以运行在一个构建环境里，比如运行在macOS；也可以作为不需要构建环境的普通任务调度编排。它有如下特性：

* 由多个Tasks(插件)组成；
* 一个Task失败，则该Job失败，其余Task将不会运行；

![](<../../.gitbook/assets/image (14).png>)

> 蓝盾可以自动生成macOS、Windows、Linux构建环境，同时也支持项目自己的物理构建机。

### Stage（阶段） <a href="#id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-stage-jie-duan" id="id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-stage-jie-duan"></a>

* 由多个Jobs（作业）组成；
* 同一个Stage下的Job执行方式为并行，由于Job之间是相互独立的，某个Job失败后，其它的Job会被运行到完成；
* 一个Job失败，则该Stage失败。

![](<../../.gitbook/assets/image (30).png>)

### Pipeline(流水线) <a href="#id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-pipeline-liu-shui-xian" id="id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-pipeline-liu-shui-xian"></a>

* 由多个Stages（阶段）组合而成；
* Stage执行方式为顺序执行，一个Stage失败，将不会执行后续Stage；
* 一个Stage失败，则该Pipeline失败。

### Materials and Triggers(材料和触发) <a href="#id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-materialsandtriggers-cai-liao-he-chu-fa" id="id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-materialsandtriggers-cai-liao-he-chu-fa"></a>

#### 材料 <a href="#id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-cai-liao" id="id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-cai-liao"></a>

流水线执行就像做饭一样，需要材料（米），有了材料，才能做出可口的饭菜。向流水线中添加了拉取代码库的相关插件后（[Git](https://w.oa.com/%E7%94%A8%E6%88%B7%E6%8C%87%E5%8D%97/%E6%8F%92%E4%BB%B6%E5%88%97%E8%A1%A8/git.md)、[SVN](https://w.oa.com/%E7%94%A8%E6%88%B7%E6%8C%87%E5%8D%97/%E6%8F%92%E4%BB%B6%E5%88%97%E8%A1%A8/svn.md)），这个流水线就有了**材料**。

#### 触发 <a href="#id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-chu-fa" id="id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-chu-fa"></a>

流水线的触发方式大致可分为4类：手动触发、定时触发、Commit触发、制品库变化时触发。

* 手动触发

你可以通过页面的流水线执行按钮直接触发一次构建。

* 定时触发

设置周期性的计划任务，后台将在规定的时间**自动触发**构建。

* Commit触发

流水线服务的后台可以轮询检测材料的Commit事件，用以**自动触发**流水线，同时，你也可以手动触发。 Pipeline可以有多个材料，任何一个材料变更都可以触发Pipeline，甚至是跟当前流水线没有任何关联。

### 其它 <a href="#id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-qi-ta" id="id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-qi-ta"></a>

#### 产出物 <a href="#id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-chan-chu-wu" id="id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-chan-chu-wu"></a>

执行一次流水线构建，会有很多产出物出现，我们按照下面的维度进行了分类：

* 构件（Artifact）

顾名思义，就是编译或打包之后产生的二进制文件，包括镜像、版本压缩包、IPA包、APK包等等，利用插件你可以将构件归档到指定的仓库中。

* 代码检查报告

如果你的流水线中包含了CodeCC代码检查任务插件，那么你的流水线就会多出这样一个代码检查报告页面，该页面数据会随着你的流水线构建执行而不断刷新。

* 测试报告
* 安装程序
* 版本日志
* 文档

如果想在不同Job之间共享产出物，需要用到【归档构件】和【拉取构件】等插件。

#### WORKSPACE <a href="#id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-workspace" id="id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-workspace"></a>

在构建机上工作目录，所有与构建机相关的插件执行时的相对目录，在如下这些需要路径插件中，需要的也是WORKSPACE下的相对路径。

#### 节点 <a href="#id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-jie-dian" id="id5-fen-zhong-du-dong-lan-dun-liu-shui-xian-jie-dian"></a>

也称之为构建机，为了编译、测试或部署你的代码，你需要将至少1台Agent节点导入至蓝盾，而随着团队规模的增长，这个数字将不断扩充。 根据节点类型的不同，我们的任务也分为两类：

* 直接运行在节点上

你可以在添加Job时直接选择导入到蓝盾的第三方构建机（包含macOS、Windows、Linux），流水线会在运行到对应Job时将任务分配到该构建机上。

* 运行在节点的docker内

当你的节点操作系统是Linux时，Job详情页的**构建资源类型**会多出一个选项：Linux构建镜像。选择这个选项，流水线会在运行到该Job后，通过以下方式来充分利用你的节点CPU、MEM等动态资源：

1. 在分配到该Job的节点上运行docker run命令，将对应镜像启动后
2. 把WORKSPACE挂载至docker内来进行编译构建
3. Job运行结束后，销毁该容器，WORKSPACE保留
