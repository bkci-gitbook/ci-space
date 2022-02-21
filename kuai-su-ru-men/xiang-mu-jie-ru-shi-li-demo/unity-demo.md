# Unity Demo

## **准备材料：** <a href="#unitydemo-zhun-bei-cai-liao" id="unitydemo-zhun-bei-cai-liao"></a>

1. #### &#x20;一个 Unity 工程 <a href="#unitydemo-yi-ge-unity-gong-cheng" id="unitydemo-yi-ge-unity-gong-cheng"></a>
2. #### &#x20;一台 mac/windows 构建机 <a href="#unitydemo-yi-tai-macwindows-gou-jian-ji" id="unitydemo-yi-tai-macwindows-gou-jian-ji"></a>
3. #### IOS 需要准备开发者证书/发布证书, 前往蓝盾证书系统申请：[https://keystore.oa.com](https://keystore.oa.com) <a href="#unitydemoios-xu-yao-zhun-bei-kai-fa-zhe-zheng-shu-fa-bu-zheng-shu-qian-wang-lan-dun-zheng-shu-xi-ton" id="unitydemoios-xu-yao-zhun-bei-kai-fa-zhe-zheng-shu-fa-bu-zheng-shu-qian-wang-lan-dun-zheng-shu-xi-ton"></a>

\


## **详细步骤:** <a href="#unitydemo-xiang-xi-bu-zhou" id="unitydemo-xiang-xi-bu-zhou"></a>

### **1. 新建项目** <a href="#unitydemo1.-xin-jian-xiang-mu" id="unitydemo1.-xin-jian-xiang-mu"></a>

蓝盾官网 [http://devops.oa.com/console/](http://devops.oa.com/console/) ， 右上角个人企业微信账号-》项目管理-》新建项目，输入项目名称、描述等相关信息后，点击确认。

### **2. 关联代码库** <a href="#unitydemo2.-guan-lian-dai-ma-ku" id="unitydemo2.-guan-lian-dai-ma-ku"></a>

#### 详情点这里：[关联你的第一个代码库](http://iwiki.oa.com/pages/viewpage.action?pageId=10718809) <a href="#unitydemo-xiang-qing-dian-zhe-li-guan-lian-ni-de-di-yi-ge-dai-ma-ku" id="unitydemo-xiang-qing-dian-zhe-li-guan-lian-ni-de-di-yi-ge-dai-ma-ku"></a>

### **3. mac构建机接入** <a href="#unitydemo3.mac-gou-jian-ji-jie-ru" id="unitydemo3.mac-gou-jian-ji-jie-ru"></a>

向devcloud申请的 macos 构建机登录方式：mac 系统使用自带的 VNC 远程协助， windows 使用 VNC View 或 remotix 等 VNC 工具(登录时不要开启代理）

下载 VNC View：

windows: [https://www.realvnc.com/download/file/viewer.files/VNC-Viewer-6.19.715-Windows.exe](https://www.realvnc.com/download/file/viewer.files/VNC-Viewer-6.19.715-Windows.exe)

mac : [https://www.realvnc.com/download/file/vnc.files/VNC-Server-6.6.0-MacOSX-x86\_64.pkg](https://www.realvnc.com/download/file/vnc.files/VNC-Server-6.6.0-MacOSX-x86\_64.pkg)

安装好后，建立一个连接，办公网的需要注意取消代理，否则可能出现无法远程连接的情况： 鼠标右键：Properties->Expect->Filter→ProxyTcpRfb设置为False

![](<../../.gitbook/assets/image (33) (1).png>)

\


构建机默认安装了Xcode10.0、Xcode10.1、Xcode10.2.1、Xcode11

android SDK： /data/bkdevops/apps/android-sdk

gradle： /data/bkdevops/apps/gradle

ndk: /data/bkdevops/apps/ndk

\


**构建机如需访问外网，需要配置devnet**外网代理， 配置指引：[**http://8000.oa.com/?s=search#/article?id=KB201901310001**](http://8000.oa.com/?s=search#/article?id=KB201901310001)

\


导入构建机:

在蓝盾选择 服务-部署-环境管理-节点-导入-第三方构建机，根据弹出提示复制Agent安装命令，VNC远程登录到申请的专机上，打开终端terminal中运行命令。待Agent安装成功后，刷新导入机器即可。

![](<../../.gitbook/assets/image (12) (1).png>)

\


### **4. 证书管理** <a href="#unitydemo4.-zheng-shu-guan-li" id="unitydemo4.-zheng-shu-guan-li"></a>

#### 4.1  IOS证书 <a href="#unitydemo4.1ios-zheng-shu" id="unitydemo4.1ios-zheng-shu"></a>

分为： 开发者正式、发布证书、企业签名证书， 对证书有任何证书疑问，可联系 KeyStore-helper（蓝盾证书助手）

开发证书： 该证书主要用于开发人员真机调试，创建前需要先把自测设备的ID号登记到对应开发者帐号中\
企业证书： 用企业证书打包应用的最大好处是：应用可以安装到非越狱的设备上运行，这样很方便进行较大规模的测试、或公司范围发起内部体验（企业证书打包的应用禁止外发）。\
发布证书： 用来打包、发布应用的证书

| 文件格式            | 描述                                                                                               |
| --------------- | ------------------------------------------------------------------------------------------------ |
| 签名p12           | 由证书（Certification）与专用密钥（keychain）组成；该文件签名打包是必须的，而且从该文件能看出开发商信息，如：Tencent Technology(Shanghai)字样。 |
| mobileprovision | 签名授权文件，与boundle id 绑定，该文件是签名打包是必须的。                                                              |
| 消息推送p12         | 该文件是可选的，只有启用消息推送的应用才需要该文件（配置到Push后台）。                                                            |

\


因公司对企业签名证书的安全要求，蓝盾的企业签名使用的是重签名的机制，只能先用开发者正式或发布证书先打包出 IPA，再通过蓝盾的流水线插件“iOS企业证书签名并归档(new)” 进行企业证书重签名。

故，流水线中需要先用开发者证书或发布证书先打出IPA包，再使用“iOS企业证书签名并归档(new)“插件进行企业证书重签名。

\


1\) 从[蓝盾证书系统](https://keystore.oa.com)获取 iOS 证书应用到流水线构建

详情请查看： [http://km.oa.com/group/keystore/articles/show/390635](http://km.oa.com/group/keystore/articles/show/390635)

2\) 如本地已有 .p12 和 .mobileprovision 文件， 可以在蓝盾的 凭证管理-新增证书-iOS证书中上传。

![](<../../.gitbook/assets/image (29).png>)

3\) 如果业务对企业签名有特殊功能，重签名时需要使用项目特定的企业签名的描述文件， 也需要上传到下图的iOS企业签名证书中。

![](<../../.gitbook/assets/image (11).png>)

#### 4.2 Android 证书 <a href="#unitydemo4.2android-zheng-shu" id="unitydemo4.2android-zheng-shu"></a>

将项目所用的android证书托管到蓝盾：[http://devops.oa.com/console/ticket/bkdevops/createCert](http://devops.oa.com/console/ticket/bkdevops/createCert)

### &#x20;<a href="#unitydemo" id="unitydemo"></a>

\


### **5. 配置流水线** <a href="#unitydemo5.-pei-zhi-liu-shui-xian" id="unitydemo5.-pei-zhi-liu-shui-xian"></a>

首先新建流水线。以 IOS 为例

一般的流水线，可能会用到以下插件： 构建环境-MACOS、拉取Git（命令行）/ 拉取SVN（命令行）、IOS证书安装、Bash、iOS企业证书签名并归档(new)、归档构件、版本体验

![](<../../.gitbook/assets/image (18).png>)

#### **5.1 构建环境-MACOS** <a href="#unitydemo5.1-gou-jian-huan-jing-macos" id="unitydemo5.1-gou-jian-huan-jing-macos"></a>

单击构建环境-MACOS 选项，在右侧会弹出配置栏，在构建资源类型选择 ”第三方构建机–节点“，指定构建资源会提示你自己接入的机器名称和 ip，选中即可。

![](<../../.gitbook/assets/image (9).png>)

\


#### **5.2 拉取Git（命令行）/   拉取SVN（命令行）** <a href="#unitydemo5.2-la-qu-git-ming-ling-hang-la-qu-svn-ming-ling-hang" id="unitydemo5.2-la-qu-git-ming-ling-hang-la-qu-svn-ming-ling-hang"></a>

拉取Git配置里可以指定我们之前关联的代码库，并指定想要构建的分支名称。拉取策略可以根据实际需要选择。

![](<../../.gitbook/assets/image (20).png>)

\


#### **5.3 Bash** <a href="#unitydemo-5.3bash" id="unitydemo-5.3bash"></a>

`这里直接调用项目的编译脚本即可。` bash 脚本中用到的一些环境变量，需要在构建触发-流水线变量中提前配置好。\
![](<../../.gitbook/assets/image (13).png>)\
![](<../../.gitbook/assets/image (34).png>)\
以下脚本仅做参考：\
\


**unity构建参考脚本**

```
# 您可以通过setEnv函数设置原子间传递的参数
# setEnv "FILENAME" "package.zip"
# 然后在后续的原子的表单中使用${FILENAME}引用这个变量

# cd ${WORKSPACE} 可进入当前工作空间目录

#配置证书
developmentTeam="84V6K7G4P8"
codeSignIdentity="iPhone Developer: xxxx xxxx (UN99VPZAW2)"
provisioningProfile="3f49e5fe-da46-4a8b-8d31-c7c6c36b7162"

#需要接受3个参数 1、scheme名 2、工程目录 3、工程名字
# cd ${WORKSPACE} 可进入当前工作空间目录
if [ -d ${WORKSPACE}/result ]
then
rm -rf ${WORKSPACE}/result
fi
mkdir -p ${WORKSPACE}/result
cd ${WORKSPACE}


#project目录
PROJECT_PATH=`pwd`
#project名称
PROJECT_NAME="Unity-iPhone.xcodeproj"
#scheme名称
SCHEME_NAME="Unity-iPhone"

#归档文件地址
ARCHIVE_PATH=$PROJECT_PATH/$SCHEME_NAME

echo "Start Build Unity to Xcodeproject"
if 
	##ProjectTool.BuildForIOS 编译入口函数
	$UNITY_PATH -batchmode -projectPath $PROJECT_PATH -executeMethod ProjectTool.BuildForIOS -quit -logFile $PROJECT_PATH/log/ios.log
then
	cat $PROJECT_PATH/log/ios.log
else
	cat $PROJECT_PATH/log/ios.log
	exit 1
fi

##取消自动签名
sed -i '' 's/ProvisioningStyle = Automatic;/ProvisioningStyle = Manual;/' Unity-iPhone.xcodeproj/project.pbxproj

#通过archive归档出对应的xcarchive文件
#对应步骤:
#1、清理工程
#2、归档工程
#3、工程名称
#4、设置工程Scheme
#5、设置Debug或者Release模式
#6、归档输出地址
xcodebuild clean \
archive \
-project "$PROJECT_PATH/$PROJECT_NAME" \
-scheme "$SCHEME_NAME" \
-configuration "Release" \
-archivePath "$ARCHIVE_PATH" \
DEVELOPMENT_TEAM="${developmentTeam}" CODE_SIGN_IDENTITY="${codeSignIdentity}" PROVISIONING_PROFILE="${provisioningProfile}"
echo "--------------------------------------"

#生成ExportOptions.plist文件
cat >> ./ExportOptions.plist <<EOF 
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE plist PUBLIC "-//Apple//DTD PLIST 1.0//EN" "http://www.apple.com/DTDs/PropertyList-1.0.dtd">
<plist version="1.0">
<dict>
<key>teamID</key>
<string>84V6K7G4P8</string>
<key>method</key>
<string>development</string>
<key>compileBitcode</key>
<false></false>
<key>provisioningProfiles</key>
<dict>
<key>com.tencent.bkdevops</key>
<string>bkdevop</string>
</dict>
</dict>
</plist>
EOF

#通过归档文件打包出对应的ipa文件
#对应步骤：
#1、打包命令
#2、归档文件地址
#3、ipa输出地址
#4、ipa打包设置文件地址
xcodebuild -exportArchive \
-archivePath "$ARCHIVE_PATH.xcarchive" \
-exportPath ${WORKSPACE}/result \
-exportOptionsPlist "$PROJECT_PATH/ExportOptions.plist"

ls -ltr ${WORKSPACE}/result

```

**5.4 iOS企业证书签名并归档(new)**

在签名并归档 IPA 中，证书名称中可下拉选择在证书管理中所配置的企业证书名称。所以需要将 IPA 文件路径相对于 $WORKSPACE 的实际路径

![](<../../.gitbook/assets/image (32).png>)

流水线配置完成后点击保存并执行，如果成功则会生成相应的 IPA 安装包。

#### 5.5 版本体验 <a href="#unitydemo5.5-ban-ben-ti-yan" id="unitydemo5.5-ban-ben-ti-yan"></a>

在版本体验中选择新增体验，在从版本仓库获取中选择刚刚打包好的 IPA，并选择体验组、附加内部人员、附加外部人员（QQ号）。

体验组人员可在权限中心-我的项目-添加用户组中 新增用户组。

![](<../../.gitbook/assets/image (28).png>)
