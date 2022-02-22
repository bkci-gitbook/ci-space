# 部署

### 蓝鲸部署（部署版本6.0.5）：

1、环境准备\
[https://bk.tencent.com/docs/document/6.0/127/7543](https://bk.tencent.com/docs/document/6.0/127/7543)\
2、部署\
[https://bk.tencent.com/docs/document/6.0/127/8587](https://bk.tencent.com/docs/document/6.0/127/8587)

### 蓝盾部署（使用标准运维）

\
[https://bk.tencent.com/docs/document/6.0/127/7539](https://bk.tencent.com/docs/document/6.0/127/7539)

蓝盾标准运维模板使用如下文件



bk\_sops\_1\_20220117163838.dat107.7KB



### FAQ：

1、标准运维报错

![](<../.gitbook/assets/image (3).png>)

vim /data/src/ci/scripts/bk-ci-gen-jrezip.sh\
找到 get\_bcprov\_from\_dispatch\_lib\_dir函数\
\## local bcprov\_glob="$BK\_CI\_SRC\_DIR/dispatch/lib/bcprov-jdk16\*.jar"\
\## 替换成\
\## local bcprov\_glob="$BK\_CI\_SRC\_DIR/dispatch/lib/bcprov-jdk\*.jar"

\


2、用户外网下载包超时问题

![](<../.gitbook/assets/image (17).png>)

![](<../.gitbook/assets/image (15).png>)

解决方式：手动下载

![](<../.gitbook/assets/image (35).png>)
