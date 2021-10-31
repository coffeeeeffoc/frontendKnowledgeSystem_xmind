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
## 文件状态
- `git add`
  这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。 将这个命令理解为“精确地将内容添加到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。 
- `git status -s` `git status --short`  简介的方式输出status状态
- [.gitignore](https://github.com/github/gitignore)
  - 规范格式
    - 所有空行或者以 # 开头的行都会被 Git 忽略。
    - 可以使用标准的 `glob 模式`匹配，它会递归地应用在整个工作区中。
    - 匹配模式可以以（/）开头防止递归。
    - 匹配模式可以以（/）结尾指定目录。
    - 要忽略指定模式以外的文件或目录，可以在模式前加上叹号（!）取反。
  - glob 模式是指 shell 所使用的简化了的正则表达式。 星号（*）匹配零个或多个任意字符；[abc] 匹配任何一个列在方括号中的字符 （这个例子要么匹配一个 a，要么匹配一个 b，要么匹配一个 c）； 问号（?）只匹配一个任意字符；如果在方括号中使用短划线分隔两个字符， 表示所有在这两个字符范围内的都可以匹配（比如 [0-9] 表示匹配所有 0 到 9 的数字）。 使用两个星号（\*\*）表示匹配任意中间目录，比如 a/**/z 可以匹配 a/z 、 a/b/z 或 a/b/c/z 等
- `git diff`  当前文件和暂存区快照之间的差异
- `git diff --staged`  `git diff --cached`  已暂存文件与最后一次提交的文件差异当前文件个暂存区快照之间的差异
<!-- 删除如果要删除之前修改过或已经放到暂存区的文件，则必须使用强制删除选项 -f（译注：即 force 的首字母） -->
- `git rm <file>`  把指定文件 从工作目录中删除，且从staged暂存区删除
- `git rm <glob>`  把满足正则表达式的文件 从工作目录中删除，且从staged暂存区删除  例如：`git rm log/\*.log`和`git rm \*~`
- `git rm --cached <file>`  从staged暂存区删除，但不从工作目录中删除
- `git mv file_from file_to`  文件改名
## 显示日志[git log ](https://git-scm.com/book/zh/v2/Git-%E5%9F%BA%E7%A1%80-%E6%9F%A5%E7%9C%8B%E6%8F%90%E4%BA%A4%E5%8E%86%E5%8F%B2)
- `-p`  `--patch` 会显示每次提交所引入的差异（按 补丁 的格式输出）
- `-<n>`  显示前n个记录，n可以为任意整数
- `--stat`  在每次提交的下面列出所有被修改过的文件、有多少文件被修改了以及被修改过的文件的哪些行被移除或是添加了。 在每次提交的最后还有一个总结
- `--pretty`  使用不同于默认格式的方式展示提交历史
  - `--pretty=oneline`  每个提交放在一行显示
  - `--pretty=format:"%h - %an, %ar : %s"`  定制记录的显示格式
    - %H  提交的完整哈希值
    - %h  提交的简写哈希值
    - %T  树的完整哈希值
    - %t  树的简写哈希值
    - %P  父提交的完整哈希值
    - %p  父提交的简写哈希值
    - %an 作者名字
    - %ae 作者的电子邮件地址
    - %ad 作者修订日期（可以用 --date=选项 来定制格式）
    - %ar 作者修订日期，按多久以前的方式显示
    - %cn 提交者的名字
    - %ce 提交者的电子邮件地址
    - %cd 提交日期
    - %cr 提交日期（距今多长时间）
    - %s  提交说明
- `--graph` 在日志旁以 ASCII 图形显示分支与合并历史。
- `--oneline` --pretty=oneline --abbrev-commit 合用的简写。
- 显示git log输出的选项
  - `-<n>`  仅显示最近的 n 条提交。
  - `--since=<date>`, `--after=<date>`  仅显示指定时间之后的提交。
  - `--until=<date>`, `--before=<date>` 仅显示指定时间之前的提交。
  - `--author=<pattern>`  仅显示作者匹配指定字符串的提交。
  - `--committer=<pattern>` 仅显示提交者匹配指定字符串的提交。
  - `--grep=<pattern>`,`--grep <pattern>`   仅显示提交说明中包含指定字符串的提交。
  - `-S<string>`  仅显示添加或删除内容匹配指定字符串的提交。例如：`git log -S function_name`和`git log -S"frotz"`
- `git log --pretty="%h - %s" --author='Junio C Hamano' --since="2008-10-01" --before="2008-11-01" --no-merges -- t/` 一个例子
## 撤销操作
- `git commit --amend`  提交完了才发现漏掉了几个文件没有添加，或者提交信息写错了。 此时，可以运行带有 --amend 选项的提交命令来重新提交上次提交后。效果上等同于旧有的提交从未存在过一样。
- `git reset HEAD <file>...`  取消暂存的文件.
- `git checkout -- <file>...` 撤消对文件的修改.Git 会用最近提交的版本覆盖掉它
## 标签
  - 列出标签
    - `git tag` 显示tag列表，等价于`git tag -l`和`git tag --list`
    - `git tag -l <pattern>`  按照指定模式显示tag，此时-l是必须有的。例子：`git tag -l "v1.8.5*"`
  - 创建标签
    - `git tag -a <tag_name>` 附注标签（annotated）
    - `git tag -a <tag_name> -m"mesage"` 附注标签需要指定一条存储在标签中的信息
    - `git tag <tag_name>`  轻量标签（lightweight）
    - `git show <tag_name>` 查看某个标签
  - 后期打标签
    - `git tag -a <tag_name> <check_sum>` 指定之前的某次提交作为tag标签，例子：`git tag -a v1.2 9fceb02`
  - 共享标签，推送标签到远程仓库
    - `git push origin <tagname>` 推送单个标签到远程仓库
    - `git push origin --tags` 推送多个标签到远程仓库，包括所有的附注标签和所有的轻量标签
  - 删除标签
    - `git tag -d <tagname>`  要删除掉你本地仓库上的标签
    - 移除远程仓库的标签
      - `git push <remote> :refs/tags/<tagname>`  冒号前的空值，推送到远程标签名，从而高效删除
      - `git push origin --delete <tagname>`  只管的删除远程标签
  - 检出标签checkout
    - ` git checkout -b <new-branch>` 创建新分支并切换过去
    - ` git checkout -b <new-branch> <remote>/< branch>` 以某个远程分支为副本，创建新分支并切换过去
    - ` git checkout <branch>` 切换到某个分支
## 分支
  - `git merge <tag|branch>`  例如下图，若当前在master分支，则`git merge topic`表示将topic分支的内容合并到master分支。若有冲突，则以公共父节点E为基准，把master和topic的改动都对比显示冲突，需手动解决
    ```
          A---B---C topic
          /
      D---E---F---G master
    ```
  - `git branch`  不加参数，相当于列表显示当前所有分支
  - `git branch -v`  查看每个分支的最后一次提交
  - `git branch --merged` 查看列表中已经合并到当前分支的分支
  - `git branch --unmerged` 查看列表中尚未合并到当前分支的分支
  - `git branch -b <branch>` 删除某个分支
## 远程分支
  - 推送
    - `git push <remote> <local_branch>:<remote_branch>`  表示推送本地分支local_branch到远程分支remote_branch。当本地分支和远程分支同名时，可以简写为一个分支名。例子：`git push origin serverfix:awesomebranch`和`git push origin serverfix`
  - 跟踪分支
    - 几种追踪方式
      - 克隆仓库时自动创建跟踪`origin/master`的`master`分支
      - `git checkout -b <branch> <remote>/<branch>`  从远程仓库拉取，创建新分支，并切换到该新分支
      - `git checkout --track <remote>/<branch>` 例子：`git checkout --track origin/serverfix`
      - `git branch --track <local_branch> <branch>` 例子：`git branch --track devel origin/mywork`
      - `git branch -u <upstream> [<local_branch>]` 例子：`git branch -u origin/serverfix`
      - `git branch --set-upstream-to=<upstream> [<local_branch>]` 例子：`git branch --set-upstream-to=origin/serverfix`
    - 查看所有跟踪分支
      - `git branch -vv`  查看所有跟踪分支
    - 
  - 拉取
    - `git fetch` 只从服务器抓取数据，但不合并merge
    - 例子：`git fetch origin` + `git merge origin/serverfix`将远程仓库拉取到本地，并合并serverfix到当前分支
    - `git pull`  相当于`git fetch` + `git merge`
  - 删除远程分支
    - `git push <remote> --delete <branch>`
## [变基](https://git-scm.com/book/zh/v2/Git-%E5%88%86%E6%94%AF-%E5%8F%98%E5%9F%BA)
  - `git rebase <target_branch> <source_branch>`  把一个源分支的改动（相对于两个分支的公共父节点），整合到另一个目标分支中。达到的效果就是，提交历史中不是显示两个分支，而是只有一个分支
  - `git rebase --onto <target_branch> <source_compare_branch> <source_branch>` 把源分支相对于源比较分支的改动，映射整合到目标分支中。达到的效果就是，source_branch与source_compare_branch原有的公共节点将不再是公共节点，犹如基于target_branch新产生一个分支
