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

 npm

如果没有异常 代表nodejs的包管理工具安装成功


依赖安装
============
首先安装网页打包工具webpack及vue等依赖项
在项目根目录下使用Shell输入

.. code::

  npm install 

由于国外服务器网络原因，等待后如果卡住不动，可尝试使用淘宝镜像服务器
输入

.. code::

 npm install -g cnpm --registry=https://registry.npm.taobao.org 

装好后输入

.. code::

 cnpm install

等待下载完成，如果出现安装失败的异常，**请使用管理员权限执行**

 

web端安装
===============

开始打包项目,输入

.. code::

 npm run build

执行完成 会在根目录下生成 **/dist**的静态资源文件夹， 放在Web服务器上
本地可以开启8896端口进行测试，输入

.. code::

 npm run start
