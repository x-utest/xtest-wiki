=================
局域网系统安装
=================

使用 Docker 部署（推荐）
--------------------

基本环境
========

- 装有 Docker 环境的宿主机

构建镜像
==================

代码准备
>>>>>>>>>>

下载 xtest Docker 项目源码

.. code::

 git clone https://github.com/x-utest/xtest-docker.git

修改配置文件
>>>>>>>>>>

修改 config.json 文件首行的 IP 为你宿主机的 IP；
例如你的 Docker 宿主机 IP 为 192.168.1.200 ，则将

.. code::
 var apiHost = 'http://192.168.3.158:8009/';

修改为

.. code::
 var apiHost = 'http://192.168.1.200:8009/';

可以选择修改端口号为你期望的端口号，但是建议保持不动；

**除非你有特殊要求, 保持其他文件不动, 否则你可能需要 xtest-server 和 Dockerfile 文件中的部分代码。**

构建镜像
>>>>>>>>>>

冒号后为版本号，不需要可以不写

.. code::

 docker build -t xtest .

构建过程需要耐心等待一段时间。

配置
===========

在构建镜像步骤完成后，需要进行一些配置。

创建卷
>>>>>>>>>>

在宿主机上创建 docker 卷，用于 MongoDB 数据持久化

.. code::

 docker volume create xtest-data

初始化 MongoDB
>>>>>>>>>>>>>>>>

xtest 系统需要创建用户，因此在上述步骤完成后，第一次运行 xtest 容器需要先进行初始化 MongoDB。执行如下命令：

.. code::

 docker run -v xtest-data/data/db -it xtest ./init_mongo.sh

完成初始化 MongoDB 后，容器会自动关闭，整个 xtest 系统的环境也已配置完成，可以运行使用啦！

运行
============

.. code::
 docker run -p 8009:8009 -p 8099:8099 -v xtest-data/data/db -it xtest

该命令中，8099 端口为浏览器访问端口，若修改该端口，则步骤7也需要修改.例如你希望通过 HOST_IP:1234 访问 xtest 网页，则将本步骤中 -p 8099:8099 修改为 -p 1234:8099；

8009 端口用于 xtest 前后端通信，若你在步骤2中修改了该端口，则本步骤也需要做相应修改。例如你在步骤2中将端口修改为2345，则需要将本步骤中 -p 8009:8009 修改为 -p 2345:8009

开始使用吧！
============

浏览器输入 HOST_IP:8099，即可访问 xtest 系统，欢迎使用！

直接安装方式部署
-----------------

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