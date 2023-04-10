# MongoDB安装及配置

* [参考b站教程](https://www.bilibili.com/video/BV1BS4y1w7Wy/)

* [参考腾讯云博客教程](https://cloud.tencent.com/developer/article/2013742)

## Windows安装

### 下载MongoDB

**下载地址** 
* https://www.mongodb.com/try/download/community

* https://fastdl.mongodb.org/windows/mongodb-windows-x86_64-6.0.4-signed.msi（直链）

**注意事项**

- MongoDB偶数版本为稳定版（推荐），奇数版本为开发版

- MongoDB对32位操作系统支持不佳，而且32位的开发包已停更

**下载包选择**

MongoDB Community Server 6.0.4 Windows .msi

*.msi代表图形化引导式安装*

**安装时配置**

- 选择自定义安装到其他盘

- 在最后一步选择安装Mongodb Compass（图形化管理工具，装好之后再下载也行，自动安装可能比较慢）


剩下的就直接下一步

### 安装后配置

- 把所安装mongodb目录内的 ./bin 目录 添加到环境变量即可

- 在 ./data 目录内新建db目录

然后终端输入 **mongob/mongos** 来测试是否安装成功

[参考链接](https://www.runoob.com/mongodb/mongodb-window-install.html)

#### 下载MongoDB Shell

MongoDB Shell可以用来连接服务器

MongoDB Shell需要额外安装

下载地址 https://www.mongodb.com/try/download/shell

下载完后是一个zip包，把它放在 Mongo的安装根目录解压

然后找到其中的 bin 目录，添加到环境变量即可

----

## linux-Ubuntu安装
