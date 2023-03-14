很高兴的告诉大家，现在 **码云** 目前支持使用 Subversion 对仓库进行操作了。      


## 使用前注意
1. 仓库体积超过 300 MB 不建议使用 Subversion 操作仓库，存储库容量达到 400 MB，或者 300 MB 并且存储大量非文本数据时，我们将关闭仓库的 Subversion 支持。  
2. 由于 GIT 不支持空目录的提交，在存储机器上，无论是普通仓库还是开启 Subversion 接入的仓库存储时都是 GIT 仓库，Subversion 的 commit 是提交到 git 仓库上的，所以码云的 Subversion 不支持空目录的提交。  
3. 第一次开启 Subversion， 操作一个仓库，如果仓库体积较大或者提交次数较多，由于缓存的缘故，响应时间会比较长。  
4. 不支持 Subversion 的 Hook 机制，请使用 WebHook 替代。
5. Subversion 属性不完全支持。  
6. 客户端需要开启 SASL 支持，不支持的客户端无法访问。  
7. 部分 svn 命令不支持。可以查看 **Subversion 客户端的兼容性**   
8. 版本号的映射，目前 Subversion 的版本号计算依据为本分支所有的commit 数目减一 不包括 merge ，如果使用了在 git 中强制回退等操作，请重新检出。

*WARNING:*   
>由于 git 在设计上就没有考虑空文件 [Kernel.org: Git FAQ](https://git.wiki.kernel.org/index.php/GitFaq#Can_I_add_empty_directories.3F)

>我们设计的原则就是不破坏，不主动修改用户的仓库，我们的后端存储的完全是一个 git 仓库，如果我们添加了，一次提交内容也不会一致了，建议你在添加目录的时候添加 .keep 之类的占位文件，空文件即可。

## 关于改版 
Subversion 功能的最终解释权归 OSChina.NET 所有。Subversion 接入的规则可能在下一次改版中发生改变。

## 开启方式
在项目的设置界面开启        

![undefined](http://gitee.com/uploads/images/2015/0324/182035_d23abb99_62561.png)


如果是空仓库：     
![emptyrepo](https://static.oschina.net/uploads/img/201506/16185355_IYvK.png "开启空仓库的支持")


## 使用指南    
码云 支持的是 svn 协议。  对于 svn 而言，获取一个仓库的代码通常是 checkout，在项目主页我们通常可以获得 URL：    
![svn-url](http://static.oschina.net/uploads/space/2015/0318/152207_DRMJ_139664.png)   
这个仓库地址为：  

``` 
svn://git.net/svnserver/newos
```  

### 1.获取仓库代码：   
 
```sh
svn checkout svn://git.net/svnserver/newos newos
```

使用上述命令，我们将得到项目默认分支的代码。并将本地的工作目录命名为 *newos*   
如果最后不带 newos，svn 默认把本地工作目录命名为 SVN-URL 的最后一个目录名，这里是 *newos*    
 
```
svn checkout svn://git.net/svnserver/newos
```

如果要获得任意分支代码，请输入近似如下的命令：
     
```sh
svn checkout svn://git.net/svnserver/newos/branches/update newosupdate
```

特别的说明，获取主干分支，也就是 master 分支可以使用下面的分支格式   
 
```sh
svn checkout svn://git.net/svnserver/newos/trunk newos
```

svn trunk 分支对应 master 分支 用户应当尽量不使用下面格式     
```sh
svn checkout svn://git.net/svnserver/newos 
```

 
## 操作说明     

如果部分检出仓库，并且仓库根目录下包含 branches/tags/trunk 这样的目录，请使用完整的路径 layout，如下：

```
svn://git.net/example/example/trunk/tags/hello
svn://git.net/example/example/branches/dev/trunk
svn://git.net/example/example/branches/dev/branches 
```

如果没有 master 分支，也就没有 trunk 分支，检出的 URL 不能省略分支名。比如只有一个 dev 分支,必须使用下列格式，否则会提示仓库不存在。 

```
svn co svn://git.net/svnserver/newos/branches/dev  svnserver_dev  
```

打开终端,输入上述命令，出现以下下面提示。其中第一个认证领域是用户的密码，这个可以留空。而用户名是用户在 码云 登陆时使用`邮箱地址`。密码则是用户登陆 码云 所使用的密码
一般而言，svn 会加密缓存用户的用户名密码，所以，对仓库的操作只需要第一次输入用户邮箱和密码。
清除密码缓存，用户目录下的 .subversion/auth/svn.simple 文件夹下的文件。

![image](http://static.oschina.net/uploads/space/2015/0318/153828_ptt4_139664.png)

下图则是成功的拉取了项目代码。

![image](http://static.oschina.net/uploads/space/2015/0318/154033_aySS_139664.png)

查看本地工作目录信息：  

```sh
svn info
```
![svninfo](http://static.oschina.net/uploads/img/201503/18161038_vF3E.png )
```
cd helloworld
echo "#SVN Options ReadMe.md">SVNReadMe.md
#svn add SVNReadMe.md
#svn add * --force类似于git add -A
svn add * --force
svn update .
svn commit -m "first svn commit"
```
Subversion 在提交前建议先使用 svn update 更新工作拷贝。也就相当于 git pull 后再 git push。
Subversion 的提交是在线的，如果机器已经离线，那么提交会失败，这个过程用git的方式理解就是 git commit+git push。
用户使用 svn 提交代码同样会有动态显示。


列出版本库中的目录内容:   

```
svn list svn://git.net/svnserver/newos/trunk
```

导出仓库指定分支的所有文件，不含版本控制信息： 

```
svn export svn://git.net/svnserver/newos/trunk newos
```



## 备注   
### 安装 Subversion 客户端  

在 Apache 基金会的 Subversion 官网：   
[http://subversion.apache.org](http://subversion.apache.org)
二进制下载提示页面：  
[http://subversion.apache.org/packages.html](http://subversion.apache.org/packages.html)    

#### Windows 系统：  

与资源管理起集成的 SVN 客户端：[TortoiseSVN](http://tortoisesvn.net/downloads.html),通常被叫做"海龟"，为 msi 安装包。可以使用 [ExtractMSI](http://pan.baidu.com/s/1szHIn) 解压缩。
很诡异的是，在 Apache 上并没有推荐 TortoiseSVN。
另外还有 SlikSVN，下载地址：[https://sliksvn.com/download/](https://sliksvn.com/download/)
其他的也就不一一介绍了。   

#### Linux 系统  
一般而言 Linux 系统自带的包控制软件能够安装 Subversion，如果版本低于1.8，就建议用户下载预编译的二进制或者自己动手编译 Subversion。这里不做过多说明。

#### OS X
XCode 自带的 Subversion 版本为1.7.x，太老，而 码云 只支持1.8以上的 SVN 客户端。
如果安装了 Homebrew
```sh
brew install subversion
```
或者使用WANdisco的预编译版本
[http://www.wandisco.com/subversion/download#osx](http://www.wandisco.com/subversion/download#osx)

### Subversion 客户端的兼容性
我们支持 Apache Subversion 1.8 或者更高的版本，当你安装一个 Subversion 客户端时，如果错误提示是“无法协商验证验证方式” 请确保你的客户端支持 SASL 验证，比如在 Ubuntu 上，你可以安装 libsasl2-dev 然后编译 Subversion, 这样的话客户端是支持 SASL 验证的。
 
>sudo apt-get install libsasl2-dev
                                                                            
当你使用 svnkit 或者 SubversionJavaHl 这类 IDE 集成客户端，请确保支持 SASL 验证。

### 关于 GIT 与 SVN 的转换
如果用户存在一个基于 Subversion 托管的项目，要迁移到 码云，可以使用 git-svn 将项目转变为基于 git 的仓库，然后推送到 码云，这样你依然能够使用SVN对项目进行操作。请记得先在 码云 上新建一个项目

```
 git svn clone http://myhost/repo -T trunk -b branches -t tags 
 git remote add oscgit https://gitee.com/user/repo
 git push -u oscgit --all
```

通常来说，如果本地存在 SVN 仓库，则可以：

```
git svn clone file:///tmp/svn-repo -T trunk -b branches -t tags 
git remote add oscgit https://gitee.com/user/repo
git push -u oscgit  --all
```

将项目转移到 码云 上以后，使用 svn 命令 checkout 即可对项目进行操作。


高级指南：
[http://git-scm.com/book/zh/ch8-2.html](http://git-scm.com/book/zh/ch8-2.html)

### 安装 git，git-svn
#### Windows
msysgit 官网 [http://msysgit.github.io/](http://msysgit.github.io/),版本比较低。         
Github for Windows 提供的 git 工具和 msysgit 一致。     
MSYS2 git 下载地址: [http://sourceforge.net/projects/msys2](http://sourceforge.net/projects/msys2)，然后启动终端,安装 git，目前版本为2.4.3。
```
pacman -S git
```
Cygwin git 下载地址: [http://www.cygwin.com/](http://www.cygwin.com/),然后使用包管理软件或者直接下载 git 源码编译 git。
```sh
make configure
./configure --prefix=/usr/local
make 
make install 
```
#### Linux
有包管理器的直接用包管理器安装。
如 Ubuntu
```sh
sudo apt-get install git git-svn
```
也可以手动编译。

#### Mac OSX
下载地址：[http://git-scm.com/download/mac](http://git-scm.com/download/mac)