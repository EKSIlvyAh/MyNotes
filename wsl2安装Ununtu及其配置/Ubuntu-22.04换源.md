# Ubuntu-22.04换源

## 使用华为开源镜像站
- [网址地址: https://mirrors.huaweicloud.com/home](https://mirrors.huaweicloud.com/home)

依次输入以下命令即可
```
sudo cp -a /etc/apt/sources.list /etc/apt/sources.list.bak
# 备份配置文件

sudo sed -i "s@http://.*archive.ubuntu.com@http://repo.huaweicloud.com@g" /etc/apt/sources.list
sudo sed -i "s@http://.*security.ubuntu.com@http://repo.huaweicloud.com@g" /etc/apt/sources.list
# 修改sources.list文件，将http://archive.ubuntu.com和http://security.ubuntu.com替换成http://repo.huaweicloud.com

apt-get update
# 更新索引
```
