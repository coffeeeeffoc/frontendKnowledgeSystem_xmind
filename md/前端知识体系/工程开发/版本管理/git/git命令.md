[toc]
## 注意
  github 默认的分支是main，git默认的分支是master
## 配置
- 切换账户：
  `git config --global user.name coffeeeeffoc`
  `git config --global user.email 1521152077@qq.com`
# 远程仓库
- 查看远程仓库
  `git remote` 
  `git remote -v` -v表示会显示需要读写远程仓库使用的 Git 保存的简写与其对应的 URL
- 添加远程仓库d
  **本地可以指定多个远程仓库，分别提交到多个仓库，这点很有用**
  `git remote add <shotname> <url>`
- 抓取仓库
  `git fetch <remote>`  需要手动合并
  `git pull`  会自动合并分支内容
- 推送到仓库
  `git push <remote> <branch>`
- 远程仓库的重命名
  `git remote rename <old> <new>`
- 远程仓库的删除
  `git remote remove <name>`
  `git remote rm <name>`
- 查看某个远程仓库
  <!-- 可以列出以下信息：你在特定的分支上执行 git push 会自动地推送到哪一个远程分支。 它也同样地列出了哪些远程分支不在你的本地，哪些远程分支已经从服务器上移除了， 还有当你执行 git pull 时哪些本地分支可以与它跟踪的远程分支自动合并。 -->
  `git remote show <name>`

## 其他
- 提交
  `git commit -m 'commit-msg'`

  <!-- 与不带a的区别是，省略了[已经过git add file添加过但没提交commit，然后又修改file的内容]时的[需要再次执行git add file]的命令。原因是git add命令是个多功能命令，根据目标文件的状态不同，此命令的效果也不同：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等 -->
  `git commit -m 'commit-msg'`

- 推送 
  `git push -u origin master`

- 查看分支历史
  `git log --oneline --decorate --graph --all`

- 重命名
  `git mv <old_filename> <new_filename>`
- 创建分支
  `git branch <branch_name>`
- 切换分支
  `git checkout <branch_name>`