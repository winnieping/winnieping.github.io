---
layout: post
title: 'Webstorm上使用Git'
---
webstorm使用git比控制台更方便，可视化操作。
<!--break-->
# Webstorm上使用Git
## 1.右键项目->git的选项卡   


<img src="/../assets/git-01.png" alt="note">   

Commit File    提交至本地版本库   
Add				添加进缓存区   
Show Current Revision 	显示当前文件的最新版本信息   
Compare with the Same Repository Version 与当前版本（的文件）做比较，可以理解为与最新版本比较，也就是可以比较本地工作区和本地版本库的差异（也记住，比较视图中，右侧永远是最新的那个版本内容）   

Show History 		展示关联本文件（或者文件夹中的所有文件）提交信息历史，我们可以看到，历史提交信息面板出现于下方Version Control面板中   

   
## 2.Respository的选项卡.  <br/>   
<img src="/../assets/git-02.png" alt="note">     
Branches 显示左右分支，包括本地分支和远程分支，进而对各个分支能够进行更多的操作   
checkout 检出至本地工作区，此时本地已经检出过   
checkout as new local branch 检出至本地工作区，并创建新分支   
compare  两个分支进行比较   
merge  进行合并操作   
delete  删除当前分支   

## 3.快捷键
ctrl+t 更新

ctrl+k 本地提交

ctrl+shift+k 远程提交
