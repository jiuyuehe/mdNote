##第一步：安装

轻松玩转github：
---
1. 注册成为 github用户
2. 首先下载github windows 客户端；  
3. 安装完成后打开gitShell 窗口，这里客户端在安装的时候就已经帮你把用户名密码设置好了，并且把公钥上传到了github。无需配置直接使用即可；
> ps：使用 ```git config --global -l```  可以查看。
4. 学习一下git的常用命令吧；这里强烈推荐[廖雪峰老师的博客](http://www.liaoxuefeng.com/),我不会告诉你我就是跟着他学的。

**然如果你觉得廖老师太多了懒得看，而且只写写博客什么的就直接看下文吧，下面会给出常用的```git命令```**。

玩转sublime中的markdown
---
1. 安装sublime (这都搞不定的话那就没救了)。
2. 安装sublime markdown 插件，并且设置语法高亮（[如何安装插件](http://jianshu.io/p/378338f10263?comment=12766)）
3. 友情提示以下sublime 是一个无比优雅的文本编辑器，通吃所有代码。


##第二步：动手，将本地笔记保存到github

如果前面两步都认真完成了的话，那么你至少干了这么几件事：

* 注册了github帐号，并且创建了一个repository，例如我的：https://github.com/jiuyuehe/mdNote
* 在sublime上写一个markdown 实例，并且使用快捷键```alt+m```打开了markdown 的预览。

使用git命名将本地笔记发布到远程[命令形式]
```shell
	git clone https://github.com/jiuyuehe/mdNote [注意改成你创建的帐号]
```
把笔记copy到对应目录下面，
```shell
	git status [查看状态]
	git add . [添加所有改动文件]
	git commite -m "add a new note" [提交]
	git push -u mdNote master [推送到github]
```




