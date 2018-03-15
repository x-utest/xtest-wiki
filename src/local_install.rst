=================
局域网系统安装
=================

基本环境
============

- Ubuntu 14.04/16.04
- Python 3.5+
- Git
- MongoDB 3.0.1-3.2.7
- NodeJS 8.9.3+

前端
===========

工具准备
>>>>>>>

下载 Nodejs, 自行在 http://nodejs.cn/ 下载最新版
可以在命令行测试是否安装成功，输入

.. code::

 node -v

测试npm是否安装成功，输入

.. code::

 npm -v

如果都显示版本号数字，则没有异常，已安装成功 nodejs 及包管理工具

代码准备
>>>>>>>

下载 x-test 前端项目代码：

.. code::

 git clone https://github.com/x-utest/xtest-web.git

依赖安装
>>>>>>>

首先安装网页打包工具webpack及vue等依赖项
**在项目根目录下使用命令行输入**
由于 npm 在国外服务器下载的原因，如果网络比较理想，可以直接输入

.. code::

  npm install 

如果网络较差或者等待后卡住不动，可以直接使用淘宝镜像服务器
安装 npm 的国内工具 cnpm, 输入

.. code::

 npm install -g cnpm --registry=https://registry.npm.taobao.org 

cnpm 可以替代npm使用，来安装包和依赖

.. code::

 cnpm -v

如果显示目录和版本号，则 cnpm 安装成功

.. code::

 cnpm install

等待下载完成，如果出现安装失败的异常，**请尝试使用管理员权限执行**

安装
>>>>>>>>>

本地需要开启服务接口，进入目录执行命令行  
先设置对应的服务接口地址

.. code::

 node init

根据提示输入服务器 IP 地址和 xtest 服务端口号（8009），改好后开始打包项目,命令行输入

.. code::

 npm run build

执行完成 会在根目录下生成 **/dist** 的静态资源文件夹， 放在Web服务器上
本地也可以使用 Nodejs 开启 Web 服务进行测试，默认 8896 端口，输入

.. code::

 npm run start


本地可访问 http://localhost:8896 或者 http://IP:8896

服务端
===============

MongoDB 配置
>>>>>>>>>>>

确认已安装好 MongoDB, 安装过程可参考 http://blog.csdn.net/nxyx520/article/details/79564288

**注意：MongoDB 版本需要为 3.0.1 - 3.2.7，其他版本不支持。**

.. code::

 mongo --version
*MongoDB shell version: 3.0.1*

登录 MongoDB

.. code::

 mongo

添加 admin 数据库的用户名密码

.. code::

 use admin

 db.createUser({
     user:"admin",
     pwd:"admin",
     roles:[{
     role:"userAdminAnyDatabase",
     db:"admin"
     }]
     })

 db.auth("admin", "admin")

添加 xtest 数据库的用户名密码

.. code::

 use xtest

 db.createUser({
     user:"xtest",
     pwd:"xtest@2018",
     roles:[{role:"readWrite", db:"xtest"}]
     })

 db.auth("xtest", "xtest@2018")

代码准备
>>>>>>>>>>>

下载 x-test 服务端代码基本包，版本 0.0.1

.. code::

 git clone https://github.com/x-utest/xtest-server-base.git

下载 x-test 服务端代码，版本 3.17.5.29.1

.. code::

 git clone https://github.com/x-utest/xtest-server.git

安装依赖
>>>>>>>>>>>

安装 x-test 服务端代码基本包

.. code::

 cd xt-server-base

 sudo ./install

使用 pip 安装部分开源库

.. code::

 cd xt-server-api

 pip install -r requirement.txt

下载并安装 dtlib 库，版本 new

.. code::

 git clone https://github.com/our-dev/dtlib.git
 cd dtlib
 ./install.sh

Nginx 安装配置
>>>>>>>>>>>

使用 apt 安装 nginx（测试版本 openresty/1.9.7.4）

.. code::

 apt-get install nginx

软链接 xt-server-api/nginx_config 目录中的配置文件到 /etc/nginx/conf.d/ 目录下，并重启 nginx 服务使之生效

.. code::

 cd /etc/nginx/conf.d/

 ln -s <YOUR_BASE_PATH>/xt-server-api/nginx_config/* .

 service nginx restart

其中 <YOUR_BASE_PATH> 为 xt-server-api 所在的目录。

重启 nginx 服务后，检查 8099, 8009 两个端口是否处于监听状态

.. code::

 netstat -ntlp | grep 80
 tcp        0      0 0.0.0.0:8099            0.0.0.0:*               LISTEN      29871/nginx
 tcp        0      0 0.0.0.0:8009            0.0.0.0:*               LISTEN      29871/nginx

启动 x-test 服务程序
>>>>>>>>>>>

最后一步，执行如下命令启动 x-test 服务端程序：

.. code::

 python start.py

开始使用吧
===========

浏览器打开 http://IP:8099 ，点击下一步即可初始化系统数据库，并获得一个管理员账号密码。使用该账号密码即可登录 X-Test 测试系统。