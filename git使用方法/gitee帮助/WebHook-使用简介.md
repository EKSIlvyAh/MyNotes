### 简介：
##### Git@OSC钩子功能（callback），是帮助用户push了代码后，自动回调一个您设定的http地址。 这是一个通用的解决方案，用户可以自己根据不同的需求，来编写自己的脚本程序（比如发邮件，自动部署等）；目前，webhook支持5种触发方式，支持复选，如图：
![输入图片说明](http://git.oschina.net/uploads/images/2015/1106/154914_9a1ba484_119968.png "在这里输入图片标题")
##### 其中，Push是指您的项目被您或者该项目的开发者push了代码，Tag Push 是指您新建了tag，Issue是指您的项目创建、更改内容、关闭、重新打开issue，评论是指任何用户在您的issue下发表了评论，合并请求是指您的项目被人提交了Pull Request。
### 说明：
* 请求的方式为POST请求
* 格式为Json
* 超时为5秒（如果程序工作时间较长，建议异步操作。）

## Post数据
为了保证安全以及识别数据来源，我们会在post数据中附带您创建hooks时填写的密码，请注意，该密码是明文

#### 如何创建WebHook

关于创建，在项目页面点击管理然后点击图中红色框中部分，如图：

![输入图片说明](http://git.oschina.net/uploads/images/2015/1106/161649_23f28dc9_119968.png "在这里输入图片标题")

然后再图中所示的地方填写post数据接收的地址以及密码：

![输入图片说明](http://git.oschina.net/uploads/images/2015/1106/161807_9e8ea098_119968.png "在这里输入图片标题")

请注意，一定要保证填写的地址是可以访问的，密码也是必须的，不然无法创建成功，如果一切都正常，你将会在刷新后的本页面看到一个类似的东西

![输入图片说明](http://git.oschina.net/uploads/images/2015/1106/162913_4f1738ca_119968.png "在这里输入图片标题")

这样你就创建好了一个WebHook

### 数据格式
#### Push的Hook发送的数据类似于：
```
hook=
{
    "password": "hod20x4hnkecy63", 
    "hook_name": "push_hooks", 
    "push_data": {
        "before": "26c9fb3533d1c63e3b822d58a8831b1caf6d9cd7", 
        "after": "219aa13d45276a5cae7436ecc66053da67ec2a2d", 
        "ref": "refs/heads/master", 
        "user_id": 449815, 
        "user_name": "gitlabu6d4bu8bd5u8d26u53f72", 
        "user": {
            "id": 449815, 
            "email": "gitlabtest_2@gitlab.com", 
            "name": "gitlabu6d4bu8bd5u8d26u53f72", 
            "time": "2015-11-06T14:51:55+08:00"
        }, 
        "repository": {
            "name": "test_gitosc_20151106145021322", 
            "url": "git@git.oschina.net:gitlab_test_1/test_gitosc_20151106145021322.git", 
            "description": "test_gitosc_20151106145021322 testting", 
            "homepage": "http://git.oschina.net/gitlab_test_1/test_gitosc_20151106145021322"
        }, 
        "commits": [
            {
                "id": "219aa13d45276a5cae7436ecc66053da67ec2a2d", 
                "message": "commit_access_test", 
                "timestamp": "2015-11-06T14:50:47+08:00", 
                "url": "http://git.oschina.net/gitlab_test_1/test_gitosc_20151106145021322/commit/219aa13d45276a5cae7436ecc66053da67ec2a2d", 
                "author": {
                    "name": "gitlab_test_2", 
                    "email": "gitlabtest_2@gitlab.com", 
                    "time": "2015-11-06T14:50:47+08:00"
                }
            }
        ], 
        "total_commits_count": 1, 
        "commits_more_than_ten": false
    }
}
```
#### Tag Push的push数据类似于：
```
hook=
{
    "password": "25b3pzwtxwu8as9", 
    "hook_name": "tag_push_hooks", 
    "push_data": {
        "ref": "refs/tags/pke1de6qpjbwpz9", 
        "before": "00000000", 
        "after": "1b0355de43a8e24ef59baa4557d4617732ed1d1b"
    }
}
```
#### Issue的push数据类似于：
```
hook=
{
    "password": "671vmti2lkhmmqv", 
    "hook_name": "issue_hooks", 
    "push_data": {
        "id": 361909, 
        "title": "x2tf25k8p0nrkoj", 
        "state": "closed", 
        "assignee": null, 
        "milestone": null
    }
}
```
#### 评论的push数据类似于：
```
hook=
{
    "password": "e7i6y15rji72sof", 
    "hook_name": "note_hooks", 
    "push_data": {
        "id": 212599, 
        "note": "_u72b6u6001u66f4u6539u4e3a **u5df2u5173u95ed**_", 
        "noteable_type": "Issue", 
        "noteable_id": 361909, 
        "author": {
            "id": 449814, 
            "user_name": "gitlab_test_1", 
            "email": "gitlabtest_1@gitlab.com"
        }
    }
}
```
#### 合并请求的push数据类似于：
```
hook=
{
    "password": "feds3os4h2f8s4k", 
    "hook_name": "merge_request_hooks", 
    "push_data": {
        "id": 15339, 
        "project": {
            "project_id": 616834, 
            "project_name": "test_gitosc_20151106145021322"
        }, 
        "target_branch": "master", 
        "source_repo": {
            "project_id": 616836, 
            "project_name": "test_gitosc_20151106145021322"
        }, 
        "source_branch": "master", 
        "author": {
            "id": 449815, 
            "user_name": "gitlab_test_2", 
            "email": "gitlabtest_2@gitlab.com"
        }, 
        "title": "pull_request_test", 
        "body": "####################"
    }
}
```

#### 以上就是5种钩子触发时推送的数据范例，为便于理解和使用，以上数据全部经过json化。另，本wiki中范例数据仅供参考，请以实际收到的数据以及格式为准。
