================
xtest报告接口
================



系统简介
=======================

域名路径：

.. code::

    http://api.apiapp.cc/

这些系统接口可以直接封装到sdk当中，目前提供了完整的python版本的sdk的demo供大家参考。

认证接口
==============

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
============

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


