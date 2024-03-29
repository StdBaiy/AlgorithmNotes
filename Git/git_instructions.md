# GIT INSTRUCTIONS

## status

- 查看哪些文件
- -s可以查看简短模式
  - 新添加的未跟踪文件前面有 ?? 标记
  - 新添加到暂存区中的文件前面有 A 标记
  - 修改过的文件前面有 M 标记，右边的 M 表示该文件被修改了但是还没放入暂存区，左边的 M 表示该文件被修改了并放入了暂存区
  - 需要移除的文件前面有 D 标记
  - 重命名的文件前面有 R 标记

    ```bash
    $ git status -s
    M README
    MM Rakefile
    A  lib/git.rb
    M  lib/simplegit.rb
    ?? LICENSE.txt
    ```

## add

- 这是个多功能命令：可以用它开始跟踪新文件，或者把已跟踪的文件放到暂存区，还能用于合并时把有冲突的文件标记为已解决状态等。
- 将这个命令理解为“添加内容到下一次提交中”而不是“将一个文件添加到项目中”要更加合适。

## .gitignore

- 可以在项目根目录用`touch .gitignore`创建
  - 形如1.gitignore的文件名不起作用
- 该文件可以指定git忽略某些文件，常见的[模板链接](https://github.com/github/gitignore)
- 所有空行或者以 ＃ 开头的行都会被 Git 忽略
- 可以使用标准的 glob 模式匹配
- 匹配模式可以以（/）开头防止递归
- 匹配模式可以以（/）结尾指定目录
- 要忽略指定模式以外的文件或目录，可以在模式前加上惊叹号（!）取反

## diff

- 此命令比较的是工作目录中当前文件和暂存区域快照之间的差异， 也就是修改之后还没有暂存起来的变化内容
- 会根据上次修改**时间**判断是否进行了修改
- --staged 和 --cached 是同义词

## commit

- 这种方式会启动文本编辑器以便输入本次提交的说明
  - 使用` git config --global core.editor `命令设定默认编辑器
- -m可以直接在后面键入提交说明
- -a可以跳过使用暂存区域，Git会自动把所有已经跟踪过的文件暂存起来一并提交，但是他不会自动追踪Untracked的文件
- --amend可以修正之前的提交，你可以额外add一些文件或者delete一些，然后amend，该次提交会覆盖上次提交
  - 如果自上次提交以来你还未做任何修改（例如，在上次提交后马上执行了此命令），那么快照会保持不变，而你所修改的只是提交信息

## rm

- 该指令会同时把文件从工作区和暂存区移除
- 先从已跟踪文件清单中移除，这样下次commit就不会包括该文件
- --cached可以把文件保留在工作区，仅从暂存区移除
- 如果一个暂存了的文件被修改了，则只能-f强制移除或者--cached仅从暂存区移除

## mv

- 用法：`$ git mv file_from file_to`
- 可以用于重命名
  - 如果手动重命名一个被追踪的文件，git会认为这是先delete然后写了个新的

## log

- 按提交时间列出所有的更新，最近的更新排在最上面，列出每个提交的 SHA-1 校验和、作者的名字和电子邮件地址、提交时间以及提交说明

  ```bash
  $ git log
  commit ca82a6dff817ec66f44342007202690a93763949
  Author: Scott Chacon <schacon@gee-mail.com>
  Date:   Mon Mar 17 21:52:11 2008 -0700

      changed the version number

  commit 085bb3bcb608e1e8451d4b2432f8ecbe6306e7e7
  Author: Scott Chacon <schacon@gee-mail.com>
  Date:   Sat Mar 15 16:40:33 2008 -0700

      removed unnecessary test

  commit a11bef06a3f659402fe7563abf99ad00de2209e6
  Author: Scott Chacon <schacon@gee-mail.com>
  Date:   Sat Mar 15 10:31:28 2008 -0700

      first commit
  ```

- -p可以显示每次提交的内容差异
- -(n)可以显示最近n次commit
- --stat显示每次提交的简略的统计信息
- --shortstat只显示 --stat 中最后的行数修改添加移除统计
- --name
  - -only仅在提交信息后显示已修改的文件清单
  - -status显示新增、修改、删除的文件清单
- --abbrev
  - -commit仅显示 SHA-1 的前几个字符，而非所有的 40 个字符
- --graph显示 ASCII 图形表示的分支合并历史
- --pretty使用其他格式显示历史提交信息。可用的选项包括 oneline，short，full，fuller 和 format（后跟指定格式
  - 示例

    ```bash
    $ git log --pretty=format:"%h - %an, %ar : %s"
    ca82a6d - Scott Chacon, 6 years ago : changed the version number
    085bb3b - Scott Chacon, 6 years ago : removed unnecessary test
    a11bef0 - Scott Chacon, 6 years ago : first commit
    ```

    ![git_pretty](../images/git_pretty.jpg)
- --since, --after仅显示指定时间之后的提交。
- --until, --before仅显示指定时间之前的提交。
- --author仅显示指定作者相关的提交。
- --committer仅显示指定提交者相关的提交。
- --grep仅显示含指定关键字的提交
- -S仅显示添加或移除了某个关键字的提交

## remote

- 该关键字用于操纵远程仓库，后面一般加上shortname
- add，`git remote add <shortname> <url>`新增一个远程仓库
  - 其中shortname用于指代后面的url，后面可以用其代替url以简写
  - 默认的shortname是origin
- show查看某一个远程仓库的更多信息
- rename可以重命名
- rm可以移除远程仓库
- -v可以查看

## fetch

- 会将数据拉取到你的本地仓库 - 它并不会自动合并或修改你当前的工作。 当准备好时你必须手动将其合并入你的工作
- 这么做比直接pull安全

## push

- `git push [remote-name] [branch-name]`
- `git push [remote-name] [branch-name]:[remote-branch-name]`
- 只有当你有所克隆服务器的写入权限，并且之前没有人推送过时，这条命令才能生效。
- 当你和其他人在同一时间克隆，他们先推送然后你再推送，你的推送就会被拒绝。 你必须先将他们的工作pull下来并将其merge进你的工作后才能推送。
  - 也就是push之前必须保证你的仓库同步了远程仓库的最新信息

## tag

- 给仓库历史中的某一个提交打上标签, 比较有代表性的是人们会使用这个功能来标记发布结点（ v1.0 、 v2.0 等等）
- Git 使用两种主要类型的标签：轻量标签（lightweight）与附注标签（annotated）
  - 轻量标签很像一个不会改变的分支 ,它只是一个特定提交的引用
- -a附注标签是存储在 Git 数据库中的一个完整对象。 它们是可以被校验的；其中包含打标签者的名字、电子邮件地址、日期时间；还有一个标签信息；并且可以使用 GNU Privacy Guard （GPG）签名与验证
- -d删除指定标签（不会删除对应commit）

## alias

- 别名可以简化你的一些操作
- 示例

  ```bash
  git config --global alias.co checkout
  git config --global alias.br branch
  git config --global alias.ci commit
  git config --global alias.st status
  ```

  以上代码用co代表checkout，其余同理
- 你可以用'xxx'代替上面的checkout，其中xxx是一串指令，这样可以保存你常用的指令串

## branch

- 什么都不跟会查看所有分支
- 后面跟分支名，会新建分支
- -d删除，如果该分支还没合并到主分支，会失败
- -D强制删除，即使没合并也删除
- -v可以查看各个分支的最后一次commit
- 如果要查看哪些分支已经合并到当前分支，可以运行`git branch --merged`，同理，`git branch --no -merged`查看未合并的分支

-

## checkout

- 该指令功能非常多，建议记住几个关键的以免混淆
- 后面跟上分支名可以跳转到该分支
  - 什么都不加会检查当前分支
- -b创建并跳转到新的分支上

## merge

- 后面跟分支名，会把对应分支合并到当前分支上
- 当你删除一个分支的时候，不会把该分支的commit给删除，而是删除了它的索引？
- 当要合并的两个分支存在先后次序，就会简单地把后一个分支的指针移到前一个上
- 如果合并时同一个文件的同一处有冲突，git会暂停并要求你手动修改

  ```xml
  <<<<<<< HEAD:index.html
  <div id="footer">contact : email.support@github.com</div>
  =======
  <div id="footer">
  please contact us at support@github.com
  </div>
  >>>>>>> iss53:index.html
  ```

  - 你可以选择====上下的其中一个版本作为合并目标
  - 保存后把它add，一旦暂存这些原本有冲突的文件，Git 就会将它们标记为冲突已解决
- 对于分别开发推进的两个分支，merge它们的时候会把它们最新的commit和它们的**最优祖先**(由git选择)commit进行一次三方合并，生成一个新的commit并同时指向二者

## rebase

- 后面跟分支名，会把当前分支合并到该分支上
- 变基操作的实质是丢弃一些现有的提交，然后相应地新建一些内容一样但实际上不同的提交，而merge则总是新建提交，不会对历史提交产生影响
- **不要对在你的仓库外有副本的分支执行变基**
- -i会唤起交互式变基本，此时可以修改commit的顺序、修改、弃用、压缩commit等
  - 指令如下：
  ![git-rebase-i](../images/git-rebase-i.png)
  - 示例：
  ![git-rebase-i-example](../images/git-rebase-i-example.jpg)

## stash

- `git stash`用于暂存当前公平做目录+暂存区的内容，可以在切换分支前使用
  - 等同于`git stash save 'msgs'`，其中msgs是注释
    - -u会同时储存未追踪的文件
  - 该命令会清空暂存区，并把工作目录变成上次提交的样子
- list可以列出最近存储的东西
- apply [stash序号]可以应用其中一个储存
- pop和apply基本类似，但是还原后会删除该stash
- show [stash序号]展示某个条目
- `git stash branch [branch-name]`可以根据当前stash新建一个分支
- clear清除所有stash信息
- drop [stash序号]清除特定stash条目
  - 不加序号默认清除最近的条目

## clean

- 该命令会移除工作目录中所有未追踪的文件
- -n可以预演本次将要清理的内容
- -f强制清理
- -d清理空的子目录
- -x会根据.gitignore规则清理文件
- 在使用本命令前，推荐使用stash备份一下

## filter-branch

- **用于大规模修改提交历史，可能会有重大安全隐患**

## notes

- github中新建的仓库会把主分支命名为main，git则会命名为master
- 要留意你的工作目录和暂存区里那些还没有被提交的修改，它可能会和你即将检出的分支产生冲突从而阻止 Git 切换到该分支
  - 请确保每次新建/切换分支的时候，你已经把当前结果commit了
- 运行` git log --oneline --decorate --graph --all `，它会输出你的提交历史、各个分支的指向以及项目的分支分叉情况
- push和pull操作都是在本地分支和其追踪的远程分支上进行的，你也可以手动更改追踪目标
- 建议的分支使用方式：
  ![lr-branches-1](../images/lr-branches-1.png)
  把上面这种分支想象成层序递进的流水线开发
  ![lr-branches-2](../images/lr-branches-2.png)
  并引入不同层级的稳定版本
  常见的开发流程如下：

  考虑这样一个例子，你在 master 分支上工作到 C1，这时为了解决一个问题而新建 iss91 分支，在 iss91 分支上工作到 C4，然而对于那个问题你又有了新的想法，于是你再新建一个 iss91v2 分支试图用另一种方法解决那个问题，接着你回到 master 分支工作了一会儿，你又冒出了一个不太确定的想法，你便在 C10 的时候新建一个 dumbidea 分支，并在上面做些实验。 你的提交历史看起来像下面这个样子：
  ![topic-branches-1](../images/topic-branches-1.png)
  现在，我们假设两件事情：你决定使用第二个方案来解决那个问题，即使用在 iss91v2 分支中方案；另外，你将 dumbidea 分支拿给你的同事看过之后，结果发现这是个惊人之举。 这时你可以抛弃 iss91 分支（即丢弃 C5 和 C6 提交），然后把另外两个分支合并入主干分支。 最终你的提交历史看起来像下面这个样子：
  ![topic-branches-2](../images/topic-branches-2.png)
- 使用git在开发的时候，可以保持本地和远程仓库不相同。比如一些分支我们不push到远程，那它就相当于是私密的
