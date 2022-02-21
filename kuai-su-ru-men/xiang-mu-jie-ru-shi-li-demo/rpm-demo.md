# RPM Demo

## **准备材料：** <a href="#rpmdemo-zhun-bei-cai-liao" id="rpmdemo-zhun-bei-cai-liao"></a>

1. #### 一个可以编译出 rpm 包的 Git 工程：[https://git.code.oa.com/ci-demo/rpm](https://git.code.oa.com/ci-demo/rpm) <a href="#rpmdemo-yi-ge-ke-yi-bian-yi-chu-rpm-bao-de-git-gong-cheng-httpsgit.code.oa.comcidemorpm" id="rpmdemo-yi-ge-ke-yi-bian-yi-chu-rpm-bao-de-git-gong-cheng-httpsgit.code.oa.comcidemorpm"></a>
2. #### 一个腾讯软件源上的 RPM 私有库账号密码 <a href="#rpmdemo-yi-ge-teng-xun-ruan-jian-yuan-shang-de-rpm-si-you-ku-zhang-hao-mi-ma" id="rpmdemo-yi-ge-teng-xun-ruan-jian-yuan-shang-de-rpm-si-you-ku-zhang-hao-mi-ma"></a>

## **详细步骤:** <a href="#rpmdemo-xiang-xi-bu-zhou" id="rpmdemo-xiang-xi-bu-zhou"></a>

### **1. 将代码库关联至蓝盾，**[**请参考**](https://iwiki.woa.com/pages/viewpage.action?pageId=1488956012) <a href="#rpmdemo1.-jiang-dai-ma-ku-guan-lian-zhi-lan-dun-qing-can-kao" id="rpmdemo1.-jiang-dai-ma-ku-guan-lian-zhi-lan-dun-qing-can-kao"></a>

### **2. 创建一条空流水线，**[**请参考**](https://iwiki.woa.com/pages/viewpage.action?pageId=1488956006) <a href="#rpmdemo2.-chuang-jian-yi-tiao-kong-liu-shui-xian-qing-can-kao" id="rpmdemo2.-chuang-jian-yi-tiao-kong-liu-shui-xian-qing-can-kao"></a>

### **3. 添加2-1的**[**Linux Job**](https://iwiki.woa.com/pages/viewpage.action?pageId=10718789)**，选择“tlinux2.2” docker镜像** <a href="#rpmdemo3.-tian-jia-21-de-linuxjob-xuan-ze-tlinux2.2docker-jing-xiang" id="rpmdemo3.-tian-jia-21-de-linuxjob-xuan-ze-tlinux2.2docker-jing-xiang"></a>

### **4. 在 Job 内以此添加如下插件** <a href="#rpmdemo4.-zai-job-nei-yi-ci-tian-jia-ru-xia-cha-jian" id="rpmdemo4.-zai-job-nei-yi-ci-tian-jia-ru-xia-cha-jian"></a>

#### 4.1 [拉取Git（命令行）](http://devops.oa.com/console/store/atomStore/detail/atom/gitCodeRepo)：将rpm工程拉取到 Linux Job 内  <a href="#rpmdemo4.1-la-qu-git-ming-ling-hang-jiang-rpm-gong-cheng-la-qu-dao-linuxjob-nei" id="rpmdemo4.1-la-qu-git-ming-ling-hang-jiang-rpm-gong-cheng-la-qu-dao-linuxjob-nei"></a>

![](<../../.gitbook/assets/image (10) (1).png>)

#### 4.2 [Bash](http://docs.devops.oa.com/%E6%89%80%E6%9C%89%E6%9C%8D%E5%8A%A1/%E6%B5%81%E6%B0%B4%E7%BA%BF/%E7%94%A8%E6%88%B7%E6%8C%87%E5%8D%97/%E6%8F%92%E4%BB%B6%E5%88%97%E8%A1%A8/script.html)：替换 rpm spec 文件的 Release 字段 <a href="#rpmdemo4.2bash-ti-huan-rpmspec-wen-jian-de-release-zi-duan" id="rpmdemo4.2bash-ti-huan-rpmspec-wen-jian-de-release-zi-duan"></a>

![](<../../.gitbook/assets/image (1).png>)

### **5. 运行流水线，观察结果** <a href="#rpmdemo5.-yun-hang-liu-shui-xian-guan-cha-jie-guo" id="rpmdemo5.-yun-hang-liu-shui-xian-guan-cha-jie-guo"></a>

![](<../../.gitbook/assets/image (6).png>)

### **6. 到腾讯软件源的**[**RPM内部组件仓库**](http://mirrors.tencent.com/#/private/rpm)**确认 RPM 是否推送成功** <a href="#rpmdemo6.-dao-teng-xun-ruan-jian-yuan-de-rpm-nei-bu-zu-jian-cang-ku-que-ren-rpm-shi-fou-tui-song-che" id="rpmdemo6.-dao-teng-xun-ruan-jian-yuan-de-rpm-nei-bu-zu-jian-cang-ku-que-ren-rpm-shi-fou-tui-song-che"></a>

![](<../../.gitbook/assets/image (19) (1).png>)

\
\
