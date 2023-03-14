# Redis基本使用方法

## 设置临时密码

redis安装后默认是没有密码的

这时可以直接使用下面的命名查看密码

```
config get requirepass
```
输出结果为空字符

要想设置密码，可以用下面的命令

```
config set requirepass "密码
```

设置密码之后，就不能直接使用config get requirepass查看密码

否则会出现(error) NOAUTH Authentication required.的错误

意思是需要身份验证

要永久设置密码，需要修改配置文件

## 设置用户名
