---
title: 安装Gensim的攻坚战
date: 2017-06-14 15:40:25
tags: Python
categories: Python
---

## Python小白安装Gensim的报错&&解决之路

官网教程：[http://radimrehurek.com/gensim/install.html](http://radimrehurek.com/gensim/install.html)
发现不能直接使用pip install --upgrade gensim，因为报错了：
 numpy.distutils.system_info.NotFoundError: no lapack/blas resources found
同时，看见官网上解释说Gensim依赖于：

+ Python >= 2.6. Tested with versions 2.6, 2.7, 3.3, 3.4 and 3.5. Support for Python 2.5 was discontinued starting gensim 0.10.0; if you mustuse Python 2.5, install gensim 0.9.1.
  
+ NumPy >= 1.3. Tested with version 1.9.0, 1.7.1, 1.7.0, 1.6.2, 1.6.1rc2, 1.5.0rc1, 1.4.0, 1.3.0, 1.3.0rc2.
  
+ SciPy >= 0.7. Tested with version 0.14.0, 0.12.0, 0.11.0, 0.10.1, 0.9.0, 0.8.0, 0.8.0b1, 0.7.1, 0.7.0.
如上的几个包。

其中的Python肯定安装了，NumPy可以使用pip install NumPy成功安装，剩下SciPy没有安装。于是尝试使用pip install命令安装，但是报错了：**error: no lapack/blas resources found**

根据这个错误，通过百度Scipy的windows安装官方文档：[https://www.scipy.org/scipylib/building/windows.html](https://www.scipy.org/scipylib/building/windows.html)，这是在Windows系统上安装的步骤。发现介绍说这是一个很常见的错误，因此心里得到些许安慰，跟着教程走...而然从安装Fortran compiler开始一脸懵逼。所以开始寻找别的解决方式。
发现在官方文档[https://www.scipy.org/install.html#individual-packages](https://www.scipy.org/install.html#individual-packages)这个教程最后提到了直接可以下载windows上编译好的二进制文件，于是在[http://www.lfd.uci.edu/~gohlke/pythonlibs/](http://www.lfd.uci.edu/~gohlke/pythonlibs/)这个网站上，下载了scipy的二进制包。因为我的Python版本是3.5，所以下载了scipy-0.19.0-cp35-cp35m-win_amd64.whl文件。进入cmd命令行界面，首先下载可以安装whl文件的工具：pip install wheel。然后就可以使用pip install  .whl绝对路径,这样的命令安装了scipy

通过上述步骤之后，确实可以通过pip install --upgrade gensim安装gensim了，但是在Python命令行界面中使用from gensim import corpora, models, similarities命令时，出现了如下的报错：

**from numpy._distributor_init import NUMPY_MKL  # requires numpy+mkl
ImportError: cannot import name 'NUMPY_MKL'**

所以，产生这个错误的原因是我的命令依赖于numpy+mkl(numpy with Intel Math Kernel Library),所以其实还需要一个numpy+mkl的Python包，依旧通过[http://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy](http://www.lfd.uci.edu/~gohlke/pythonlibs/#numpy)这个网址，找到对应Python版本的numpy+mkl文件，下载之后，使用pip install命令安装。
解决~~~


