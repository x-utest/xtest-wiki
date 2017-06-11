
===================
新手入门
===================


前言
=========

在正式开始本文的长篇大论前，有必要综述一下本文的内容。

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


软件开发工作流
===================

大家都试着构想过这样一种工作流：

1. 小组成员一起开会设计好系统
2. 定义好接口并形成书面文档
3. 开发人员负责 **软件功能项目** 和测试人员负责 **测试用例项目**，同步开发
4. 长时间的小步快速开发和迭代，功能和用例数都不断增长
5. 功能项目的目标是让所有的测试用例都通过
6. 最终达成团队的预期目标，项目上线


工程流程图如下：

.. image:: ./images/xtest-work-flow.png


以上工作流的好处如下：

1. 开发和测试同步进行，人员得到最大化利用
2. 让小步和快速迭代上线成为可能，极大降低了项目无谓的消耗和项目失败的风险
3. 每个过程都严格的让测试用例对软件质量进行了有效把控
4. 测试用例的总数和成功总数对软件这个黑盒形成了良好的 **量化**
5. 整个开发过程和进度对于 **不懂技术** 的团队成员也是 **完全透明** 的


整个过程用两句话可以描述：

1. 测试 **驱动** 开发
2. 测试 **量化** 开发


当然，一个IT企业，做到以上是非常不容易的，但是可以朝向这个方向不断前进。

本文先介绍一个系统来助力IT企业实现这一目标。在实现这一目标之前，先达成一个小目标：

让企业重视软件质量，提升软件质量控制的 **技术和方法论** ，让软件测试活动变得 **流行** 起来，让软件测试成为整体软件生产活动中的 **高频事件** 。

为了上述的小目标落地，那么先要到如下几点：

1. 让软件测试工作确实起到 **量化** 开发过程的作用
2. 加速测试重要产出物 ***测试报告** 的曝光度和传播度
   
借助 **xtest系统** 可以有效实现这个目标：

1. 测试开发人员完成自动化测试项目的编写
2. 提取测试项目的报告
3. 按照 **xtest系统** 组织成 **规范的报告格式**
4. 上传测试报告到 **xtest系统**
5. 根据时间线形成完整的 **产品成长量化图**
6. 形成直观的 **测试报告分享报表**，直接分享给协作人员查看

具体效果如后文所示。

xtest系统演示
====================

形成稳定的产品测试线路图：

.. image:: ./images/xtest-list-share.png

对测试记录生成 **分享链接** ，供其他非登录用户直接进行访问：

.. image:: ./images/xtest-share-gen.png


`示例链接地址 <http://xtest.apiapp.cc/utest-report-share.html?stoken=b40a587c362a11e7856f00163e006b26>`_

.. code::

    http://xtest.apiapp.cc/utest-report-share.html?stoken=b40a587c362a11e7856f00163e006b26

被分享和传播的精美测试报告：

.. image:: ./images/xtest-share-report.png

可以任意邮件或者QQ微信进行分享和传播，极大的提高了报告的 **可读性和传播性**。


系统试用 `地址 <http://xtest.apiapp.cc/login.html>`__ ：

.. code::

    http://xtest.apiapp.cc/login.html

测试的概念陈述
==================

目前软件测试行业的理论和细分领域层出不穷，主要包括以下几方面：

1. 判定测试
2. 压力测试
3. 响应测试
4. 安全测试

本文只讲 **判定测试** 这个领域的内容。

所谓的 **判定测试** 包括如下几类：

1. 界面自动化
2. 接口自动化
3. 单元自动化
   
其实这就是 **分层自动化金字塔** 中的三层。

.. image:: ./images/auto-test-by-layer.png

它们的共同点就是：对于一个定义数据和定义的执行过程通常会有一个预期输出，如果实际输出和预期输出不同，则定义为Flase，否则为True。

所以，基于以上特点，都可以归结到 “判定测试” 的概念里面来，可以生成一样的 测试报告产出物，都适用于本文提到的 **xtest系统** 。

此处的 **判定测试** 的测试场景和类别包括且不限于：

1. 单元测试
2. 接口测试
3. UI测试
4. 环境测试


只要是涉及到：

1. 测试用例
2. 测试套件
3. 测试结果
4. 测试详情
   
都可以使用本系统生成报表并存储历史测试数据。


良好的开发规范
=====================


为了达到上述的预期效果，有一些基本的开发规范需要地团队共同遵守。

1. 良好分层的软件系统架构
2. 明确的版本管理体系
3. 形成良好信息流的基础软件功能

分层架构
-----------

前面提到了 **软件的分层金字塔** 结构，是目前主流互联网公司的所推崇的基本结构。
很多无法推进自动化测试的软件系统，大部分问题都出在这儿了，需要好好设计一下。


版本管理
----------------------

所谓的版本管理，其实目标就是：明确被测对象。

一个软件系统从开始到结束它的名字基本上是不会变的，但是每个时期，只要是任何代码的改变，其实它都是变化的。对外展现的是不变性，对内则一定要进行好的区分。

被测对象版本号的重要性。每次提测而且需要记录备案的软件系统必须是独一无二的，拥有唯一的代号，即： **版本号**。

关于版本号的命名方式，业界有很我成熟的方法，在此不再赘述。只要满足如下要求即可：

1. 保证版本号的唯一性
2. 从字面意思可以看得到版本的演进和迭代顺序

基础功能
-------------------

形成流畅的信息流的前提是：所有的过程尽量能够自动化。

对于测试人员来说，有两样东西非常重要：

1. 被测对象版本号及特性信息
2. 运行相关环境及软件库依赖

以 **服务端接口** 作为被测试对象的 **自动化测试** 过程为例子，开发人员必须至少提供一个接口：

- 显示应用程序内部信息的接口。

对于该接口的定义，下面给出一个示范：


- 接口名称
    /app-info/
- 请求方式
    GET
- 输入参数
    无

返回值：

.. code::

    {   
        "code": 200, 
        "msg": "", 
        "data": {
            "server": "tornado", 
            "req_time": "2017-04-19 15:38:46", 
            "app_version": "3.17.04.18.1"
        }
    }

参数说明：

- app_version 服务端接口应用程序版本号
- server 服务器类型

如果有其它需要关注的信息，可以随时扩展上去。

本文最关注的内容是 **app_version 所表示的被测对象版本信息** ，在上面的接口中有所体现。


xtest系统接口
=======================

域名路径：

.. code::

    http://api.apiapp.cc/

这些系统接口可以直接封闭到sdk当中，目前提供了python版本的sdk的demo供大家参考。

认证接口
-----------

功能：生成token以授权接口的调用

路径：

.. code::

    /testdata/api-auth/


传入参数：

- app_id
- app_key
  
请求方法：POST

返回值：

.. code::

    {
        "code": 200,
        "msg": "",
        "data": {
            "user": null,
            "u_name": null,
            "user_id": null,
            "token": "7c45fc98391311e78e1a00163e006b26",
            "ip": "113.57.119.51",
            "user_agent": null,
            "rc_time": "2017-05-15 10:09:11",
            "c_type": 4,
            "app_id": "3832f354872411e6a7c700163e006b26",
            "last_use_time": "2017-05-15 07:22:54",
            "finger_prt": null,
            "c_name": "api",
            "id": "59190dc747fc890ec5ba42e0",
            "is_del": false,
            "cookie": null,
            "del_time": "2017-05-15 10:09:11"
        }
    }

其中最重要的是：

- token  后续进行接口调用的授权值

数据接口
----------------

xtest系统提供了报告上行接口。

接口路径:


.. code::

    /testdata/create-test-data/



token认证: 需要，token放url里面

请求方式: POST

请求参数:


.. code::

    {   
        "pro_id": "57a835c8c6e905166da94243",
        "pro_version":"1.3.4.5",
        "run_time": 51.77724599838257,

        "was_successful": false,
        "total": 88,
        "skipped": 7,
        "errors": 0,
        "failures": 10,
        
        "details": [
            {
                "status": "failures",
                "note": "AssertionError: 访问不合法,返回404",
                "explain": "只是用于测试的Demo,没有太多意义",
                "test_case": "test_nginx_config"
            },
            {},
            {}
        ]
    }


以上的请求参数分为两部分：

1. 单元测试框架标准结果部分
2. 自动化测试项目后期添加
   
其中：

- pro_id 项目在xtest系统中的id代号
- pro_version 被测对象的唯一版本号
- run_time 运行所有脚本花费的时间
  
属于自动化测试项目后期运算出来的数据，其它的则是标准的单元测试框架提供的测试结果中自带内容。


上传数据成功后的返回值:

.. code::

    {"code":200,"msg":"success","data":""}

提取测试结果
=====================

本小节以 pyunit 单元测试框架为例子，来对测试报告所需要的内容进行提取。

主要内容
----------------------

`TextTestResult` 结果中包含的如下内容：

- errors  错误详细信息列表
- failures  运行失败详细信息列表
- skipped 跳过的详细信息列表
- testsRun 运行的用例总数

具体如下图所示：


.. image:: ./images/xtest-xunit-result.png

具体条目
----------------

以 `failures` 为例子：

.. image:: ./images/xtest-xunit-details.png

本测试用例的描述：

编号 **索引为0** 的数据:

- _testMethodName  测试函数名称
- _testMethodDoc  测试函数的文档，这里面一般陈述本测试的功能

打印出来的堆栈错误信息：

- 编号  **索引为1** 的数据。

提取方法
----------------

对测试结果进行内容提取，同时加入如下内容：

1. 测试执行时间
2. 项目ID
3. 项目版本号

进行标准化格式打包。

代码：

.. code::
    
    def dict_encode_test_results(test_results, **kwargs):
        """
        将测试结果进行json编码
        :param test_results:
        :type test_results:  unittest.TestResult
        :return:
        """
    
        run_time = kwargs.get('run_time', None)
        pro_id = kwargs.get('pro_id', None)
        pro_version = kwargs.get('pro_version', None)
    
        # 主体部分
        res_dict = dict(
            # was_successful=True if test_results.wasSuccessful() else False,
            was_successful=test_results.wasSuccessful(),
            total=test_results.testsRun,
            failures=len(test_results.failures),
            errors=len(test_results.errors),
            skipped=len(test_results.skipped),
            run_time=run_time,
            pro_id=pro_id,
            pro_version=pro_version
        )
    
        # 详细信息部分
        failure_list = []  # 失败的内容
        for x in test_results.failures:
            note_data = {
                'test_case': x[0]._testMethodName,
                'explain': x[0]._testMethodDoc.rstrip('\n        :return:'),
                'status': 'failures',
                'note': x[1]
            }
    
            failure_list.append(note_data)
    
        for i in test_results.errors:
            note_data = {
                'test_case': i[0]._testMethodName,
                'explain': i[0]._testMethodDoc.rstrip('\n        :return:'),
                'status': 'errors',
                'note': i[1]
            }
            failure_list.append(note_data)
    
        res_dict['details'] = failure_list
    
        return res_dict


可以提到一个如下的字典对象：

.. code::

    test_res_dict = {
        "pro_version": "1.16.10.10.1",
        "pro_id": "57fa12ec47fc894ee04a2c69",  # 在后台管理系统中组织信息详细信息里面可以查看到:项目ID
        "run_time": 51.772,

        "was_successful": False,
        "skipped": 2,
        "errors": 1,
        "failures": 1,
        "total": 5,
        
        "details": [
            {
                "status": "failures",
                "note": "AssertionError: 访问不合法,返回404",
                "explain": "只是用于测试的Demo,没有太多意义",
                "test_case": "test_nginx_config"
            }
        ]
    }




对接xtest
==================

主要步骤如下：

1. 使用微信在首页扫码进行注册或者登录
2. 【资产管理】-【项目信息】查看到 **项目编号**
3. 查看 **app_id** 和 **app_key**

依据xtest系统提供的API进行接口调用，可将  **判定测试** 的结果上传到 **测试报告系统服务器** 数据库，即可生成 **软件系统量化线路图** 和 **精美测试报表服务** 。





定位和展望
==============

系统定位：

1. 本系统定位为 **自动化** 判定测试报告系统
2. 没有代码执行系统，需要自己去写测试代码本地执行
3. 可以将执行系统放在Jenkins里面去自动构建触发你的执行代码，然后将测试结果显示到本系统中
4. Jenkins只能显示构建的历史，然后本系统可以显示测试的历史，刚好可以成为一个补充

后续展望：

1. 提供看板报表功能，将量化过程投影到公司电视墙上
2. 自动化SAAS服务器再触发相应的webhook，回调到后续的系统（例如：自动化发布系统）
3. 对接Jenkins等持续集成


关于xtest系统，目前还有些功能在完善中，完善后再对外开放，相当于测试界人员的福利吧。换句话来说：

目前本系统还不对外开放，我们公司自己先用好了再说 ^_^

项目过程量化图
=========================

.. image:: ./images/xtest-project-cycle.png

主要特点：

1. 以测试用例的数目和失败数量来量化开发过程
2. 最终失败用例数是趋向于0的
3. 随着功能的增加，总的用例数原则上是一直在增长的
4. 以用例成功率来衡量每个版本的发布节点