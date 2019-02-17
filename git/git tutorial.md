## git 学习笔记

学习资料：

可视化练习git：https://learngitbranching.js.org/

git 官方文档：https://git-scm.com/doc

廖雪峰git教程：https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000

### 概念

#### 1、工作区（Working Directory）、暂存区（Stage）、版本库（Repository）

![工作区&暂存区&版本库](https://raw.githubusercontent.com/opwtryl/photos/master/git/%E5%B7%A5%E4%BD%9C%E5%8C%BA%26%E6%9A%82%E5%AD%98%E5%8C%BA%26%E7%89%88%E6%9C%AC%E5%BA%93.jpg)
(图片来源：https://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/0013745374151782eb658c5a5ca454eaa451661275886c6000)

工作区即电脑上的文件夹。
版本库即工作区里面的 .git文件夹，保存项目的元数据和对象数据库。
暂存区是 版本库 .git文件夹里面的 index文件，保存了下次将提交的文件列表信息。

#### 2、文件状态

![文件状态](https://github.com/opwtryl/photos/blob/master/git/lifecycle.png?raw=true)
（图片来源：https://git-scm.com/book/en/v2/Git-Basics-Recording-Changes-to-the-Repository）

untracked 表示 文件是新建的，从未添加到暂存区（版本库里当然也没有）。相反，tracked表示版本库里面有文件的记录。

unmodified 表示 文件和版本库里面的记录相比，没有修改过。modified则相反。

staged 表示 文件已被添加到暂存区。

### 命令

#### 基础命令

##### git config

git有3个级别的配置文件,分别是：

1. --system 包含系统上每一个用户及他们仓库的通用配置，优先级最低。 WIN7文件路径："C:\ProgramData\Git\config"
2. --global 只针对当前用户。WIN7文件路径："C:\Users\administrator\.gitconfig"
3. --local 只针对当前仓库，优先级最高。WIN7文件路径：在当前仓库的 ".git\config"

上述路径可以通过 `git config --list --show-origin` 查看

添加用户信息
当安装完 Git 应该做的第一件事就是设置你的用户名称与邮件地址。 这样做很重要，因为每一个 Git 的提交都会使用这些信息，并且它会写入到你的每一次提交中，不可更改：

`$ git config --global user.name "John Doe"`
`$ git config --global user.email johndoe@example.com`

再次强调，如果使用了 --global 选项，那么该命令只需要运行一次，因为之后无论你在该系统上做任何事情， Git 都会使用那些信息。 当你想针对特定项目使用不同的用户名称与邮件地址时，可以在那个项目目录下运行没有 --global 选项的命令来配置。

##### git init

用于创建仓库，生成 .git文件夹，同时会创建 master分支，和指向 master的 HEAD文件。

#### 增删改查

##### .gitignore 文件

.gitignore文件规定了哪些文件不需要跟踪，已经跟踪的文件不会受影响。当遇到无法添加文件的时候，可以使用 git check-ignore 查看那条规则忽略了该文件，修改规则；也可以使用 git add -f 强制添加文件。

##### git add

用于添加文件到暂存区。

git add -f ：强制添加被忽略文件

##### git commit

用于把git add 或 git rm 到暂存区的记录提交到版本库。
git commit -m ：添加提交信息，并提交到版本库
git commit -a : 可以把跟踪过的文件的 修改和删除 记录提交到版本库。相当于 git add /git rm  ，git commit的结合。但是新建的文件没跟踪过，所以不会提交到版本库。
git commit -amend：用新的提交替换上一次commit，而不是新增一个commit。

##### git rm

用于删除文件，git rm 之后 git commit 把删除记录提交到版本库。可以通过 git reset 恢复到之前的版本，但如果 git rm的文件是未跟踪过的，因为版本库没有记录，则无法恢复。

##### git mv

移动或重命名文件，git mv 之后需要 git commit 到版本库。

##### git reset

用于回退到指定版本。

##### git status

用于查询文件的状态。如上文的，已跟踪，未跟踪；已修改，未修改；已暂存，未暂存。

##### git diff

用于对比两个版本的文件的差异。

#### 分支

##### git log

查询 现存的 commit 记录。可以设置别名 `git config --global alias.lg "log --color --graph --pretty=format:'%Cred%h%Creset -%C(yellow)%d%Creset %s %Cgreen(%cr) %C(bold blue)<%an>%Creset' --abbrev-commit"`

##### git reflog

查询所有操作记录，包括已删除的记录。

##### git tag

为某个 commit 创建标签。

##### git branch

创建分支。

##### git checkout

切换分支。

git checkout -b yourbranch:相当于 git branch yourbranch 创建分支后，再 git checkout yourbranch 切换到新建的分支。

git checkout -- yourfilename: 让这个文件回到最近一次git commit或git add时的状态

git checkout \<commit> ：分离的HEAD，HEAD由指向分支（如 master），变成指向 commit。

git checkout master^: master^ 表示 master的第一个父节点，master^^ 表示第一个父节点的第一个父节点，master^2 表示 master 的第二个父节点，注意区别。

git checkout master~：master\~  表示master的第一个父节点，master~2 相当于 master^^ ,master^^^ 相当于 master~3，以此类推。 ~ 和 ^ 可以链式操作，如 master^2~2 表示 master的第二个父节点的第一个父节点的第一个父节点。

##### git merge

合并分支。

##### git stash

用于修改代码途中,需要临时做其他工作，如修改bug，但又不想提交当前修改，git stash 可以保存当前状态。完成其他临时任务，再恢复保存的状态。

##### git rebase

用于合并commit。

git rebase -i : 进入交互式界面调整 commit。
git cherry-pick ：调整commit顺序

#### 远程仓库

![远程仓库](https://github.com/opwtryl/photos/blob/master/git/remote%20repository.jpg?raw=true)
(图片来源：http://www.ruanyifeng.com/blog/2014/06/git_remote.html)

##### git remote

用于管理远程仓库。

##### git clone

下载远程仓库到本地。

##### git push

推送本地更新到远程仓库。

##### git fetch

下载远程仓库的更新到本地，但不合并。

##### git pull

下载远程仓库的更新到本地，并和本地仓库合并。相当于 git fetch 和 git merge 的结合。

##### git revert

> git revert is used to record some new commits to reverse the effect of some earlier commits (often only a faulty one).

git revert : 为了撤销更改并分享给别人, 提交新的commit，这些新的commit是旧的commit的逆操作。