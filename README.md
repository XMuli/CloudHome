# Home
偕臧的家用小仓库，自用

<br>

- [Qt](https://github.com/xmuli/CloudHome/tree/master/Qt) : `Qt` 文档教程
- [Cmake](https://github.com/xmuli/CloudHome/tree/master/cmake) : `Cmake` 文档教程
- [Linux](https://github.com/xmuli/CloudHome/tree/master/Linux) : `Linux` 文档教程
- [UML](https://github.com/xmuli/CloudHome/tree/master/UML) : UML 文档资料

<br>

[TOC]

<br>

## 经验积累

### Git

#### 01.导出 patch，合并 patch

```bash
git format-patche 795fefabc  # 导出本次之后的commit，不含本次
git am --abort # 放弃掉以前的am信息
git am *.patch # 合并补丁
```

<br>

### Gerrit

#### 01.推送命令

```bash
git push origin HEAD:refs/for/master # 最后的推送格式
```

[wiki 链接](https://wikidev.uniontech.com/index.php?title=Gerrit%E9%85%8D%E7%BD%AE%E5%8F%8A%E7%AE%80%E5%8D%95%E4%BD%BF%E7%94%A8)  

<br>

### 制做 `deb` 包

准备工作：如下写入 `~/.bashrc` 后，执行 `source ~/.bashrc` 立即生效

```bash
DEBEMAIL=”xmulitech@gmail.com”
DEBFULLNAME=”xmuli”
export DEBEMAIL DEBFULLNAME
```



目标：制作 deb 包

```bash
sudo apt install debhelper devscripts dh-make build-essential
sudo apt install g++ cmake libqt5*-dev libdtk{core,widget,gui}-dev # DTK 开发包
dh_make -s --createorig  # ./ 中生成debian 目录; ../ 外面生成 .tar.xz
dpkg-buildpackage -b -rfakeroot -us -uc  # 生成 deb
```

<br>

**参考：**

- [Ubuntu下制作deb包的方法详解](https://github.com/gatieme/AderXCoding/tree/master/system/tools/build_deb)

- [星火-DEBIAN及UOS源码构建参考文档](https://gitee.com/deepin-opensource/document/wikis/UOS%E5%8F%8ADEEPIN%E6%BA%90%E7%A0%81%E6%9E%84%E5%BB%BA%E5%8F%82%E8%80%83%E6%96%87%E6%A1%A3?sort_id=2710162)

<br>