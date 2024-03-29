#### Git提交操作产生的对象

- blob对象：保存文件快照
- 树对象：记录目录结构和blob对象索引(即哈希值)
- 提交对象：包含指向树对象的指针和所有提交信息

#### Git分支

- 本质仅仅是指向提交对象的可变指针
- Git默认分支master，在多次提交后已经有一个指向最后那个提交对象的master分支，并且它会在每次提交操作中自动向前移动合并
- Git有一个HEAD的特殊指针，它会指向当前所在本地分支

#### Git分支新建与合并

- 创建分支前最好先提交当前分支的修改，防止当前分支和要创建并切换过去的分支产生冲突从而阻止Git切换回该分支（也有绕过这一问题的方法，即stashing、amending）
- 合并分支先切换到要合并主分支上，然后 git merge {被合并分支} 
- git merge后一定要进行git status查看是否有冲突，解决完所有冲突后使用git add将每个有冲突的文件标记为冲突已解决，最后git commit进行合并提交

#### Git分支管理

- 查看所有分支最后一次提交:`git branch -v`
-  `--merged` 与 `--no-merged` 这两个有用的选项可以过滤这个列表中已经合并或尚未合并到当前分支的分支 
- 删除分支:`git branch -d {分支名} `；
- 强制删除分支:`git branch -D {分支名} `；

#### Git 分支开发工作流

- 长期分支： 整个项目开发周期的不同阶段，你可以同时拥有多个开放的分支；你可以定期地把某些特性分支合并入其他分支中 
- 特性分支：短期分支，用来实现单一特性或其相关工作。

#### Git 远程分支

- 获取远程分支信息：`git ls-remote `或 `git remote show`
- 同步工作：`git fetch {远程分支}` 或  `git fetch --all `（抓取所有远程分支）

- 跟踪分支： 一个远程跟踪分支检出一个本地分支会自动创建所谓的“跟踪分支”（它跟踪的分支叫做“上游分支”） 
  - 快捷方式：`git checkout --track [remotename]/[branch]`
  - 自定义名称：`git checkout -b [branch] [remotename]/[branch]`
-  修改正在跟踪的上游分支 :`git branch -u [remotename]/[branch]`（ `-u` 或 `--set-upstream-to`  ）
-  查看设置的所有跟踪分支 ：`git branch -vv`
- 删除远程分支：`git push origin --delete {remotebranch}`

#### Git 变基

- 特性分支变基到目标分支: `git rebase [basebranch] [topicbranch]`  
- 多个特性分支比较变基到目标分支：`git rebase --onto master server client` （ 取出 `client` 分支，找出处于 `client` 分支和 `server` 分支的共同祖先之后的修改，然后把它们在 `master` 分支上重放一遍 ）
- 变基后快进合并：1、`git checkout [basicbranch]` 2、`git merge [topicbranch]`
- 下载后进行变基：`git pull --rebase`
- 设置下载后默认进行变基：  `git config --global pull.rebase true` 