
# WSL2安装Ubuntu-22.04

*在写这个时我已经安装过一次Ubuntu-20.04就不重复之前的内容了*
其他准备内容可以在`win11安装wls2.txt`文件中查看

> 获取Ubuntu22.04分发包

1.打开微软商店下载Ubuntu-22.04

2.安装完成之后，打开运行以完成注册

3.确保关闭 `Ubuntu-22.04`
```
wsl --shutdown
# 关闭

wsl -l -v
# 查看是否已经停止运行
```

4.创建文件夹 `E:/WSLUbuntu`

5.迁移`Ubuntu-22.04`到其他盘
```
wsl --export Ubuntu-22.04 D:/WSLUbuntu/Ubuntu-22.04.tar
# 导出到指定文件夹

wsl --unregister Ubuntu-22.04  
#注销wsl

wsl -l -v  
# 检查是注销成功

wsl --import D:\WSLUbuntu D:\WSLUbuntu\Ubuntu-22.04.tar --version 2
# wsl --import <安装路径> <tar包的路径> --version 2 (代表wsl2)
```

6.修改默认用户

打开wsl ubuntu之后，会默认以root身份登录。所以这里修改回来
```
Ubuntu2202 config --default-user 原来的用户名
```
## 至安装完毕!
