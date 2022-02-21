# UE4 Demo

## **准备材料：** <a href="#ue4demo-zhun-bei-cai-liao" id="ue4demo-zhun-bei-cai-liao"></a>

1. #### &#x20;一个 Unreal Engine 工程 <a href="#ue4demo-yi-ge-unrealengine-gong-cheng" id="ue4demo-yi-ge-unrealengine-gong-cheng"></a>
2. #### &#x20;一台 mac/windows 构建机 <a href="#ue4demo-yi-tai-macwindows-gou-jian-ji" id="ue4demo-yi-tai-macwindows-gou-jian-ji"></a>
3. #### 开发者证书/发布证书 <a href="#ue4demo-kai-fa-zhe-zheng-shu-fa-bu-zheng-shu-qian-wang-lan-dun-zheng-shu-xi-tong-httpskeystore.oa.co" id="ue4demo-kai-fa-zhe-zheng-shu-fa-bu-zheng-shu-qian-wang-lan-dun-zheng-shu-xi-tong-httpskeystore.oa.co"></a>

## **详细步骤:** <a href="#ue4demo-xiang-xi-bu-zhou" id="ue4demo-xiang-xi-bu-zhou"></a>

### **1. 新建项目** <a href="#ue4demo1.-xin-jian-xiang-mu" id="ue4demo1.-xin-jian-xiang-mu"></a>

蓝盾官网 [http://devops.oa.com/console/](http://devops.oa.com/console/) ， 右上角个人企业微信账号-》项目管理-》新建项目，输入项目名称、描述等相关信息后，点击确认。

### **2. 关联代码库** <a href="#ue4demo2.-guan-lian-dai-ma-ku" id="ue4demo2.-guan-lian-dai-ma-ku"></a>

#### 详情点这里：[关联你的第一个代码库](http://iwiki.oa.com/pages/viewpage.action?pageId=10718809) <a href="#ue4demo-xiang-qing-dian-zhe-li-guan-lian-ni-de-di-yi-ge-dai-ma-ku" id="ue4demo-xiang-qing-dian-zhe-li-guan-lian-ni-de-di-yi-ge-dai-ma-ku"></a>

### **3. 构建机接入** <a href="#ue4demo3.-gou-jian-ji-jie-ru" id="ue4demo3.-gou-jian-ji-jie-ru"></a>

向devcloud申请的macos构建机登录方式：mac 系统使用自带的 VNC 远程协助， windows 使用 VNC View 或 remotix 等 VNC 工具、(登录时不要开启代理）

下载 VNC View：

windows: [https://www.realvnc.com/download/file/viewer.files/VNC-Viewer-6.19.715-Windows.exe](https://www.realvnc.com/download/file/viewer.files/VNC-Viewer-6.19.715-Windows.exe)

mac : [https://www.realvnc.com/download/file/vnc.files/VNC-Server-6.6.0-MacOSX-x86\_64.pkg](https://www.realvnc.com/download/file/vnc.files/VNC-Server-6.6.0-MacOSX-x86\_64.pkg)

安装好后，建立一个连接，办公网的需要注意取消代理，否则可能出现无法远程连接的情况： 鼠标右键：Properties->Expect->Filter→ProxyTcpRfb 设置为 False\
![](<../../.gitbook/assets/image (16) (1).png>)

导入构建机：

在蓝盾选择 服务-部署-环境管理-节点-导入-第三方构建机，根据弹出提示复制Agent安装命令，VNC远程登录到申请的专机上，打开终端terminal中运行命令。待Agent安装成功后，刷新导入机器即可。

![](<../../.gitbook/assets/image (3) (1).png>)

### **4. UE4引擎与项目配置部分** <a href="#ue4demo4.ue4-yin-qing-yu-xiang-mu-pei-zhi-bu-fen" id="ue4demo4.ue4-yin-qing-yu-xiang-mu-pei-zhi-bu-fen"></a>

#### &#x20;   4.1 引擎编译过程需要网络，请保持可以连接外网 <a href="#ue4demo4.1-yin-qing-bian-yi-guo-cheng-xu-yao-wang-luo-qing-bao-chi-ke-yi-lian-jie-wai-wang" id="ue4demo4.1-yin-qing-bian-yi-guo-cheng-xu-yao-wang-luo-qing-bao-chi-ke-yi-lian-jie-wai-wang"></a>

#### &#x20;   4.2 IOS打包需要配置证书信息，证书的配置在项目文件下面的Config目录中修改 DefaultEngine.ini文件，此文件为项目配置文件，大部分的设置都可以在这个文件中配 置。具体如下： <a href="#ue4demo4.2ios-da-bao-xu-yao-pei-zhi-zheng-shu-xin-xi-zheng-shu-de-pei-zhi-zai-xiang-mu-wen-jian-xia" id="ue4demo4.2ios-da-bao-xu-yao-pei-zhi-zheng-shu-xin-xi-zheng-shu-de-pei-zhi-zai-xiang-mu-wen-jian-xia"></a>

| `[/Script/IOSRuntimeSettings.IOSRuntimeSettings]` `BundleIdentifier=com.tencentt.*                // 具体根据项目填写` `SigningCertificate=iPhone Developer: ********  //具体证书信息` `MobileProvision=*.mobileprovision            //证书对应描述文件信息` |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |

\


#### &#x20;   4.3 编译脚本： <a href="#ue4demo4.3-bian-yi-jiao-ben" id="ue4demo4.3-bian-yi-jiao-ben"></a>

1）引擎编译脚本如下：

可根据平台的不同选择编译对应的引擎。 如在windows下编译android，需要编译对应的引擎，mac下可支持ios和android，均需要通过脚本编译对应平台的引擎。

如在Mac平台针对ios基本命令如下，其他平台类似，只需调整部分参数即可。

| `./UE4/Engine/Build/BatchFiles/RunUAT.sh BuildGraph -target="Installed Build Group Mac"` `-script="Engine/Build/InstalledEngineBuild.xml"` `-set:WithTVOS=false` `-set:WithLinux=false` `-set:WithHTML5=false` `-set:WithSwitch=false` `-set:WithDDC=false` `-set:WithWin32=false` `-set:WithPS4=false` `-set:WithMac=true` `-set:WithIOS=true`  `-set:WithAndroid=false` |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

\


2） 项目编译部分IOS：

| `-ScriptsForProject={0} BuildCookRun -nocompileeditor -nop4 -project={1} -cook -stage -archive -archivedirectory={2} -package` `-clientconfig=Development -ue4exe=UE4Editor -pak -prereqs -nodebuginfo -targetplatform=IOS -build -utf8output"` |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |

**其中0,1： 为项目.uproject，  2: 为最终包归档的路径**

\


### **5. 配置流水线** <a href="#ue4demo5.-pei-zhi-liu-shui-xian" id="ue4demo5.-pei-zhi-liu-shui-xian"></a>

首先新建流水线。以IOS为例

一般的流水线，可能会用到以下插件： 构建环境-MACOS、拉取Git（命令行）/ 拉取SVN（命令行）、IOS证书安装、Bash、iOS企业证书签名并归档(new)、归档构件、版本体验

![](<../../.gitbook/assets/image (15) (1) (1).png>)

#### 5.1 构建环境-MACOS <a href="#ue4demo5.1-gou-jian-huan-jing-macos" id="ue4demo5.1-gou-jian-huan-jing-macos"></a>

单击构建环境-MACOS 选项，在右侧会弹出配置栏，在构建资源类型选择 ”第三方构建机--节点“，指定构建资源会提示你自己接入的机器名称和 IP，选中即可。

![](<../../.gitbook/assets/image (26) (1).png>)

\


#### **5.2 拉取Git（命令行）/   拉取SVN（命令行）** <a href="#ue4demo5.2-la-qu-git-ming-ling-hang-la-qu-svn-ming-ling-hang" id="ue4demo5.2-la-qu-git-ming-ling-hang-la-qu-svn-ming-ling-hang"></a>

拉取 Git 配置里可以指定我们之前关联的代码库，并指定想要构建的分支名称。拉取策略可以根据实际需要选择。

![](<../../.gitbook/assets/image (30) (1).png>)

\


#### **5.3 Bash** <a href="#ue4demo-5.3bash" id="ue4demo-5.3bash"></a>

`这里直接调用项目的编译脚本即可。` bash 脚本中用到的一些环境变量，需要在构建触发-流水线变量中提前配置好。\
\
![](<../../.gitbook/assets/image (20) (1).png>)\
![](<../../.gitbook/assets/image (21) (1).png>)

#### 5.4 iOS企业证书签名并归档(new) <a href="#ue4demo5.4ios-qi-ye-zheng-shu-qian-ming-bing-gui-dang-new" id="ue4demo5.4ios-qi-ye-zheng-shu-qian-ming-bing-gui-dang-new"></a>

在签名并归档IPA中，证书名称中可下拉选择在证书管理中所配置的企业证书名称。所以需要将 IPA 文件路径相对于 $WORKSPACE 的实际路径

![](<../../.gitbook/assets/image (27) (1) (1).png>)

流水线配置完成后点击保存并执行，如果成功则会生成相应的 IPA 安装包。

#### 5.5 版本体验 <a href="#ue4demo5.5-ban-ben-ti-yan" id="ue4demo5.5-ban-ben-ti-yan"></a>

在版本体验中选择新增体验，在从版本仓库获取中选择刚刚打包好的 IPA，并选择体验组、附加内部人员、附加外部人员（QQ号）。

体验组人员可在权限中心-我的项目-添加用户组中 新增用户组。

![](<../../.gitbook/assets/image (24) (1).png>)
