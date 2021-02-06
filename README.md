# Home
偕臧的家用小仓库，自用

<br>

- [Qt](https://github.com/xmuli/CloudHome/tree/master/Qt) : `Qt` 文档教程
- [Cmake](https://github.com/xmuli/CloudHome/tree/master/cmake) : `Cmake` 文档教程
- [Linux](https://github.com/xmuli/CloudHome/tree/master/Linux) : `Linux` 文档教程
- [UML](https://github.com/xmuli/CloudHome/tree/master/UML) : UML 文档资料

<br>
<br>
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

#### 02.推送命令

```bash
git push origin HEAD:refs/for/master # 最后的推送格式
```

[wiki 链接](https://wikidev.uniontech.com/index.php?title=Gerrit%E9%85%8D%E7%BD%AE%E5%8F%8A%E7%AE%80%E5%8D%95%E4%BD%BF%E7%94%A8)  
