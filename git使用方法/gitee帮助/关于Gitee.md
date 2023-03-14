关于Gitee
-------------
>Gitee 是[开源中国社区](http://www.oschina.net)团队基于开源项目 [GitLab](http://github.com/gitlabhq/gitlabhq) 开发的在线代码托管平台。

>每一个Gitee 账号可创建 1000 个项目，不限公有或私有项目，并已宣布[代码托管服务永久免费](http://www.oschina.net/news/41842/git-osc-no-limitation)。

Gitee 的功能
-------------
>Gitee 除了提供最基础的 Git 代码托管之外，还提供代码在线查看、历史版本查看、Fork、Pull Request、打包下载任意版本、Issue、Wiki 等方便管理、开发、协作、共享的功能，具体请查看[帮助](https://gitee.com/oschina/git-osc/wikis/帮助)。

意见和建议
-------------
>在您使用 Gitee 的过程中有任何意见和建议，请到此项目提交Issue，我们的开发者会在这里跟您沟通。

帮助
-------------
>在使用 Gitee 的过程中有任何不明白的问题，可以查阅[码云帮助文档](http://oschina-git.mydoc.io/)查看帮助说明，也欢迎您到[开源中国社区](http://www.oschina.net)上提问，也可以加入我们开源中国官方Git交流群：515965326。

Git 版本控制入门
------------------------
>如果你不熟悉Git，点此查看权威Git书籍 [ProGit（中文版）](https://gitee.com/progit)，新手老鸟均适合。

>Git参考手册：http://gitref.org/zh/index.html

>Git官网：http://git-scm.com

>Git客户端下载地址： [官方Git](http://git-scm.com/downloads) － [TortoiseGit](http://tortoisegit.org/download/)  － [SourceTree](https://www.sourcetreeapp.com/) 

>Git 手机APP下载地址：[点击这里](http://gitee.com/appclient)

>Git手册：http://git-scm.com/docs

>网友整理的码云教程，请[查看这里](http://gitee.com/oschina/git-osc/wikis/%E5%B8%AE%E5%8A%A9#继续阅读)。

>一份很好的 Git 入门教程，[点击这里查看](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001373962845513aefd77a99f4145f0a2c7a7ca057e7570000)。

>Git快速入门（gif动画版），[点击这里查看](http://gitee.com/wzw/git-quick-start)

码云 快速使用
------------------------
以下步骤以 [oschina/git-osc](http://gitee.com/oschina/git-osc) 仓库为例子，在您使用 Gitee 的过程中，具体链接地址请填写对应的仓库地址。
- 快速设置:

  如果您知道该怎么操作，直接使用下面的地址:
    ```
    https://gitee.com/oschina/git-osc.git  
    git@gitee.com:oschina/git-osc.git  
    ```
我们强烈建议所有的 Git 仓库都有一个`README`,`LICENSE`,`.gitignore`文件。  
Git入门？查看 [帮助](http://gitee.com/oschina/git-osc/wikis/%E5%B8%AE%E5%8A%A9) , [Visual Studio](http://my.oschina.net/gal/blog/141442) / [TortoiseGit](http://my.oschina.net/longxuu/blog/141699) / [Eclipse](http://my.oschina.net/songxinqiang/blog/192567) / [Xcode](http://my.oschina.net/zxs/blog/142544) 下如何连接本站, [如何导入项目](http://www.oschina.net/question/82993_133520)  
- 简易的命令行入门教程：
    1. Git 全局设置：

        ```
        # 用户名和邮箱需要填写您在 码云 对应的用户信息
        git config --global user.name "username"
        git config --global user.email "user email"
        ```
    2. 在 码云 新建一个仓库，我们以 [oschina/git-osc](http://gitee.com/oschina/git-osc)为例
    3. 在本地创建 Git 仓库：

        ```
        # git remote add 应添加您对应的仓库地址，可为 HTTPS 或 SSH
        mkdir git-osc  
        cd git-osc  
        git init  
        touch README.md  
        git add  README.md  
        git commit -m 'first commit'
        git remote add origin https://gitee.com/oschina/git-osc.git  
        git push -u origin master
        ```
    4. 如果您在本地已经有需要上传到 码云 的项目，那么您需要执行如下命令：

        ```
        cd existing_git_repo
        git remote add origin https://gitee.com/oschina/git-osc.git
        git push -u origin master
        ```