# Linux
Linux 学习：

​		对于Linux编程基础，可以看看这本大书：Linux系统编程手册（The Linux Programming Interface），中文翻译质量也不错，当然内容比较厚：https://item.jd.com/11383763.html

![image-20210127092606077](/home/xmuli/project/github/CloudHome/Linux/README.assets/image-20210127092606077.png)





[TOC]

<br>





### bash 学习

推荐： [https://github.com/wangdoc/bash-tutorial](https://github.com/wangdoc/bash-tutorial/)





<br>

### 打 `deb` 包 

使用 `dh_make`命令， 参考： https://github.com/gatieme/AderXCoding/tree/master/system/tools/build_deb



```bash
sudo apt install debhelper devscripts # 一大堆集合，类似于 base 包，带有 dh_make 包

修改文件名（文件名格式：必须`字母纯小写`，`0~9`，`+`  `-`  `.` ）
修改对应 debian 目录
dh_make --createorig  # 生成 .tar.xz
dpkg-buildpackage -b -rfakeroot -us -uc -d #不签名

-b, --build=binary          binary-only, no source files.
-nc, --no-pre-clean         do not pre clean source tree (implies -b).
     --pre-clean             pre clean source tree (default).
     --no-post-clean         do not post clean source tree (default).
-uc, --unsigned-changes     unsigned .buildinfo and .changes file.
     --no-sign               do not sign any file.
     --force-sign            force signing the resulting files.
     --admindir=<directory>  change the administrative directory.
-us, --unsigned-source      unsigned source package.
 -d, --no-check-builddeps   do not check build dependencies and conflicts.
     --ignore-builtin-builddeps
                            do not check builtin build dependencies.
```



```bash
g++ cmake libqt5*-dev libdtk{core,widget,gui}-dev dde-dock-dev
```



## DDE

```bash
export https_proxy=http://127.0.0.1:8889  # 执行，代理设置如下

ftp_proxy=http://127.0.0.1:8889
http_proxy=http://127.0.0.1:8889
https_proxy=http://127.0.0.1:8889
```



https://cdn.jsdelivr.net/gh/xmuli/xmuliPic@pic