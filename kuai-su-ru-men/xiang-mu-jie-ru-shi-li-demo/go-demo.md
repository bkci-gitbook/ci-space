# Go Demo

## **准备材料：** <a href="#godemo-zhun-bei-cai-liao" id="godemo-zhun-bei-cai-liao"></a>

#### 1. 一个可以成功 go build 的 Git 工程：[https://git.code.oa.com/viky-personal/gomod-git.code.oa.com-github.com-test](https://git.code.oa.com/viky-personal/gomod-git.code.oa.com-github.com-test) <a href="#godemo1.-yi-ge-ke-yi-cheng-gong-gobuild-de-git-gong-cheng-httpsgit.code.oa.comvikypersonalgomodgit.c" id="godemo1.-yi-ge-ke-yi-cheng-gong-gobuild-de-git-gong-cheng-httpsgit.code.oa.comvikypersonalgomodgit.c"></a>

## **详细步骤:** <a href="#godemo-xiang-xi-bu-zhou" id="godemo-xiang-xi-bu-zhou"></a>

#### **1. 将代码库以SSH方式（使用go mod时需要）关联至蓝盾，**[**请参考**](http://iwiki.oa.com/pages/viewpage.action?pageId=10718809) <a href="#godemo1.-jiang-dai-ma-ku-yi-ssh-fang-shi-shi-yong-gomod-shi-xu-yao-guan-lian-zhi-lan-dun-qing-can-ka" id="godemo1.-jiang-dai-ma-ku-yi-ssh-fang-shi-shi-yong-gomod-shi-xu-yao-guan-lian-zhi-lan-dun-qing-can-ka"></a>

仓库必须是SSH方式，不是加载的mod，模块会继承仓库的SSH。git+ssh凭证配置方式，[请参考](https://iwiki.oa.tencent.com/x/UfW3CQ)

#### **2. 创建一条空流水线，**[**请参考**](http://iwiki.oa.com/pages/viewpage.action?pageId=10718801) <a href="#godemo2.-chuang-jian-yi-tiao-kong-liu-shui-xian-qing-can-kao" id="godemo2.-chuang-jian-yi-tiao-kong-liu-shui-xian-qing-can-kao"></a>

#### **3. 添加2-1的** [**Linux Job**](https://iwiki.woa.com/display/DevOps/Job)**，选择 “tlinux\_ci” docker 镜像** <a href="#godemo3.-tian-jia-21-de-linuxjob-xuan-ze-tlinuxcidocker-jing-xiang" id="godemo3.-tian-jia-21-de-linuxjob-xuan-ze-tlinuxcidocker-jing-xiang"></a>

* 请确保使用了[tlinux\_ci](http://devops.oa.com/console/store/atomStore/detail/image/tlinux\_ci)镜像，该镜像内已内置了oa.com的证书，可以执行go mod等操作
* 请不要选择页面上自带的“golang”环境

![](<../../.gitbook/assets/image (17) (1).png>)

#### **4. 将密钥通过bash插件写入本地，**[**请参考**](https://iwiki.oa.tencent.com/x/9gZSBQ) <a href="#godemo4.-jiang-mi-yao-tong-guo-bash-cha-jian-xie-ru-ben-di-qing-can-kao" id="godemo4.-jiang-mi-yao-tong-guo-bash-cha-jian-xie-ru-ben-di-qing-can-kao"></a>

写入的凭证必须是不带私有token的ssh类型（和第一步拉取代码的凭证不同）

#### **5. 在 Job 内以此添加如下插件：** <a href="#godemo5.-zai-job-nei-yi-ci-tian-jia-ru-xia-cha-jian" id="godemo5.-zai-job-nei-yi-ci-tian-jia-ru-xia-cha-jian"></a>

* [拉取Git（命令行）](http://devops.oa.com/console/store/atomStore/detail/atom/gitCodeRepo)：将 go 工程（如有用到go mod，请务必确保以ssh方式关联）拉取到 Linux Job 内

![](<../../.gitbook/assets/image (4).png>)

* [Bash](http://devops.oa.com/console/store/atomStore/detail/atom/linuxScript)：go mod download

![](<../../.gitbook/assets/image (11) (1).png>)

* [Bash](http://devops.oa.com/console/store/atomStore/detail/atom/linuxScript)：go run main.go

![](<../../.gitbook/assets/image (9) (1).png>)

* [Bash](http://devops.oa.com/console/store/atomStore/detail/atom/linuxScript)：go build

![](<../../.gitbook/assets/image (8) (1).png>)

* [归档构件](http://devops.oa.com/console/store/atomStore/detail/atom/UploadArtifactory)：将编译产物上传至[蓝盾制品库](https://iwiki.woa.com/pages/viewpage.action?pageId=17503979)，这里demo里的产物是"[gomod-git.code.oa.com-github.com](http://gomod-git.code.oa.com-github.com)-test"

![](<../../.gitbook/assets/image (13) (1).png>)

#### **6. 运行流水线，观察结果，并查看上传的构件** <a href="#godemo6.-yun-hang-liu-shui-xian-guan-cha-jie-guo-bing-cha-kan-shang-chuan-de-gou-jian" id="godemo6.-yun-hang-liu-shui-xian-guan-cha-jie-guo-bing-cha-kan-shang-chuan-de-gou-jian"></a>

![](<../../.gitbook/assets/image (2).png>)

![](<../../.gitbook/assets/image (12) (1) (1).png>)

\
\
