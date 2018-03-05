=================
局域网系统安装
=================

基本环境
============

- Linux
- Python 3.5
- Git
- MongoDB

前端
===========

代码准备
>>>>>>>

下载Nodejs，自行在http://nodejs.cn/ 下载最新版
可以在命令行测试是否安装成功，输入

.. code::

 node -v

测试npm是否安装成功，输入

.. code::

 npm -v

如果都显示版本号数字，则没有异常，已安装成功nodejs及包管理工具


依赖安装
>>>>>>>

首先安装网页打包工具webpack及vue等依赖项
**在项目根目录下使用命令行输入**
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

安装
>>>>>>>>>

本地需要开启服务接口，进入目录执行命令行  
先设置对应的服务接口地址

.. code::

 node init

根据提示输入地址，改好后开始打包项目,命令行输入

.. code::

 npm run build

执行完成 会在根目录下生成 **/dist** 的静态资源文件夹， 放在Web服务器上
本地也可以使用Nodejs开启Web服务进行测试，默认8896端口，输入

.. code::

 npm run start


本地可访问 http://localhost:8896 或者 ip加端口号

服务端
===============

MongoDB 配置
>>>>>>>>>>>

确认已安装好 MongoDB

.. code::

 mongo --version
*MongoDB shell version: 3.0.1*

编辑配置文件 /etc/mongo.conf, 修改端口为 27027，并开放局域网网络访问权限

.. code::

 # network interfaces
 net:
   port: 27027
   bindIp: 0.0.0.0

重启 MongoDB 服务

.. code::

 service mongod restart

登录 MongoDB

.. code::

 mongo --port 27027

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
     pwd:"xtest@2017",
     roles:[{role:"readWrite", db:"xtest"}]
     })

 db.auth("xtest", "xtest@2017")

代码准备
>>>>>>>>>>>

下载 x-test 服务端代码，版本 3.17.5.29.1

.. code::

 git clone https://gitee.com/x-utest/xt-server-api.git

安装依赖
>>>>>>>>>>>

使用 pip 安装部分开源库

.. code::

 cd xt-server-api

 pip install -r requirement.txt

下载并安装 dtlib 库，版本 new

.. code::

 git clone https://gitee.com/our-dev/dtlib.git
 cd dtlib
 ./install.sh

Nginx 安装配置
>>>>>>>>>>>

使用 apt 安装 nginx，版本 openresty/1.9.7.4

.. code::

 apt-get install nginx

复制 test-api.conf 和 test.conf 到 /etc/nginx/conf.d/ 目录下后，重启 nginx 服务

.. code::

 service nginx restart


检查 8099, 8009 两个端口是否处于监听状态

.. code::

 netstat -ntlp | grep 80
 tcp        0      0 0.0.0.0:8099            0.0.0.0:*               LISTEN      29871/nginx
 tcp        0      0 0.0.0.0:8009            0.0.0.0:*               LISTEN      29871/nginx

*至此，整个 xtest 系统的安装配置已经完成，接下来登录页面即可*

最后
===========

浏览器打开 http://IP:8099 ，点击下一步即可初始化系统数据库，并获得一个管理员账号密码。