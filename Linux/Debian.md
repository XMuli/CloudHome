### Debian

​		**Debian** 是一个大型的、历史悠久的志愿者组织。因此，它具有许多需要遵守的社会上和技术上的规则和惯例

**Debian 的社会动力学：**

- 我们都是志愿者

  ​	任何人都不能把事情强加给他人。

  ​	您应该主动地做自己想做的事情。

  
  
- 友好的合作是我们前行的动力

  ​	您的贡献不应致使他人增加负担。
  
  ​	只有当别人欣赏和感激您的贡献时，它才有真正的价值。
  
  
  
 - **Debian** 并不是一所学校

   ​	在这里没有所谓的老师会自动地注意到您。您需要有自学大量知识和技能的能力。

   ​	其他志愿者的关注是非常稀缺的资源。

   

 - **Debian** 一直在不断进步

   ​	**Debian** 期望您制作出高质量的软件包。

   ​	您应该随时调整自己来适应变化。



### Debian 社区中常见的角色

​		**上游作者（upstream author）**：程序的原始作者。

​		**上游维护者（upstream maintainer）**：目前在上游维护程序代码的人。

​		**软件包维护者（maintainer）**：制作并维护该程序 **Debian** 软件包的人。

​		**赞助者（sponsor）**：帮助维护者上传软件包到 **Debian** 官方仓库的人（在通过内容检查之后）。

​		**导师（mentor）**：帮助新手维护者熟悉和深入打包的人。

​		**Debian** **开发者**（**DD, Debian Developer**）：**Debian** 社区的官方成员。**DD** 拥有向 **Debian** 官方仓库上传的全部权限。

​		**Debian 维护者**（**Debian Maintainer, DM**）：拥有对 **Debian** 官方仓库部分上传权限的人。



### 贡献流程

```````python
if exist_in_debian(program):
  if is_team_maintained(program):
    join_team(program)
  if is_orphaned(program) # maintainer: Debian QA Group
    adopt_it(program)
  elif is_RFA(program) # Request for Adoption
    adopt_it(program)
  else:
    if need_help(program):
      contact_maintainer(program)
      triaging_bugs(program)
      preparing_QA_or_NMU_uploads(program)
    else:
      leave_it(program)
else: # new packages
  if not is_good_program(program):
    give_up_packaging(program)
  elif not is_distributable(program):
    give_up_packaging(program)
  else: # worth packaging
    if is_ITPed_by_others(program):
      if need_help(program):
        contact_ITPer_for_collaboration(program)
      else:
        leave_it_to_ITPer(program)
    else: # really new
      if is_applicable_team(program):
        join_team(program)
      if is_DFSG(program) and is_DFSG(dependency(program)):
        file_ITP(program, area="main") # This is Debian
      elif is_DFSG(program):
        file_ITP(program, area="contrib") # This is not Debian
      else: # non-DFSG
        file_ITP(program, area="non-free") # This is not Debian
      package_it_and_close_ITP(program)
```````



### 关于  debmake  /  dh_make 

​		实现同样功能工具(生成 **debian** 目录和模板文件)， **debmake** 命令设计时便旨在替代历史上由 **dh_make** 命令所提供的功能，**debmake** 拥有更多的新特性和功能

​		生成的模板文件注释行以 **#** 开始，其中包含一些教程性文字。在将软件包上传至 **Debian** 仓库之前必须删除或者修改这样的注释行 

​		这些工具的目的是为软件包维护者提供开始工作的模板文件，但他们不是必需品，如果你熟悉的话，可以仅仅使用一个文本编辑器来完成。



### 关于 [dpkg-buildpackage](https://manpages.debian.org/buster/dpkg-dev/dpkg-buildpackage.1.en.html) / debuild  /  pbuilder 

​		都是 deb 包构建工具

​		**[dpkg-buildpackage](https://manpages.debian.org/buster/dpkg-dev/dpkg-buildpackage.1.en.html)**
​				**dpkg-source --before-build**                  应用补丁
​				**dpkg-checkbuilddeps**						  检查依赖和冲突
​				**fakeroot  debian/rules clean**                清理源码目录
​				**dpkg-source -b .**                                    构建上游源码压缩包
​				**fakeroot debian/rules binary**                构建 **deb** 包
​				**dpkg-genbuildinfo**		  					   生成 **buildinfo** 文件
​				**dpkg-genchanges**								  生成 **changes** 文件
​				**dpkg-source --after-build**				      取消应用补丁
​				**debsign** 												  对   ***.dsc** , ***.changes** 和 **buildinfo ** 文件进行签名



​		**[debuild](https://manpages.debian.org/buster/devscripts/debuild.1.en.html)**
​				**debuild** 是封装了 **dpkg-buildpackage** 的一个打包工具,提供了更多的特性,默认会运行 **lintian** 对 **changes** 文件检查，会读 **.devscripts** 配置文件



​		**pbuilder**
​				提供了一个 **chroot** 环境构建包

​				**参考：https://pbuilder-team.pages.debian.net/pbuilder/**

​						   **http://manpages.ubuntu.com/manpages/bionic/man8/pbuilder.8.html**

​						



### 关于 debian 目录

1. **debian/rules**

   用于实际构建 **Debian** 软件包的可执行脚本

   这个文件事实上是另一个 `Makefile`，但不同于上游源代码中的那个。和 `debian` 目录中的其他文件不同，这个文件被标记为可执行。

   

   每一个 `rules` 文件， 就像其他的 `Makefile` 一样，包含着若干 rules，其中每一个都定义了一个 target 以及其具体 操作。一个新的 rule 以自己的 target 声明(置于第一列)来起头。 后续的行都以 TAB 字符 (ASCII 9) 来开头，以指示 target 的具体行为。 空行和以井号 `#` 开头的行会被当作注释而被忽略。

   当你想要执行一个 rule 的时候，就将 target（目标）名称作为命令行参数来调用。

   

   以下是对各 target 的简单解释：

   - `clean` target：清理所有编译的、生成的或编译树中无用的文件。(必须)

   - `build` target：在编译树中将代码编译为程序并生成格式化的文档。(必须)

   - `build-arch` target：在编译树中将代码编译为依赖于体系结构的程序。(必须)

   - `build-indep` target：在编译树中将代码编译为独立于平台的格式化文档。(必须)

   - `install` target：把文件安装到 `debian` 目录内为各个二进制包构建的文件树。如果有定义，那么 `binary*` target 则会依赖于此 target。(可选)

   - `binary` target：创建所有二进制包(是 `binary-arch` 和 `binary-indep` 的合并)。(必须)[[39\]](https://www.debian.org/doc/manuals/maint-guide/dreq.zh-cn.html#ftn.idm1576)

   - `binary-arch` target：在父目录中创建平台依赖(`Architecture: any`)的二进制包。(必须)[[40\]](https://www.debian.org/doc/manuals/maint-guide/dreq.zh-cn.html#ftn.idm1584)

   - `binary-indep` target：在父目录中创建平台独立(`Architecture: all`)的二进制包。(必须)[[41\]](https://www.debian.org/doc/manuals/maint-guide/dreq.zh-cn.html#ftn.idm1592)

   - `get-orig-source` target：从上游站点获得最新的原始源代码包。(可选)

     

   ### 4.4.2. 默认的 `rules` 文件

   新版本的 **dh_make** 会生成一个使用 **dh** 命令的非常简单但非常强大的默认的 `rules` 文件

   

   ``````shell
   #!/usr/bin/make -f
   # You must remove unused comment lines for the released package.
   #export DH_VERBOSE = 1
   #export DEB_BUILD_MAINT_OPTIONS = hardening=+all
   #export DEB_CFLAGS_MAINT_APPEND  = -Wall -pedantic
   #export DEB_LDFLAGS_MAINT_APPEND = -Wl,--as-needed
   
   %:
   	dh $@  
   
   #override_dh_auto_install:
   #	dh_auto_install -- prefix=/usr
   
   #override_dh_install:
   #	dh_install --list-missing -X.pyc -X.pyo
   ``````

   **DH_VERBOSE** 环境变量可以强制 **debhelper** 工具输出更详细的构建信息。

   **DEB_BUILD_MAINT_OPTION** 变量可以控制使用或禁用一些功能和选项，具体参考 **[dpkg-buildflags](https://www.man7.org/linux/man-pages/man1/dpkg-buildflags.1.html) **手册页中“FEATURE AREAS/ENVIRONMENT”部分

   **DEB_CFLAGS_MAINT_APPEND** 可以强制 C 编译器给出所有类型的警告内容。

   **DEB_LDFLAGS_MAINT_APPEND** 可以强制链接器只对真正需要的库进行链接。

   第 8 和 9 行使用了 pattern rule，以此隐式地完成所有工作。 其中的百分号意味着“任何 targets”， 它会以 target 名称作参数调用单个程序 **dh**。 **dh** 命令是一个包装脚本，它会根据参数执行妥当的 **dh_\*** 程序序列。 

   - `debian/rules clean` 运行了 `dh clean`，接下来实际执行的命令为：

     ```
     dh_testdir
     dh_auto_clean
     dh_clean
     ```

   - `debian/rules build` 运行了 `dh build`，其实际执行的命令为：

     ```
     dh_testdir
     dh_auto_configure
     dh_auto_build
     dh_auto_test
     ```

     

   **dh_\*** 命令的功能依其名称不言而喻。不过其中有一些值得在这里进行简要解释， 假定有一个基于 `Makefile` 的典型构建环境：

   - **dh_auto_configure** 在 `./configure` 存在时通常执行以下命令(省略部分参数以方便此处阅读)。

     ```
     ./configure --prefix=/usr --sysconfdir=/etc --localstatedir=/var ...
     ```

   - **dh_auto_build** 通常使用以下命令执行 `Makefile` 中的第一个 target。

     ```
     make
     ```

   - **dh_auto_test** 通常在 `Makefile` 存在且有 `test` target 时执行以下命令。

     ```
     make test
     ```

   - **dh_auto_install** 通常在 `Makefile` 存在且有 `install` target 时执行以下命令(进行了换行以便阅读)。

     ```
     make install \
       DESTDIR=/path/to/package_version-revision/debian/package
     ```

   所有需要 **fakeroot** 命令的都包含了 **dh_testroot**。如果你没有使用 fakeroot，那将会报错并退出。

   关于 **dh_make** 生成的 `rules` 文件，你应该知道的最重要的事是，它仅仅是一个建议。它对多数简单的软件包有效，但对于更复杂的则要大胆对其进行定制以满足需要。

   

2. **debian/control**

   软件包配置文件包含了源码包名称、源码构建依赖、二进制软件包名称、二进制软件包依赖，等等

   ``````shell
     1 Source: demo        
     2 Section: unknown
     3 Priority: optional
     4 Maintainer: Clay Stan <claystan97@gmail.com>
     5 Build-Depends: debhelper-compat (= 13)
     6 Standards-Version: 4.5.1
     7 Homepage: <insert the upstream URL, if relevant>
     8 #Vcs-Browser: https://salsa.debian.org/debian/demo
     9 #Vcs-Git: https://salsa.debian.org/debian/demo.git
    10 Rules-Requires-Root: no
    11 
    12 Package: demo
    13 Architecture: any
    14 Depends: ${shlibs:Depends}, ${misc:Depends}
    15 Description: <insert up to 60 chars description>
    16  <insert long description, indented with spaces>                                               
   ``````

   **第 1–9 行**是源代码包的信息。第 12–16 行是二进制包的控制信息。

   **第 2 行**是该源码包要进入发行版中的分类。

   **Debian** 仓库被分为几个类别：`main` (自由软件)、`non-free` (非自由软件)以及 `contrib` (依赖于非自由软件的自由软件)。

   

   ##### 关于 **main / contrib / non-free**

   - 对于仓库中 `main` 区的软件， **Debian Policy** 要求其 **完全兼容 Debian Free Software Guidelines （Debian 自由软件准则）** ([DFSG](http://www.debian.org/social_contract#guidelines)) 并且它 **不能要求使用 `main`** 区以外的软件来编译或执行。

   - 对于仓库中 `contrib` 区的软件，其许可证必须满足 **DFSG** 的全部条件，不同于 main 区软件的一点是，它们可以依赖于 `main` 之外的软件包来完成编译或运行。

   - 对于仓库中 `non-free` 区的软件，其许可证可以不满足 **DFSG** 中的一部分条件。其中坚决不能违背的一点是，该软件 **必须是可分发的**。

   

   在这些大的分类之下还有多个逻辑上的子分类，用以简短描述软件包的用途类别。`admin` 为供系统管理员使用的程序，`devel` 为开发工具，`doc` 为文档，`libs` 为库，`mail` 为电子邮件阅读器或邮件系统守护程序，`net` 为网络应用程序或网络服务守护进程，`x11` 为不属于其他分类的为 X11 程序，此外还有很多很多。

   [详细参考](https://www.debian.org/doc/debian-policy/ch-archive.html#s-subsections)

   [参考2](https://packages.debian.org/unstable/)

   
   
   **Section: unknown** 不做修改的话，后续的 **lintian** 错误可能导致构建失败。
   
   
   
   **第 3 行**描述了用户安装此软件包的优先级
   
   每个软件包必须具有一个*优先级*值，该信息用于控制标准或最小 **Debian** 安装中包括哪些软件包。
   
   大多数 **Debian **软件包的优先级为`optional`。除 optional 外的优先级的包都是包含在 **Debian** 标准安装列表的
   
	`optional` 优先级适用于与优先级为 `required`、`important` 或 `standard` 的软件包不冲突的新软件包
   

   
   优先级分类如下：
   - **required**
   	 系统正常运行所必需的软件包（通常，这意味着 dpkg 依赖这些软件包）
     
      卸载`required`软件包可能会导致系统完全损坏，甚至可能 dpkg 都无法将他们安装回来，因此只有在知道自己在做什么的情况下才这样做。
   
      仅安装了 `required` 软件包的系统至少可以启动系统并安装更多软件
	
   	​	
		
   - **important**
   	重要的程序，包括在任何类Unix系统上都能找到的程序。
   
   	如果没有这些包，系统和其他软件包可能无法正常运行。
   
   
   
   - **standard**
   	
   	这些软件包提供了一个相当小但不太受限的字符模式系统。这是默认情况下，如果用户不选择任何其他选项，将安装的内容。它不包括很多大型应用程序。
   	
   	两个优先级都为 **standard** 或优先级更高的包不能相互冲突。
   	   	
   	
   - **optional**
   
     这是大多数存档的默认优先级。除非默认情况下在标准 **Debian** 系统上安装了该软件包，否则软件包的优先级应为**`optional`**。
   
     优先级为的软件包**`optional`**可能会相互冲突。
   
     
   
   - **extra**
     此优先级已弃用。请改为使用 **optional** 优先级。此优先级被视为等同于 **optional** 的。
     **extra** 优先级以前用于与其他包冲突的包，以及只对有特殊需求的人有用的包。然而，这一区别有点武断，没有一贯遵循，也不足以保证维护工作。
   
     
   
   **Section** 和 **Priority** 常被如 **aptitude** 的前端所使用，以分类软件包并选择默认值。一旦你把软件包上传到 **Debian**，这两项的值可以被仓库维护人员修改，此时你将收到提示邮件
   
     
   
   **第 4 行**是维护者的姓名和电子邮件地址。
   
   
   
   **第 5 行**中的 `Build-Depends` 项列出了编译此软件包需要的软件包。有些被 `build-essential` 依赖的软件包，如 `gcc` 和 `make` 等，已经会被默认安装而不需再写到此处。如果你需要其他工具来编译这个软件包，请将它们加到这里。多个软件包应使用半角逗号分隔
   
   - 对于所有在 `debian/rules` 文件中使用 **dh** 命令打包的软件包，必须在 `Build-Depends` 中包含 `debhelper (>=9)` 以满足 Debian Policy 中对 `clean` target 的要求。
   - 对于生成有标记过 `Architecture: any` 的二进制包的源码包，它们将被 autobuilder 重构建。因为 autobuilder 过程在仅安装 `Build-Depends` 中列出的程序前便执行 `debian/rules build` 中的内容，`Build-Depends` 字段需要列出所有必须的编译依赖，而 `Build-Depends-Indep` 则很少使用。
   - 对于生成全标记 `Architecture: all` 二进制包的源码包，`Build-Depends-Indep` 中应列出所有要求的软件包，除非 `Build-Depends` 中已经列出，这样以便满足Debian Policy 中对 `clean` target 的要求。
   
   如果你不知道应该使用哪一个，则使用 `Build-Depends` 以保证安全
   
   
   
   **第 6 行**是此软件包所依据的 [Debian Policy Manual](http://www.debian.org/doc/devel-manuals#policy) 标准版本号。
   
   **第 7 行**可以放置上游项目首页的**URL**
   
   **第 9 行**是二进制软件包的名称。通常情况下与源代码包相同，但不是强制的
   
   **第 10 行**描述了可以编译本二进制包的体系结构。根据二进制包的类型，这个值常常是下列中的一个：
   
   - `Architecture: any`
   
     ​	一般而言，包含 编译型语言编写的程序 生成的二进制包依赖于具体的体系结构。`any`与所有 **Debian** 机器架构匹配，并且是最常用的。
   
   - `Architecture: all`
   
     ​	一般而言，包含 文本、图像、或解释型语言脚本 生成的二进制包独立于体系结构。all 表示与体系结构无关的软件包。
   
     
   
   此字段可以包含特殊值`all`，特殊体系结构通配符 `any`或由空格分隔的特定体系结构和通配符体系结构的列表。如果`all`或`any`出现，则该值必须是该字段的全部内容。大多数软件包应使用`all`或`any`。
   
   指定特定的体系结构列表表示源仅在列表中包含的体系结构上构建依赖于体系结构的程序包。指定体系结构通配符列表表示源仅在与任何指定的体系结构通配符匹配的那些体系结构上构建依赖于体系结构的程序包
   
   
   
   **第 11 行** 展示了 **Debian** 软件包系统中最强大的特性之一。每个软件包都可以和其他软件包有各种不同的关系。除 `Depends` 外，还有 `Recommends`、`Suggests`、`Pre-Depends`、`Breaks`、`Conflicts`、`Provides` 和 `Replaces`。
   
   
   
   **关于软件包关系的简述**：
   
   - `Depends`
   
     此软件包仅当它依赖的软件包均已安装后才可以安装。此处请写明你的程序所必须的软件包，如果没有要求的软件包该软件便不能正常运行（或严重抛锚）的话。
   
     
   
   - `Recommends`
   
     这项中的软件包不是严格意义上必须安装才可以保证程序运行。当用户安装软件包时，所有前端工具都会询问是否要安装这些推荐的软件包。**aptitude** 和 **apt-get** 会在安装你的软件包的时候自动安装推荐的软件包(用户可以禁用这个默认行为)。**dpkg** 则会忽略此项。
   
     
   
   - `Suggests`
   
     此项中的软件包可以和本程序更好地协同工作，但不是必须的。当用户安装程序时，所有的前端程序可能不会询问是否安装建议的软件包。**aptitude** 可以被配置为安装软件时自动安装建议的软件包，但这不是默认。**dpkg** 和 **apt-get** 将忽略此项。
   
     
   
   - `Pre-Depends`
   
     此项中的依赖强于 `Depends` 项。软件包仅在预依赖的软件包已经安装并且 *正确配置* 后才可以正常安装。在使用此项时应 *非常慎重*，仅当在 [debian-devel@lists.debian.org](http://lists.debian.org/debian-devel/) 邮件列表讨论后才能使用。记住：根本就不要用这项。
   
     
   
   - `Conflicts`
   
     仅当所有冲突的软件包都已经删除后此软件包才可以安装。当程序在某些特定软件包存在的情况下根本无法运行或存在严重问题时使用此项。
   
     
   
   - `Breaks`
   
     此软件包安装后列出的软件包将会受到损坏。通常 `Breaks` 要附带一个“版本号小于多少”的说明。这样，软件包管理工具将会选择升级被损坏的特定版本的软件包作为解决方案。
   
     
   
   - `Provides`
   
     某些类型的软件包会定义有多个备用的虚拟名称。你可以在 [virtual-package-names-list.txt.gz](http://www.debian.org/doc/packaging-manuals/virtual-package-names-list.txt)文件中找到完整的列表。如果你的程序提供了某个已存在的虚拟软件包的功能则使用此项。
   
     
   
   - `Replaces`
   
     当你的程序要替换其他软件包的某些文件，或是完全地替换另一个软件包(与 `Conflicts` 一起使用)。列出的软件包中的某些文件会被你软件包中的文件所覆盖。
   
     
   
   所有的这些项都使用统一的语法。它们是一个软件包列表，软件包名称间使用半角逗号分隔。也可以写出有多个备选软件包名称，这些软件包使用 `|` 符号(管道符)分隔。
   
   这些项内还可以限定与某些软件包的某个版本区间之间的关系。版本号限定在括号内，这紧随软件包名称之后，并在以下逻辑符号后写清具体版本：`<<`、`<=`、`=`、`>=` 和 `>>`，分别代表严格小于、小于或等于、严格等于、大于或等于以及严格大于。例如，
   
   ```
   Depends: foo (>= 1.2), libbar1 (= 1.3.4)
   Conflicts: baz
   Recommends: libbaz4 (>> 4.0.7)
   Suggests: quux
   Replaces: quux (<< 5), quux-foo (<= 7.6)
   ```
   
   
   
   最后一个需要了解的特性是 `${shlibs:Depends}`,`${misc:Depends}`, 之类。
   
   **dh_shlibdeps** 会为二进制包计算共享库依赖关系。它会为每个二进制包生成一份 [ELF](http://en.wikipedia.org/wiki/Executable_and_Linkable_Format) 可执行文件和共享库列表。 这个列表用于替换 `${shlibs:Depends}`。
   
   一些 `debhelper` 命令可能会使生成的软件包需要依赖于某些其他的软件包。所有这些命令将会为每一个二进制包生成一个列表。这些列表将用于替换 `${misc:Depends}` 。
   
   **dh_gencontrol** 会为每个二进制包生成 `DEBIAN/control` 替换 `${shlibs:Depends}`, `${perl:Depends}`, `${misc:Depends}`
   
   
   
   **第 12 行**是简述。绝大多数人的屏幕是 80 列宽，所以描述不应超过 60 个字符。
   
   **第 13 行**是长描述开始的地方。这应当是一段更详细地描述软件包的话。每行的第一个格应当留空。描述中不应存在空行，如果必须使用空行，则在行中仅放置一个 `.` (半角句点)来近似。同时，长描述后也不应有超过一行的空白。

3. **debian/changelog**

   记录了 **Debian** 软件包的历史并在其第一行定义了上游软件包的版本和 **Debian** 修订版本。

   所有改变的内容应当以明确、正式而简明的语言风格进行记录

   

   关于包名和版本号命名规则正则表达式

   ``````text
   上游软件包名称（**-p**）：[-+.a-z0-9]{2,}
   
   二进制软件包名称（**-b**）：[-+.a-z0-9]{2,}
   
   上游版本号（）：[0-9][-+.:~a-z0-9A-Z]
   
   *Debian 修订版本（**-r**）： [0-9][+.~a-z0-9A-Z]*
   ``````

   为了能有效地使用一些流行的工具（如 **aptitude**）管理软件包名称和版本信息，最好能将软件包名称保持在 30 字符以下；版本号和修订号加起来最好能不超过 14 个字符

   

   通常使用 **debchange** 命令（它具有一个别名，即 **dch **）对其进行编辑添加或修改

   环境变量，`$DEBEMAIL` 和 `$DEBFULLNAME`  可以被读取到

   

   该文件将由 **dh_installchangelogs** 命令安装到 **/usr/share/doc/***binarypackage* 目录，文件名为 **changelog.Debian.gz**。

   

4. **debian/copyright**

   这个文件包含了上游软件的版权以及许可证信息

   每个软件包都必须在`/usr/share/doc/PACKAGE/copyright`随附其发行许可证的完整副本。该文件既不能压缩也不能是符号链接。

   此外，版权文件必须说明上游来源（如果有）的来源，并应包括上游作者的姓名或联系地址。这可以是个人或组织的名称，电子邮件地址，Web论坛或Bugtracker或任何其他方式来明确标识与谁联系以及参与上游源代码开发的方法。

   在 **contrib** 或了 **non-free** 存档区域中的包应该在版权文件中声明该包不是 **Debian** 发行版的一部分，并简要解释原因。

   位于源包中的 `debian/copyright`  将安装在/usr/share/doc/PACKAGE/copyright中

   根据Apache许可（2.0版），Artistic许可，Creative Commons CC0-1.0许可，GNU GPL（1、2或3版），GNU LGPL（2、2.1或3版）， GNU FDL（1.2或1.3版）和Mozilla公共许可证（1.1或2.0版）应引用`/usr/share/common-licenses`， 而不是在 copyright 文件中引用它们。

   不要将版权文件用作常规`README`文件。

   所有 copoyright 文件必须用 UTF-8 编码。

   

   [文件格式规范](https://dep-team.pages.debian.net/deps/dep5/)

   

   

   

5. **debian/compat**

   **debhelper **有时候会有重大更新，并且不向后兼容，为了防止此类重大更改破坏现有程序包，引入了 **debhelper** 兼容性级别的概念

   

   旧版本的 **debhelper** 要求在文件 **debian/compat** 中指定兼容性级别，**debian/compat**  应该仅包含表示兼容级别的数字，而不能包含其他内容。如果通过此方法指定兼容性级别，则还需要指定一个等于或大于等于 兼容性级别的 **debhelper**  软件包版本。例如，如果您在 **debian/compat** 中指定兼容性级别13，请确保 **debian/control** 具有：**Build-Depends: debhelper (>=13~)**

   

   目前可以通过在 **debian/control** 中的 **Build-Depend** 字段指定的兼容性级别，例如，要使用 **v13** 模式，只需在 **debian/control** 添加：**Build-Depends: debhelper-compat (= 13)**这也可以作为对 **debhelper** 软件包的版本依赖，因此无需指定 **debhelper** 版本，除非您需要特定版本的 **debhelper**

   

   **[read more about this](https://www.man7.org/linux/man-pages/man7/debhelper.7.html)**

   

6. **debian/soruce/format**

   此源代码包的格式 (查看 dpkg-source(1) 获得完整列表)

   在 `squeeze` 后，它应该是以下二者之一：

   `3.0 (native)` - Debian 本土软件

   `3.0 (quilt)` - 其他所有软件

   全新的 `3.0 (quilt)` 源代码格式将所有修改使用 **quilt** 补丁系列记录到 `debian/patches`。这些修改会在解压源代码包时自动应用。Debian 修改保存于 `debian.tar.gz` 归档文件，其中包含了整个 `debian` 目录。这个新格式支持直接添加例如 PNG 图标等的二进制文件

   **dpkg-source** 解压 `3.0 (quilt)` 格式的源码包时会自动应用所有列于 `debian/patches/series` 的补丁。你可以使用 `--skip-patches` 选项避免在解压后自动应用补丁。

   

   推荐使用 **3.0 (quit)** 格式

   **3.0 (quit)** 格式不再需要 debian/README.source 文件

   如果 **lintian** 报 **warning** ,可以忽略，或升级 **lintian** 到 **sid** 的最新版本

   

   **3.0 (native)**

   native **Debian** 软件包不对 **上游代码** 和 **Debian 的修改** 进行区分，仅包含以下内容：

   *package_version*.**tar.gz**(源码压缩包)

   *package_version*.**dsc**

   (native会多一个 .debian.tar.xz 并且需要先创建源码压缩包)

   

   如果使用 **3.0 (native)** 格式，不必事先创建 tarball 压缩包。要创建一个 **3.0 (native)** 格式 Debian 软件包，应当将 **debian/source/format** 文件的内容设置为“**3.0 (native)**”，修改 **debian/changelog** 文件使得版本号中不包含 **Debian** 修订号（例如，**1.0** 而非 **1.0-1**），最后在源码树中调用“**dpkg-source -b .**”命令。这样做便可以自动生成包含源代码的 tarball。

   

   更多信息：https://wiki.debian.org/Projects/DebSrc3.0

   

   

7. **debian/patches**

   旧的 `1.0` 源代码包格式使用单一的大 `diff.gz` 文件为源码保存 `debian` 中的维护文件和补丁。这样的软件包比较难于在事后检查和分析。这不是很好。

   新的 `3.0 (quilt)` 源码格式将补丁存储在 `debian/patches/*` 中，用 **quilt** 命令。 这些补丁和其他 `debian` 目录下的打包数据都会被打包成 `debian.tar.gz` 文件。由于 **dpkg-source** 命令可以处理 **quilt** 格式的补丁数据 (在格式为 `3.0 (quilt)` 的源码中)，而不需要 `quilt` 软件包；不需要在 `Build-Depends` 中添加 `quilt`

   


**修改上游代码生成patch**

   ​		**dpkg-source --commit**



#### **debian** 目录下的其他文件

要控制 `debhelper` 在构建软件包过程中的大部分行为，可以在 `debian` 目录中放置可选的配置文件

**dh_make** 命令会在 `debian` 目录中创建一些配置文件模板，它们的文件名多带有 `.ex` 后缀。其中的一些可能以二进制包名作为前缀

还有一些 `debhelper` 的配置文件模板可能未被 **dh_make** 命令创建。在这种情况下如果你需要它们，则要使用文本编辑器手工创建

如果你希望或需要使用它们中的任意一个或多个：

- 如果模板文件带有 `.ex` 或 `.EX` 后缀，则重命名它去掉后缀；

- 把配置文件的名称改为实际的二进制软件包名，例如 `package`;

- 修改模板文件来满足你的需要;

- 删除不需要的模板文件;

  

任何不带有 `*`package`*` 前缀的`debhelper` 配置文件， 比如应用到第一个二进制包的 `install`。 当此处有多个二进制包时，可以通过在文件名中前置 它们各自的名称来指定它们各自的配置文件，比如`*`package-1`*.install`, `*`package-2`*.install`, 之类



## `README.Debian`

所有关于你的 Debian 版本与原始版本间的额外信息或差别都应在这里记录。**dh_make** 创建了一个默认的文件

如果你没有需要写在这里的东西，则删除这个文件



## `conffiles`

关于软件有件很恼人的事，就是当你付出了很多时间和精力来自定义一个程序，但是升级后所有的修改都被覆盖掉了。Debian 通过将配置文件单独标记来解决这个问题， 当软件包升级的时候，你将会被询问是否要保留你的旧配置文件。

如果你的程序使用配置文件，但程序会自动对配置文件进行改写，则最好别将其标记为 conffiles ，因为 **dpkg** 总是会要求用户校验变更。



## `package.cron`

如果你的软件包需要有计划运行的操作以保证正常工作，可以使用这个文件达成。你既可以设置常规的任务使其在每小时、每天、每星期、每月或其他情况或你希望的时间下运行。相应的文件名是

`*`package`*.cron.hourly` - 安装至 `/etc/cron.hourly/*`package`*` ；每小时运行一次。

`*`package`*.cron.daily` - 安装至 `/etc/cron.daily/*`package`*` ；每天运行一次。

`*`package`*.cron.weekly` - 安装至 `/etc/cron.weekly/*`package`*` ；每周运行一次。

`*`package`*.cron.monthly` - 安装至 `/etc/cron.monthly/*`package`*` ：每月运行一次。

`*`package`*.cron.d` - 安装至 `/etc/cron.d/*`package`*`： 对于其他的任何时间。

这些文件均为 shell 脚本。唯一不同的是 `*`package`*.cron.d`，它的格式需要参照 crontab(5)



## `install`

如果你的软件包需要那些标准的 `make install` 没有安装的文件，你可以把文件名和目标路径写入 `install` 文件，它们将被 dh_install(1) 安装。

这个 `install` 文件每行安装一份文件，格式上先是相对于编译目录的文件路径，然后是一个空格，接下来是相对于安装目录的目标目录。例如，假设某个二进制文件 `src/`bar 没有被默认安装，可以这样：

```
src/bar usr/bin
```

这意味着安装这个软件包时，将有一个二进制文件 `/usr/bin/bar`.

当然，在相对路径保持不变的情况下 `install` 文件也可以只包含相对的源路径而不带目标位置。这样的格式通常用于使用 `*`package-1`*.install`、`*`package-2`*.install` 等将大软件包分割为多个二进制包的情况。

如果 **dh_install** 命令没有在当前目录(或者你可能使用 `--sourcedir` 参数指定的位置)找到文件，它会回滚至使用 `debian/tmp` 目录。



##  `package.info`

如果你的软件包有 info 信息页，应该使用 dh_installinfo安装，要安装的文件列于 `*`package`*.info` 文件中。



## `package.links`

如果你作为包维护者需要在包中创建附加的符号链接，你应该使用 dh_link(1) 来安装，要安装的文件完整路径列于 `*`package`*.info` 文件中。



## `manpage.*`

你的程序应该有 man 手册页，如果它们没有自带则需要由你来创建。**dh_make** 命令创建了几个 man 手册页的模板。为每一个缺手册的命令拷贝一份模板，并妥善地编写，并且要删除未使用的模板。   



## `package.manpages`

如果你的软件包有 man 手册页，你应该将它们列在 `*`package`*.manpages` 文件中以便 dh_installman(1) 进行安装



## `{pre,post}{inst,rm}`

`postinst`、`preinst`、`postrm` 和 `prerm` 文件 被称为 *maintainer scripts*。它们是放置于软件包控制区域，并由 **dpkg** 在软件包安装、升级或卸载时执行的脚本。

## `watch`

`watch` 文件的格式被详述于 uscan(1) man 手册页中。`watch` 文件配置了 **uscan** 程序以便监视你获得原始源代码的网站。它还被用于 [Debian 软件包跟踪系统（PTS）](https://tracker.debian.org/) 服务。




### DESTDIR  &&  prefix  变量

​	**DESTDIR**

​	一般用于包的构建中，实现分阶段安装，和告诉那些想知道包中文件安装位置的用户

​	**debian/rules build** 会构建源代码并安装到 **$(DESTDIR)** 中

- **DESTDIR = debian/*binarypackageName/***（单二进制包）
- **DESTDIR = debian/tmp/**（多个二进制包）



​	**prefix**

​	一般遵循 [**GNU coding standards**](https://www.gnu.org/prep/standards/) 和 [**Filesystem Hierarchy Standard**](https://en.wikipedia.org/wiki/Filesystem_Hierarchy_Standard)

​	**Makefile** 中的 **prefix** 的值默认为 **/usr/local** ，**Debian** 的打包为了将安装文件变成系统文件的一部分，**Makefile** 文件中 **$(prefix)** 默认的 **/usr/local** 的值需要被覆盖为 **/usr**。 	在 **debian/rules** 文件中添加名为 **override_dh_auto_install** 的目标，在其中设置“**prefix=/usr**”。

​	

### 构建多个包

​	为所有二进制软件包在 **debian/control** 文件中创建对应的二进制软件包条目。

​	在对应的 **debian/* **二进制软件包名 ***.install**  文件中列出所有文件的路径（相对于 **debian/tmp** 目录）。





### 关于 lintian 检查

**Lintian**是Debian软件包的包检查器。

它会检查

- 违反Debian政策以及违反各种子政策的行为，

- 更好的做法

- 常见错误

  

​	**lintian** 的 **tag** 

- `E:` 代表错误：确定违反了 Debian Policy 或是一个肯定的打包错误。
- `W:` 代表警告：可能违反了 Debian Policy 或是一个可能的打包错误。
- `I:` 代表信息：对于特定打包类别的信息。
- `N:` 代表注释：帮助你调试的详细信息。
- `O:` 代表已覆盖：一个被 `lintian-overrides` 文件覆盖的信息，但由于使用 `--show-overrides` 选项而显示。

对于警告，你应该改进软件包或者检查警告是否的确无意义。如果确定没有意义，可以使用 lintian-overrides 将其覆盖。

如果 `lintian` 根据 Debian Policy 的某些规则允许例外从而报告了错误诊断，你可以使用 `package`.lintian-overrides` 或 `source/lintian-overrides` 使其不再警告。请认真阅读 Lintian 用户手册(`https://lintian.debian.org/manual/index.html`) 并避免滥用。`*`package`*.lintian-overrides` 是对于名为 `*`package`*` 的二进制包的有效配置，会由 dh_lintian 命令安装到`usr/share/lintian/overrides/*`package`*`。`source/lintian-overrides 是针对源代码包的，不会安装。



### 包构建后生成的一些文件

1. **pkgName_version.orig.tag.gz**

   上游源码压缩包

   

2. **pkgName_version-rv.debian.tar.xz**

   维护者的debian目录

   

3. **pkgName_version-rv.dsc**

   Debian源码包的元数据文件

   

4. **pkgName_version-rv_arch.deb**

   通过源码构建的二进制软件包

   

5. **pkgName-dbgsym_version-rv.arch.deb**

   是 Debian 的调试符号二进制软件包

   

6. **pkgName_version-rv_arch.build**

   构建日志文件

   

7. **pkgName_version-rv_arch.buildinfo**

    **dpkg-genbuildinfo** 生成的元数据文件

   

8. **pkgName_version-rv_arch.changes**

   **Debian** 二进制软件包的元数据文件



**文档资料来源：**

1. [维护者指南](https://www.debian.org/doc/manuals/debmake-doc/index.zh-cn.html)
2. [新维护者手册](https://www.debian.org/doc/manuals/maint-guide/)
3. [Debian Policy Manual](https://www.debian.org/doc/debian-policy/)
4. [Debian Developer's Reference](https://www.debian.org/doc/manuals/developers-reference/index.en.html)