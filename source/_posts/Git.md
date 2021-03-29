---
title: Git
tags:
  - git
abbrlink: 25246
date: 2019-05-13 16:00:00
---

打 tag 
    
    git tag v2.0.88
    git push origin v2.0.88
    
创建分支
    
    git branch gravity_test
查看分支

    git branch

切换分支

    git checkout gravity_test
    
提交代码

    1.git add .(git add -A)
    
    2.git commit -m ""
    
    3.git push origin gravity_test
    
git commit 编写提交信息

1.`Insert`+信息

2.`Esc` : wq `Enter`
    
    
暂存

    #储藏
    git stash
    git stash list
    #释放最后一次stash的数据并将其从list中移除
    git stash pop
    #将你指定版本号为stash@{1}的工作取出来
    git stash pop stash@{1} 