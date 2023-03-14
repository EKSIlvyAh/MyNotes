# Node.js的安装

`参考链接`：**https://blog.csdn.net/zxy15974062965/article/details/121117803**

`参考链接`：**https://blog.csdn.net/qq_43557395/article/details/124325563**

## Windows安装

### 下载Node.js

**下载链接**

- `英文官网`：**https://nodejs.org/en/**

- `中文官网`：**https://nodejs.org/zh-cn/download/**

**安装包选择**

- `英文官网`：18.3.0 LTS(长期维护的稳定版本)

- `中文官网`：Windows安装包

### 安装时配置

下载完之后是一个.msi文件，根据引导安装即可

**在某一处它会提示安装时需要C/C++编译和python环境**，这里如果本机没有安装相应工具的话，可以选择勾选自动安装

### 安装后配置

安装完一般会自动添加环境变量

如果没有的话，完成下面的步骤

**找到安装路径**（我本机的安装路径为 E:\Development\Nodejs）

把该路径添加到环境变量即可

### 验证安装是否成功

打开终端输入
```
node -v  # 查看版本

npm -h  # 查看node package manager的帮助
```

**如果输出没问题的话说明安装完成**

----

## linux-Ubuntu安装


