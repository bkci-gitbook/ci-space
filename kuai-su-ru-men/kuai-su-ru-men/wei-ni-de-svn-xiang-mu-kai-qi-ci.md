# 为你的SVN项目开启CI

### 一、准备事项 <a href="#id-wei-ni-de-svn-xiang-mu-kai-qi-ci-yi-zhun-bei-shi-xiang" id="id-wei-ni-de-svn-xiang-mu-kai-qi-ci-yi-zhun-bei-shi-xiang"></a>

* 一个SVN工程（如没有，请参考[关联你的第一个代码库](http://iwiki.oa.com/pages/viewpage.action?pageId=10718809)）
* 一个蓝盾项目
* 了解蓝盾流水线基本概念和使用（若首次使用蓝盾，请参考[创建你的第一条蓝盾流水线](chuang-jian-ni-de-di-yi-tiao-liu-shui-xian.md)）

### 二、创建 SVN Commit Push 事件触发执行的流水线 <a href="#id-wei-ni-de-svn-xiang-mu-kai-qi-ci-er-chuang-jian-svncommitpush-shi-jian-chu-fa-zhi-hang-de-liu-shu" id="id-wei-ni-de-svn-xiang-mu-kai-qi-ci-er-chuang-jian-svncommitpush-shi-jian-chu-fa-zhi-hang-de-liu-shu"></a>

#### Step 1 创建空白流水线 <a href="#id-wei-ni-de-svn-xiang-mu-kai-qi-cistep1-chuang-jian-kong-bai-liu-shui-xian" id="id-wei-ni-de-svn-xiang-mu-kai-qi-cistep1-chuang-jian-kong-bai-liu-shui-xian"></a>

![](<../../.gitbook/assets/image (9) (1) (1).png>)

#### Step 2  添加触发器：SVN事件触发 <a href="#id-wei-ni-de-svn-xiang-mu-kai-qi-cistep2-tian-jia-chu-fa-qi-svn-shi-jian-chu-fa" id="id-wei-ni-de-svn-xiang-mu-kai-qi-cistep2-tian-jia-chu-fa-qi-svn-shi-jian-chu-fa"></a>

1. 在1-1 构建触发 Job 添加触发器：SVN事件触发
2. 填写触发器参数：
   1. \[必填] 选择代码库为你已关联到蓝盾的、待监听的SVN代码库
   2. \[必填] 选择事件类型为 Commit Push Hook
   3. 可添加相对路径、需排除的路径、人员等过滤条件

![](<../../.gitbook/assets/image (7) (1).png>)

#### Step 3 添加Job，用来执行具体的构建任务 <a href="#id-wei-ni-de-svn-xiang-mu-kai-qi-cistep3-tian-jia-job-yong-lai-zhi-hang-ju-ti-de-gou-jian-ren-wu" id="id-wei-ni-de-svn-xiang-mu-kai-qi-cistep3-tian-jia-job-yong-lai-zhi-hang-ju-ti-de-gou-jian-ren-wu"></a>

![](<../../.gitbook/assets/image (19) (1) (1).png>)

#### Step 4 在Job下添加拉取代码插件：[拉取SVN（命令行）](http://devops.oa.com/console/store/atomStore/detail/atom/svnCodeRepo) <a href="#id-wei-ni-de-svn-xiang-mu-kai-qi-cistep4-zai-job-xia-tian-jia-la-qu-dai-ma-cha-jian-la-qu-svn-ming-l" id="id-wei-ni-de-svn-xiang-mu-kai-qi-cistep4-zai-job-xia-tian-jia-la-qu-dai-ma-cha-jian-la-qu-svn-ming-l"></a>

1. 填写参数
   1. \[必填] 选择代码库为在Step 2中监听的代码库，指定拉取方式、分支名称等信息
   2. \[必填] 选择拉取策略为Fresh Checkout、Revert Update 或者 Increment Update
   3. \[必填]支持配置拉取深度
   4. 其他丰富的配置项请参考插件说明

![](<../../.gitbook/assets/image (20) (1) (1).png>)

#### Step 5 添加执行构建命令的插件，如：[Bash](http://devops.oa.com/console/store/atomStore/detail/atom/linuxScript) <a href="#id-wei-ni-de-svn-xiang-mu-kai-qi-cistep5-tian-jia-zhi-hang-gou-jian-ming-ling-de-cha-jian-ru-bash" id="id-wei-ni-de-svn-xiang-mu-kai-qi-cistep5-tian-jia-zhi-hang-gou-jian-ming-ling-de-cha-jian-ru-bash"></a>

执行构建命令，将拉取最新代码进行构建

![](<../../.gitbook/assets/image (13) (1) (1).png>)

#### Step 6 添加归档构件插件，将构建产物归档到仓库，如：[归档构件](http://devops.oa.com/console/store/atomStore/detail/atom/UploadArtifactory) <a href="#id-wei-ni-de-svn-xiang-mu-kai-qi-cistep6-tian-jia-gui-dang-gou-jian-cha-jian-jiang-gou-jian-chan-wu" id="id-wei-ni-de-svn-xiang-mu-kai-qi-cistep6-tian-jia-gui-dang-gou-jian-cha-jian-jiang-gou-jian-chan-wu"></a>

![](<../../.gitbook/assets/image (15) (1) (1).png>)
