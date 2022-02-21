# Android Demo

## 准备材料： <a href="#androiddemo-zhun-bei-cai-liao" id="androiddemo-zhun-bei-cai-liao"></a>

* 一个可以编译出 APK 包的 Git 工程

## 详细步骤： <a href="#androiddemo-xiang-xi-bu-zhou" id="androiddemo-xiang-xi-bu-zhou"></a>

### 1. 搭建开发分支的持续构建 <a href="#androiddemo1.-da-jian-kai-fa-fen-zhi-de-chi-xu-gou-jian" id="androiddemo1.-da-jian-kai-fa-fen-zhi-de-chi-xu-gou-jian"></a>

先上图：

![soho app流水线](http://km.oa.com/files/photos/pictures/20190924/1569303237\_9.png)

这个是我自己搭建的流水线，主要分别构建Release和Debug类型的版本，Release版本构建需要经过CodeCC代码检查，3-1我设置了一个通知并且将编译好的apk设置金刚漏洞扫描，保证每一次编译的包都是安全可靠的。下面我会介绍我是怎么搭建的：

#### 1.1 设置构建触发 <a href="#androiddemo1.1-she-zhi-gou-jian-chu-fa" id="androiddemo1.1-she-zhi-gou-jian-chu-fa"></a>

![选择Job类型](http://km.oa.com/files/photos/pictures/20190924/1569303475\_98.png)

这里选择Job类型是Linux。

![1-1 构建触发](http://km.oa.com/files/photos/pictures/20190924/1569303684\_31.png)

这里设置构建的版本，一般版本格式为：主版本号.次版本号.修订号.构建号

* MajorVersion（主版本号）
* MinorVersion（次版本号）
* FixVersion（修订号）
* BuildNo（构建号）

比如：1.3.1.33

版本号设定好之后，我这里设置了两种构建触发类型：

* 手动触发
* Git事件触发

手动触发大家好理解，**Git事件触发是基于每一次我们提交，监听具体分支进行触发构建**。

#### ![Git事件触发](http://km.oa.com/files/photos/pictures/20190924/1569307599\_46.png) <a href="#androiddemo" id="androiddemo"></a>

#### **1.2 设置构建类型构建** <a href="#androiddemo1.2-she-zhi-gou-jian-lei-xing-gou-jian" id="androiddemo1.2-she-zhi-gou-jian-lei-xing-gou-jian"></a>

这部分就是真正执行构建的步骤

**1) 拉取GIT**

![拉取GIT](http://km.oa.com/files/photos/pictures/20190924/1569308153\_80.png)

**2) 证书签名**

前往 [https://keystore.oa.com](https://keystore.oa.com) 进行证书添加/管理.

新增完之后，就可以对Android证书签名插件进行配置（配置问题请咨询【KeyStore-helper(证书助手)】）：

![](https://iwiki.woa.com/download/attachments/1488956085/image2021-8-23\_11-38-10.png?version=1\&modificationDate=1645176855000\&api=v2)

**3) 执行构建**

选择Bash插件，通过这个插件来执行工程中的构建脚本，笔者这里是**cibuild.sh（这里存放于工程根目录）**

&#x20;展开源码

```
```

主要就是通过gradle命令传递参数来执行具体的task来构建apk，可以看下我定义的两个函数：

* buildDebug()
* buildRelease()

分别构建debug版本和release版本的apk。

这里还没完，构建完还需要做两件事：

* 重命名apk文件
* 归档到bin目录

我这里抽了一个单独的gradle脚本：**tools.gradle**

&#x20;展开源码

稍微解释一下这段脚本的作用：通过gradle hook assemble task，基于variant变量拿到输出文件，按照我们自己的规则重命名apk，然后通过copy命令复制到bin目录下。

然后在app module下的build.gradle这样引用：

`apply from: 'tools.gradle'`

因为这里要拿到gradle传递过来的参数，笔者是这样做的：

![请在这里填写图片描述](http://km.oa.com/files/photos/pictures/20190924/1569311892\_88.png)

可以看到笔者这里有定义一些全局的变量，比如rootProject.ext.batch\_run，这里根据你自己项目来设计，我贴下我的配置：

**Crowdsource-android/config.gradle**

| `ext {    // version code不能随便修改，正常每个版本升一级    version_code = 33    // 构建号,本地默认取版本号    build_no = "${version_code}"    build_front_name = "1.3.1"    apk_prefix = "SOHO_ANDROID"    batch_run = true    build_mode = "debug"    build_version = build_front_name + "."` `+ build_no` `android = [            compileSdkVersion: 28,            buildToolsVersion: '28.0.0',            applicationId    : "com.tencent.csapp",            minSdkVersion    : 16,            targetSdkVersion : 28,            javaVersion      : JavaVersion.VERSION_1_8,            versionCode      : version_code,            versionName      : build_front_name    ]}` |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

这样就很统一的去维护工程是一些配置，版本一目了然。

**4) 归档构件**

这个很简单，配置一下待归档的文件规则就行了。

![](http://km.oa.com/files/photos/pictures/20190924/1569312890\_81.png)

**5) CodeCC代码检查**

为了保证app研发质量，引入Code CC静态代码扫描工具来帮助我们发现一些潜在缺陷，具体看下配置：

![CodeCC插件配置](http://km.oa.com/files/photos/pictures/20190924/1569313074\_14.png)

shell脚本：

| `export` `COVERITY_NO_PRELOAD_CLASSES=java.io.ObjectInputStream:java.lang.UNIXProcess` `rm` `-rf local.propertiestouch` `"local.properties"echo` `-e "sdk.dir"="/data/bkdevops/apps/android-sdk-linux/android-sdk"` `> "local.properties"echo` `-e "\nndk.dir"="/data/bkdevops/apps/ndk/android-ndk-r13b"` `>> "local.properties"` `export` `CI=trueexport` `BUILD_MODE=debug` `chmod` `755 cibuild.sh/bin/sh` `cibuild.sh` |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\


因为笔者工程使用的是Java8，当时编译出错了，后面找官方同学解决了，仅供参考。

以上，完整的构建基本上搭建完成了。我们再来回顾一下做了哪些事情：

* 拉取代码
* 证书安装
* 编译构建apk
* 归档构件
* CodeCC代码检查

至此我们已经可以构建出供我们自测和体验的版本：

![查看构件](http://km.oa.com/files/photos/pictures/20190924/1569313403\_62.png)

\


#### **1.3 发送通知&漏洞扫描** <a href="#androiddemo1.3-fa-song-tong-zhi-lou-dong-sao-miao" id="androiddemo1.3-fa-song-tong-zhi-lou-dong-sao-miao"></a>

因为公司对安全是十分重视的，所以笔者的pipeline也增加了漏洞扫描插件：

![金刚漏洞扫描](http://km.oa.com/files/photos/pictures/20190924/1569313600\_51.png)

可以看到你构件的apk需要满足一定规则，不然不知道要扫描哪个apk。

笔者还设置了企业微信通知，构建完成之后给相关人发送通知，测试同学可以直接链接去拿测试包：

![企业微信通知](http://km.oa.com/files/photos/pictures/20190924/1569313838\_91.png)

ok，这样我们就搭建了一个相对比较好用的流水线了，每天定时和每次push都会触发构建，简直不能再爽了。

\


### 2. 搭建发布分支的持续构建 <a href="#androiddemo2.-da-jian-fa-bu-fen-zhi-de-chi-xu-gou-jian" id="androiddemo2.-da-jian-fa-bu-fen-zhi-de-chi-xu-gou-jian"></a>

上一节我们搭建的是开发过程的流水线，但发布版本构建应该要跟开发分开，并且只有发布版本的时候才会去构建，这里笔者抛砖引玉：

![发布分支构建](http://km.oa.com/files/photos/pictures/20190924/1569314118\_70.png)

因为我们只需要Release包，发布构建我在前面基础上加了加固插件，这里用的是乐固，上应用宝强制使用加固，加固完还需要重新签名，通过这条流水线构建出来的包就可以发布到应用宝啦。
