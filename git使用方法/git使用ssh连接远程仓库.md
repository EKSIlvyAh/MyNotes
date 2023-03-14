# git使用ssh连接远程仓库

## 生成公钥
```
ssh-keygen
```
然后会让你输入密钥保存路径 *默认在当前用户的工作目录下新建.ssh文件夹*

密钥口令(passphrase)可不填

然后会生成2个文件

* id_rsa
* id_rsa.pub


## 找到公钥

到保存路径找到.pub文件复制里面的公钥

## 进入github

在 settings > SSH and GPG kys > New SSH Key

把公钥复制过去即可

## 测试
ssh git@github.com

## 打开git-bash

然后就可以用 **git clone git@github:用户名/仓库名** 的形式clone项目了

