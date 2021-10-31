## 配置
有三个配置文件，存储配置.[参考](https://git-scm.com/book/zh/v2/%E8%B5%B7%E6%AD%A5-%E5%88%9D%E6%AC%A1%E8%BF%90%E8%A1%8C-Git-%E5%89%8D%E7%9A%84%E9%85%8D%E7%BD%AE)
- `/etc/gitconfig`或者windows下`${git安装位置}/etc/gitconfig` 系统上每一个用户及他们仓库的通用配置。可通过`--system`指定
- `~/.gitconfig`或windows下`${C:/Users/$USER}/.gitconfig` 只针对当前用户。会对你系统上 所有 的仓库生效。可通过`--global`指定
- `.git/config`，仓库根目录。针对该仓库。可通过`--local`指定
## 状态
- 四种状态
  - untracked
  - unmodified
  - modified
  - staged
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
## git别名
  例子
  - `git config --global alias.co checkout` 设置co为checkout的别名，`git co`来代表`git checkout`.
  - `git config --global alias.last 'log -1 HEAD'`  可以用`git last`代表`git log -1 HEAD`.
  - 命令前面加入 ! 符号，可以执行外部命令。例如，`git config --global alias.visual '!gitk'`,`git visual`代表`gitk`.