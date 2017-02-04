# setuptools

标签（空格分隔）： setuptools

---
[toc]
## 1.基本介绍与特性
对用户来说，使用Setuptools构建和分发的包看起来就像普通的基于distuils的python包。为了使用Setuptools，用户不必安装或了解它们，开发者也不必在发行版中包含全部的Setuptools工具。通过包含一个单独的bootstrap模块，如果用户使用源码构建包，并且没有安装相应版本的Setuptools，那么包会自动的下载和安装它。

功能特点

- 使用easy_install工具的时候，在构建期间会自动的寻找/下载/安装/升级依赖包。easy_install支持通过HTTP，FTP，Subversion和SourceForge下载包，并且会自动的浏览PYPI上链接的网页，来寻找下载链接（它是当前python中最象CPAN的工具）。
- 创建python egg文件 - 一个单文件，可导入的发行格式。
- 增强对访问寄宿于zip包中的数据文件的支持。
- 自动的包含源代码树中所有的包，而不必在setup.py中单独的列出它们。
- 自动的包含源发行版中所有相关的文件，而不必创建一个MANIFEST.in文件，- 并且在源代码树发生改变的时候，不必重新生成MANIFEST文件。
- 自动生成项目中任意数量的main函数的包装脚本或windows上的.exe文件（注意：它不是py2exe的替代，.exe文件依赖于本地的python）。


## 2包管理
### 2.1基本应用
```
from setuptools import setup, find_packages
setup(
    name="包名",
    version="0版本号",
    packages=find_packages(),
)
```
- 项目就能够生成egg，上传到PYPI，和自动地包含setup.py所在的目录中的所有的包。 
- 可以给setup脚本添加更多的信息，来帮助人们发现和了解项目。并且项目可能包含一些依赖，一些数据文件和脚本
- 可以指定特定的版本号，正常情况用不到，不过多介绍

### 2.2setup()中的关键参数
- 在清单文件”MANIFEST.in”中，列出想要在包内引入的目录路径,而不仅仅是源码
- include_package_data 
接受所有的被MANIFEST.in匹配的或者是在版本控制系统中找到的文件和目录。
package_data 
指定额外的模式来匹配不被MANIFEST.in匹配的或者是不在版本控制系统中的文件和目录。 
exclude_package_data 
指定用于安装包的时候不应该被包含的数据文件和目录的模式
例如
```
include_package_data = True,    # 包含所有文件
package_data = {'包名': ['data/*.dat']} #包含包名下data文件下.dat文件
exclude_package_data = { '': ['README.txt'] }, #排除README.txt
```
- find_packages()
会遍历目标目录，寻找Python包，并使用inclusion模式过滤。
- install_requires
指定相应的依赖包的版本，回去PyPI获取，如果没有的话，还可以用`dependency_links`指定下载路径
```
install_requires=[    # 依赖列表
        'Flask>=0.10',     
        'Flask-SQLAlchemy>=1.5,<=2.1'
    ]
dependency_links=[    # 依赖包下载路径
        'http://example.com/dependency.tar.gz'
    ]
```
- entry_points
通过entry_points可以定义一系列接口，供别的应用或者自己调用
```
entry_points={'blogtool.parsers': '.rst = some_module:SomeClass'}
    }
    #将some_module中的函数，以名字blogtool.parsers的借口共享给别的应用。
```

## 3命令
- alias   -定义常用命令的快捷键
`setup.py alias aliasname expansion`
例如
`setup.py alias --global-config daily egg_info --tag-build=development`
这样设置以后 daily代表  egg_info --tag-build=development，
如`setup.py daily bdist_egg`

- bdist_egg   -创建python egg文件
egg文件是跨平台的，包含的元数据有脚本和包依赖关系，一般情况下这个命令不需要添加特殊的参数

- develop -在开发模式下部署项目
部署项目源以在一个或多个“暂存区域”中使用，以便导入。 部署完成后，项目源的更改立即在暂存区域中可用，无需在每次更改后运行生成或安装步骤。
提供了一些选项，注意的是这些选项也会影响依赖的包。另外除了上面的选项，develop命令还接受easy_install接受的所有相同选项。
如果在setup.cfg中配置了任何easy_install设置，则develop命令将使用它们作为默认值，除非[develop命令行中重写它们。
--uninstall, -u取消部署
--multi-version, -m多版本，指定这个选项后，不会导入特定的包的版本
--install-dir=DIR, -d DIR 指定安装目录
- easy_install  查找并安装包
配置选项在 setup.cfg 文件中

- egg_info - 创建egg元数据并设置构建标记
创建元数据文件执行2个操作，一是更新项目的.egg-info的元数据，2是用来扩展其他元数据文件。
还可以发布时添加标记，如日期：`--tag-date, -d`

- install 可以指定用easy_install方式还是旧的方式安装，默认是easy_install

- rotate - 删除过期的分发文件
自动清理旧版本

- saveopts将使用的选项保存到配置文件里
- setopt 在配置文件中将一些选项简化
- test功能，必须包含测试模块，可以在set-up函数里用test-suite=NAME, -s NAME命令指定特定的要运行的测试，如果在setup（）调用中没有设置test_suite，并且不提供--test-suite选项，则会出现错误。

### 不同模式的区别
- devlop开发模式下使用，无需在每次更改后运行生成或安装步骤，可以edit/debug/develop操作；实际上没有安装任何东西。相反，它在部署目录中创建一个特殊的.egg-link文件，该文件链接到项目的源代码。而且，如果你的部署目录是Python的site-packages目录，它还会更新easy-install.pth文件以包含项目的源代码，从而使它可以在sys.path中使用Python安装的所有程序。
- install 生产环境下使用，安装后无法修改，如果修改，需要重新安装；会下载所有的安装包。
