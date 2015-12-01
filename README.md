# git问题记录--如何从从detached HEAD状态解救出来

git问题记录--如何从从detached HEAD状态解救出来

今天使用git的时候在终端发现这样一条信息  
`HEAD detached at head
`
##**分析**
心里一惊,艾玛这是什么状态?
其实我们知道,`git checkout`本质上是修改HEAD里面的内容来让它指向不同分支的,而HEAD文件指向的分支就是我们当前的分支,但是有时候HEAD不会指向任何分支,严谨的说是HEAD指向了一个没有分支名字的修订版本,此时恭喜你,已经处于游离状态了(detached HEAD).这时候我们在进行commit操作不会提交到任何分支上去.  

这个时候输入`git status`查看当前状态发现我没有在任何本地分支上也验证了刚才的猜想,而这时候我又作死的进行了commit操作,git提示我
```
Warning: you are leaving 1 commit behind, not connected to
any of your branches:

  fef4501 interrationRecord page completed

If you want to keep them by creating a new branch, this may be a good time
to do so with:

 git branch <new-branch-name> fef4501
```  
然后我欢天喜地的`git checkout ask_11_16`切换到工作分支以为万事大吉,艾玛坑爹啊,我特么辛辛苦苦写了一天的代码呢?不过这时候我们回看上面那段提示,智能的git告诉我如果我还想要这次提交的话,可以创建一个新的分支,同时把本次提交的id告诉了我:`fef4501`.
那么这时候我已经有了一个思路:
1. 基于本次提交创建一个临时分支.
2. 然后merge到我当前工作分支.
3. 删除临时分支  

##**实操**
####基于本次提交创建临时分支
输入
`$git branch temp fef4501  `
使用git branch 分支名 操作ID 这句命令能够创建一个新的分支,但要注意此时我们还没有切换到这个分支上,这个分支上面代码跟我刚才提交完之后的一样.
####切换到工作分支并合并代码
输入
`$git checkout ask_11_16`
意味着我已经切换到ask_11_16分支,这个分支是我之前想要提交的分支.
然后
`$git merge temp`
这行命令过后我们已经上次commit合并到ask_11_16上了,此时终端状态为
`Your branch is ahead of 'origin/ask_11_16' by 1 commit.
`
我们只需要`$git push`即可把本次提交push到远程分支.
这时候检查代码,perfect!正式我们想要的状态.
####删除temp分支
大功告成,至于temp分支已经没有了利用价值,本着过河拆桥的精神我不得不输入
`$git branch -d temp`
来删除temp分支. 

*git是一个很优秀的版本控制工具,利用得当能让我们在团队协作时候如鱼得水,但是万一有操作失误,也会让很多不熟悉git命令的人各种发愁,下面贴一个git命令大全,非常实用*





