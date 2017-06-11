===============
新手快速使用
===============




使用步骤
========================


1. 微信扫码登录
#. 创建一个项目
#. 下载sdk-demo
#. 填写自己的配置信息
#. 运行demo
#. 上传demo的测试结果
#. 分享报告


本方提供了基于 Python的Pyunit的Demo程序，大家只需要登录xtest系统，修改配置信息即可运行使用。



客户端Demo
=====================


Demo地址：

.. code::

    https://git.oschina.net/x-utest/xtest-python-demo.git


登录xtest系统，可以查找自己项目配置查找：


点击项目的菜单：

.. image:: ./images/xtest-client-config-0.png


查看指定配置：

.. image:: ./images/xtest-client-config-1.png

在文件 **demo.py** 中替换掉原有的配置：

.. code::

    # todo 在系统中注册了,组织信息中看到这个值,替换到此处
    project_id = '590c2a0947fc894a51f9e616'
    app_id = '3832f354872411e6a7c700163e006b26'
    app_key = '38342936872411e6a7c700163e006b26'

然后运行程序(基于python3.5及以上) ：

.. code::

    python demo.py

运行结果如下：

.. code::

    (py3venv) harmo@harmo-pc:~/work/workspace/xtest-python-demo$ python demo.py
    FFF...
    ======================================================================
    FAIL: test_first_hello_world_false (__main__.MyTestDemo)
    用户不应该越权访问资源
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "demo.py", line 65, in test_first_hello_world_false
        self.assertTrue(False, msg='Hello Word是失败的')
    AssertionError: False is not true : Hello Word是失败的

    ======================================================================
    FAIL: test_first_hello_world_false2 (__main__.MyTestDemo)
    此处用户操作太多内容了
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "demo.py", line 71, in test_first_hello_world_false2
        self.assertTrue(False, msg='Hello Word是失败的')
    AssertionError: False is not true : Hello Word是失败的

    ======================================================================
    FAIL: test_first_hello_world_false3 (__main__.MyTestDemo)
    这个用户不是超级管理员
    ----------------------------------------------------------------------
    Traceback (most recent call last):
      File "demo.py", line 77, in test_first_hello_world_false3
        self.assertTrue(False, msg='Hello Word是失败的')
    AssertionError: False is not true : Hello Word是失败的

    ----------------------------------------------------------------------
    Ran 6 tests in 0.000s

    FAILED (failures=3)
    {"code":200,"msg":"success","data":""}

其中，最后的：

.. code::

    {"code":200,"msg":"success","data":""}

的 success 表明上传报告服务器成功


执行完毕后，即可在自己的 xtest系统 界面看到相应的报告，并进行分享


使用效果图
====================


todo


后文提要
==============


在正式开始系统的长篇大论前，有必要综述一下本文的内容。

1. 展示了一种 **测试量化开发** 和 **测试驱动开发** 的工作流
2. 陈述了一些关于自动化测试的基本概念
3. 强调了一些必要的良好规范的开发习惯
4. 提出了一种自动化测试报告的报文格式
5. 给出了一种测试报告内容的提取的方法（以python下的测试框架pyunit为例子）
6. 介绍了一种pyunit和自动化报告平台的数据对接方法


最终希望本文能够给测试开发从业人员一些启示，同时本文提到的系统能够让：

1. 注重软件质量
2. 有条件做自动化测试

的软件公司形成良好的开发过程，成就自己高效透明的开发信息流。