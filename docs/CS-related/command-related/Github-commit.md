---
title: Github回滚代码——利用commit进行回溯
date: 2021-08-22_Sun
status: 发芽期
tags: 
- Git
- Github
source: https://blog.csdn.net/qq_28093585/article/details/101465845
aliases: [撤回远程github误操作, git push失败解决方法]
---

*[[Hugo自动部署]]与[[如何利用obsidian高效建立笔记]]中都提到了git与github的相关操作，初学git相关指令并解除github，在很多概念搞不清时很容易出现操作失误。*

回退命令：

```
git reset --hard HEAD^  # 回退到上个版本
git reset --hard HEAD~3  # 回退到前3次提交之前，以此类推，回退到n次提交之前
git reset --hard commit_id  # 退到/进到指定commit
```

之后强制推到远程:
`git push origin HEAD --force`

拓展阅读：
[git 回溯commit](https://www.jianshu.com/p/7ac00fdc136f)