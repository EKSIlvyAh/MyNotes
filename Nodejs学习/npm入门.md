# npm的使用

**参考链接**

`npm换源`
- **https://blog.csdn.net/Fhyoiii/article/details/106729645**

- **http://t.zoukankan.com/treasury-p-12793253.html**


## npm换源

### 使用阿里定制的 cnpm 命令行工具代替默认的 npm

```
npm install -g cnpm --registry=https://registry.npm.taobao.org
```

### 换淘宝源

```
npm config set registry https://registry.npm.taobao.org
```

### 使用rnm换源工具

**安装**

```
npm install -g nrm --registry=https://registry.npm.taobao.org
```

**查看可用源**

带 * 号的表示当前源

```
rnm ls
```

**切换源**

nrm use `源名称`，例如

```
nrm use taobao
```

**删除源**

```
nrm del
```

**测试速度**

```
nrm test
```

## npm查看配置

```
npm config list
```

## 设置全局包存储/缓存路径

前提得先创建 `node_global` `node_cache` 文件夹

```
npm config set prefix "Nodejs安装路径/node_global"
# 设置全局包存储路径

npm config set cache "Nodejs安装路径/node_cache"
# 设置缓存路径存储路径
```

