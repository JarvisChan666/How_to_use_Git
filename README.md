# How to use GIT?(Chinese Ver)

## git "Merge" and "Rebase"

```
git checkout feature
git merge main
```

但这里的最大问题是merge会把main里的commit信息全部并入进来，这些大部分都和feature毫无相关，当开发feature的人想去查看log的时候，会发现有一堆无相关的在main上的工程师的commit，加重查找信息难度

因此就有了rebase
如果我们使用rebase来更新feature(main有了一些更新，我们想保持一下更新)，也是和merge差不多
优势：
● 消除了不必要的，与feature无关的commit信息，减少很多无关噪音
● 线性的，没有出现分支，能直接查到。不像上面的merge，当项目大起来之后会有一堆forks和merge超难看。
当然这个的优点就是缺点，尽管merge commit信息很多，但有些还是很宝贵，例子如下
M点的commit信息清楚展示时间线，以及在C点的时候把main信息更新到feature
如果把下面的DE rebase，那么就看不到M的信息了
   A---B---C (main)
    \     \
     \     M---N (feature with merge commit)
      \
       D---E (feature before merge)


 A---B---C (main)
           \
            D'---E' (feature after rebase)       

时刻遵循： Golden Rule of Rebasing: Never rebase a branch that is public (shared with others) unless you are certain that nobody else is working with those branches or you've communicated with your team and they understand the implications.， 尽量自己一个人用

git checkout feature
git rebase main
A---B---C (main)
 \
  D---E (feature)

A---B---C (main)
         \
          D'---E' (feature)

有关automated tebase 和 interactive rebase 自动和交互式
interactive 的rebase方式能够在分享给他人的时候清除commit history
5点能做的
1. Squash Commits: Combine multiple commits into a single commit.
2. Reword Commit Messages: Change commit messages without altering the content.
3. Edit Commits: Amend a commit to include new changes, correct errors, or break it into smaller commits.
4. Reorder Commits: Alter the order of commits.
5. Drop Commits: Remove unwanted commits from the history.
为什么有这个东西
● 在merging或者提交之前，把本地的commits信息打磨一下，把我们的feature branch打磨干净后才上传到共享库
怎么用
For example, if the 2nd commit fixes a small problem in the 1st commit
you can condense them into a single commit with the fixup command:

pick 33d5b7a Message for commit #1
fixup 9480b3d Message for commit #2
pick 5c67e61 Message for commit #3

# Rebase 1234567..ebfd367 onto 1234567 (3 command(s))
#
# Commands:
# p, pick <commit> = use commit
# r, reword <commit> = use commit, but edit the commit message
# e, edit <commit> = use commit, but stop for amending
# s, squash <commit> = use commit, but meld into previous commit
# f, fixup <commit> = like "squash", but discard this commit's log message
# x, exec <commit> = run command (the rest of the line) using shell
# d, drop <commit> = remove commit
By changing the word pick to another command (reword, edit, squash, fixup, drop), you can perform any of the aforementioned actions 上述行动 on the respective commits.
