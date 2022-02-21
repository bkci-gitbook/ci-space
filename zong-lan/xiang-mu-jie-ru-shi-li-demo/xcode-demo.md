# XCode Demo

## **准备材料：** <a href="#xcodedemo-zhun-bei-cai-liao" id="xcodedemo-zhun-bei-cai-liao"></a>

1. #### &#x20;一个 XCode 工程 <a href="#xcodedemo-yi-ge-xcode-gong-cheng" id="xcodedemo-yi-ge-xcode-gong-cheng"></a>
2. #### &#x20;一台 mac 构建机 <a href="#xcodedemo-yi-tai-mac-gou-jian-ji" id="xcodedemo-yi-tai-mac-gou-jian-ji"></a>
3. #### 开发者证书/发布证书, <a href="#xcodedemo-kai-fa-zhe-zheng-shu-fa-bu-zheng-shu-qian-wang-lan-dun-zheng-shu-xi-tong-httpskeystore.oa." id="xcodedemo-kai-fa-zhe-zheng-shu-fa-bu-zheng-shu-qian-wang-lan-dun-zheng-shu-xi-tong-httpskeystore.oa."></a>

## **详细步骤:** <a href="#xcodedemo-xiang-xi-bu-zhou" id="xcodedemo-xiang-xi-bu-zhou"></a>

### **1. 新建项目** <a href="#xcodedemo1.-xin-jian-xiang-mu" id="xcodedemo1.-xin-jian-xiang-mu"></a>

蓝盾官网 [http://devops.oa.com/console/](http://devops.oa.com/console/) ， 右上角个人企业微信账号->项目管理->新建项目，输入项目名称、描述等相关信息后，点击确认。

### **2. 关联代码库** <a href="#xcodedemo2.-guan-lian-dai-ma-ku" id="xcodedemo2.-guan-lian-dai-ma-ku"></a>

#### 详情点这里：[关联你的第一个代码库](https://iwiki.woa.com/pages/viewpage.action?pageId=1488956012) <a href="#xcodedemo-xiang-qing-dian-zhe-li" id="xcodedemo-xiang-qing-dian-zhe-li"></a>

### **3. mac构建机接入** <a href="#xcodedemo3.mac-gou-jian-ji-jie-ru" id="xcodedemo3.mac-gou-jian-ji-jie-ru"></a>

macos 构建机登录方式：mac系统使用自带的 VNC 远程协助， windows 使用 VNC View 或 remotix 等 VNC 工具、(登录时不要开启代理）

下载 VNC View：

windows: [https://www.realvnc.com/download/file/viewer.files/VNC-Viewer-6.19.715-Windows.exe](https://www.realvnc.com/download/file/viewer.files/VNC-Viewer-6.19.715-Windows.exe)

mac : [https://www.realvnc.com/download/file/viewer.files/VNC-Viewer-6.20.529-MacOSX-x86\_64.dmg](https://www.realvnc.com/download/file/viewer.files/VNC-Viewer-6.20.529-MacOSX-x86\_64.dmg)

安装好后，建立一个连接，办公网的需要注意取消代理，否则可能出现无法远程连接的情况： 鼠标右键：Properties->Expect->Filter→ProxyTcpRfb 设置为 False

![](<../../.gitbook/assets/image (15).png>)

\


导入构建机：

在蓝盾选择 服务-部署-环境管理-节点-导入-第三方构建机，根据弹出提示复制 Agent 安装命令，VNC 远程登录到申请的专机上，打开终端 terminal 中运行命令。待 Agent 安装成功后，刷新导入机器即可。

![](<../../.gitbook/assets/image (14).png>)

\


### **4. 证书管理** <a href="#xcodedemo4.-zheng-shu-guan-li" id="xcodedemo4.-zheng-shu-guan-li"></a>

ios证书分为： 开发者证书、发布证书、企业签名证书， 对证书有任何证书疑问，可联系 KeyStore-helper（蓝盾证书助手）

开发证书： 该证书主要用于开发人员真机调试，创建前需要先把自测设备的ID号登记到对应开发者帐号中\
企业证书： 用企业证书打包应用的最大好处是：应用可以安装到非越狱的设备上运行，这样很方便进行较大规模的测试、或公司范围发起内部体验（企业证书打包的应用禁止外发）。\
发布证书： 用来打包、发布应用的证书

| 文件格式            | 描述                                                                                               |
| --------------- | ------------------------------------------------------------------------------------------------ |
| 签名p12           | 由证书（Certification）与专用密钥（keychain）组成；该文件签名打包是必须的，而且从该文件能看出开发商信息，如：Tencent Technology(Shanghai)字样。 |
| mobileprovision | 签名授权文件，与boundle id 绑定，该文件是签名打包是必须的。                                                              |
| 消息推送p12         | 该文件是可选的，只有启用消息推送的应用才需要该文件（配置到Push后台）。                                                            |



1\) 从[蓝盾证书系统](https://keystore.oa.com)获取 IOS 证书应用到流水线构建

详情请查看： [http://km.oa.com/group/keystore/articles/show/390635](http://km.oa.com/group/keystore/articles/show/390635)

2\) 如本地已有 .p12 和 .mobileprovision 文件， 可以在蓝盾的 凭证管理-新增证书-iOS证书中上传。

![](<../../.gitbook/assets/image (10).png>)

3\) 如果业务对企业签名有特殊功能，重签名时需要使用项目特定的企业签名的描述文件， 也需要上传到下图的 iOS 企业签名证书中。

![](<../../.gitbook/assets/image (25).png>)

\


\


### **5. 配置流水线** <a href="#xcodedemo5.-pei-zhi-liu-shui-xian" id="xcodedemo5.-pei-zhi-liu-shui-xian"></a>

首先新建流水线。

一般的流水线，可能会用到以下插件： 构建环境-MACOS、拉取Git（命令行）、IOS证书安装、Bash、iOS企业证书签名并归档(new)、归档构件、版本体验

![](<../../.gitbook/assets/image (34).png>)

#### **5.1 构建环境-MACOS** <a href="#xcodedemo5.1-gou-jian-huan-jing-macos" id="xcodedemo5.1-gou-jian-huan-jing-macos"></a>

单击构建环境-MACOS 选项，在右侧会弹出配置栏，在构建资源类型选择 ”第三方构建机--节点“，指定构建资源会提示你自己接入的机器名称和 IP，选中即可。

![](<../../.gitbook/assets/image (19).png>)

\


#### **5.2 拉取Git（命令行）** <a href="#xcodedemo5.2-la-qu-git-ming-ling-hang" id="xcodedemo5.2-la-qu-git-ming-ling-hang"></a>

拉取Git配置里可以指定我们之前关联的代码库，并指定想要构建的分支名称。拉取策略可以根据实际需要选择。

![](<../../.gitbook/assets/image (8).png>)

\


#### **5.3 Bash** <a href="#xcodedemo-5.3bash" id="xcodedemo-5.3bash"></a>

这里直接调用项目的编译脚本即可。 bash 脚本中用到的一些环境变量，需要在构建触发-流水线变量中提前配置好。\


**5.4 iOS企业证书签名并归档(new)**

在签名并归档 IPA 中，证书名称中可下拉选择在证书管理中所配置的企业证书名称。所以需要将 IPA 文件路径相对于 $WORKSPACE 的实际路径

![](<../../.gitbook/assets/image (30).png>)

\


流水线配置完成后点击保存并执行，如果成功则会生成相应的 IPA 安装包。

#### 5.5 版本体验 <a href="#xcodedemo5.5-ban-ben-ti-yan" id="xcodedemo5.5-ban-ben-ti-yan"></a>

在版本体验中选择新增体验，在从版本仓库获取中选择刚刚打包好的 IPA，并选择体验组、附加内部人员、附加外部人员（QQ号）。

体验组人员可在权限中心-我的项目-添加用户组中 新增用户组。

![](<../../.gitbook/assets/image (21).png>)
