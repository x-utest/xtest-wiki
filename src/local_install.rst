=================
局域网系统安装
=================

基本环境
============

- Linux
- Python 3.5


代码准备
===========

下载Nodejs，自行在http://nodejs.cn/ 下载最新版
可以在Shell测试是否安装成功，输入

.. code::

 node -v

测试npm是否安装成功，输入

.. code::

 npm -v

如果都显示版本号数字，则没有异常，已安装成功nodejs及包管理工具


依赖安装
============
首先安装网页打包工具webpack及vue等依赖项
**在项目根目录下使用Shell输入**
由于npm在国外服务器下载的原因，如果网络比较理想，可以直接输入
.. code::

  npm install 

如果网络较差或者等待后卡住不动，可以直接使用淘宝镜像服务器
安装npm的国内工具 cnpm，输入

.. code::

 npm install -g cnpm --registry=https://registry.npm.taobao.org 

cnpm 可以替代npm使用，来安装包和依赖

.. code::

 cnpm -v

如果显示目录和版本号，则cnpm安装成功
.. code::

 cnpm install

等待下载完成，如果出现安装失败的异常，**请尝试使用管理员权限执行**

 

web端安装
===============

开始打包项目,输入

.. code::

 npm run build

执行完成 会在根目录下生成 **/dist** 的静态资源文件夹， 放在Web服务器上
本地可以开启8896端口进行测试，输入

.. code::

 npm run start

本地可访问 http://localhost:8896 或者 局域网ip加端口号