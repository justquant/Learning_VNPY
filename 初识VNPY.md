# 初体验

我学习任何东西，都喜欢先看看他是如何工作的。因此我第一步下载了VN.Trader，试着把它跑起来

## 下载和安装

在官网[下载链接](http://www.vnpy.org/pages/download.html)里下载了最新的版本v1.6.2.这是一个zip包。

解压到一个目录，阅读README. 按照README的指导，需要准备好一些环境设施。

要求AnnaConda必须是2.7 32bit版本的。而我的机器是win10操作系统，现在安装的版本是3.6 64bit版本的AnnaConda。

不管怎样，先跑跑看。运行install.bat: 

报了一堆语法错误，原因是3.5版本和2.7版本的语法是不兼容的。想起conda是可以创建虚拟环境的。关于Conda创建虚拟环境的[中文翻译文档](http://www.jianshu.com/p/d2e15200ee9b)

    conda create --name vnpy python=2.7.3

```
(C:\Anaconda3) c:\vnpy-1.6.2>conda create --name vnpy python=2.7.3
Fetching package metadata .....
Solving package specifications: .

Package plan for installation in environment C:\Anaconda3\envs\vnpy:

The following NEW packages will be INSTALLED:

    pip:        9.0.1-py27_1  https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
    python:     2.7.3-7       https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
    setuptools: 27.2.0-py27_1 https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free
    wheel:      0.29.0-py27_0 https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free

Proceed ([y]/n)? y

python-2.7.3-7 100% |###############################| Time: 0:02:04 166.38 kB/s
python-2.7.3-7 100% |###############################| Time: 0:01:00 343.03 kB/s
setuptools-27. 100% |###############################| Time: 0:00:02 381.45 kB/s
wheel-0.29.0-p 100% |###############################| Time: 0:00:00 550.92 kB/s
pip-9.0.1-py27 100% |###############################| Time: 0:00:03 428.85 kB/s
#
# To activate this environment, use:
# > activate vnpy
#
# To deactivate this environment, use:
# > deactivate vnpy
#
# * for power-users using bash, you must source
#
```

激活vnpy的虚拟环境:

    activate vnpy

运行install.bat，安装成功了。（注：我之前安装过rqalpha，因此对于.net相关的安装包已经安装好)

## 注册仿真账号

我（现在）没有钱，从仿真账号玩起。按照推荐，我去[SimNow](http://www.simnow.com.cn/)注册了一个账号。

注册流程很方便，而且有100w的启动模拟金！注册后要重新登录才能看到看到自己的账号信息。

将README.md中的示例代码存成一个run.py，然后运行：

```python
(vnpy) D:\>python  run.py

(vnpy) D:\>python run.py
Traceback (most recent call last):
  File "run.py", line 9, in <module>
    from vnpy.event import EventEngine
ImportError: No module named vnpy.event
```

在python 2.7的REPL里，运行:

```
>>> import vnpy.event
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
  File "vnpy\event\__init__.py", line 3, in <module>
    from .eventEngine import EventEngine, EventEngine2, Event
  File "vnpy\event\eventEngine.py", line 10, in <module>
    from qtpy.QtCore import QTimer
ImportError: No module named qtpy.QtCore
```

qtpy.QtCore不应该在安装脚本一起装吗？在知乎上找到[这篇文章](https://www.zhihu.com/question/51627423),说是因为版本不兼容。我目前的python环境没有pyqt，然后就默认使用主conda环境的pyqt包。

尝试给vnpy虚拟环境安装pyqt包:

    conda install pyqtgraph=0.9.10

强制将环境设置为32bit：

    set CONDA_FORCE_32BIT=1

切换回64 bit的环境：

    set CONDA_FORCE_32BIT=


折腾了好久，依然没有搞定。我决定换成python 2.7版本的Anaconda了。

下载了[Anaconda2-4.0.0-Windows-x86.exe](https://repo.continuum.io/archive/Anaconda2-4.0.0-Windows-x86.exe),并安装。幸运的是可以在同一台机器上同时安装不同的anaconda版本。

安装完成后，继续创建一个虚拟环境vnpy，运行install.bat，依然报出找不到pyQT的错误。

然后我尝试着不使用虚拟环境，安装成功了！！

总结其中的血泪史：在现阶段，要安装vnpy，你必须使用python 2.7 32位的4.0.0的conda版本而且不能使用conda虚拟环境。

但是从软件功能的角度，我有一个问题：能不能不使用vnpy的GUI，而只是用它的交易接口？

这个问题我后面继续探索。

vnpy是如何连接mongodb的呢？这点README里没有详细的介绍。后来才发现需要把mongodb注册成系统的服务，然后vnpy会自己连接。我很奇怪，如果没有用户名和密码，如何连接的？

且待后面继续探索。

与VNPY的初次见面，不太愉快，折腾！

## 其它

1. 使用conda的清华镜像，会快很多。

    conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
    conda config --set show_channel_urls yes

2. 注意由于QT的关系，建议只使用anaconda 4.0.0版本。在官方没有升级到python 3之前，我们只能用旧一点的版本了。