# learnGit
GitHub Notes
## git shh 秘钥生成
- ssh-keygen (生成秘钥)
- ssh配置：本地私钥,远程github存放公钥
## git 推送和关联 （本地与远程分支的关联操作）
```js
echo "# yang" >> README.md
git init
git add README.md
git commit -m "first commit"
git remote add origin https://github.com/yang997/yang.git
本地->远程
    git push
    方法一
    git push -u origin master
    git push -u origin dev
    方法二
    git push --set-upstream origin test
远程->本地
    1.pull 远程->追踪
    2. 追踪->本地
    方法一
    git checkout -b dev origin/dev
    方法二
    git checkout -b test --track origin/test
    或 git checkout --track origin/test



```
```js 
已有文件
git remote add origin https://github.com/yang997/yang.git
git push -u origin master
```
- git remote add origin git@github.com:yang997/yang.git(ssh方式)
- git remote add origin https://github.com/yang997/yang.git (https方式)
- git push -u origin master (本地master分支关联到远程origin分支) 后续修改推送执行执行git push
- 
## 分支的理解
- master 生产阶段,分支很少变化
- dev 开发分支,频繁改变
- test 基本开发完毕后,交给测试的分支
- bugfix 临时修复bug分支
- git 会在本地维护 origin/master分支,通过该分支感知远程github的内容变化(如何查看origin/master分支:git checkout origin/master cd .git/,)
## 指令的解释
- touch XXX 创建文件
- git add . 提交暂存区
- git commit -m 'XXX' 提交对象区
- git commit -am 'XXX' 合并add 和commit
- rm -rf * 删除当前目录下的所有文件，不放回收站,隐藏目录无法删除
- git push (本地推送到远程)
- cd - 
- git pull: 等于fetch + merge (从远程拉取到本地) 
- git remote show (查看有多个远程服务器,服务器列表)
- git remote show origin (查看远程仓库)
- git branch 列出当前分支清单
- git branch -a 查看远程分支和本地分支
- git branch -av 查看远程分支和本地分支提交点
- git status 查看状态
- git log --graph 
- git log --graph --pretty=oneline --abbrev-commit
- git rm XXX 1.删除 2.暂存区 删除已提交的文件,删除之后文件被放到暂存区,git commit -m 'XXX'彻底删除(后悔git rm 执行1.git reset HEAD XXX(工作区)然后2.git checkout -- XXX)
- 操作系统删除 rm XXX 1.删除 2.工作区 (后悔git checkout -- XXX)

## 重命名
- git mv a.txt a1.txt (将a.txt重命名a1.txt)后悔执行1.git reset HEAD XXX(工作区)然后2.git checkout -- XXX
- git branch -m master master2
## 注释重写
- 上一次注释重写 git commit --amend -m'XXX'
  
## stash 保存现场
- 1.建议（规范）：在功能没有开发完毕前，不要commit。2.规定（必须）：在没有commit之前，不能chekcout切换分支(不在同一个commit阶段)
- 如果还没有将某一个功能开发完毕 就要切换分支：建议 1.保存现场（临时保存stash）2.切换
- git stash 保存现场
- git stash pop (将原来保存的删除，用于还原内容)
- git stash apply(还原内容,不删除原保存的内容)，可以指定某一次现场git stash apply stash@{n} 手工删除现场：git stash drop stash@{0}
- git stash list 查看现场

## 忽略文件 gitignore

## 分支
- 查看分支git branch 
- git branch -v 查看最近提交的sha1值
- git branch XXX 创建分支
- git checkout XXX 切换分支
- git checkout -b XXX 创建并切换分支
- git branch -d XXX 删除分支(不能在当前分支删除)其他不能删除情况：包含"未合并"的内容
- git branch -D XXX 强行删除分支
- 细节：1.如果在分支A中进行了写操作，但此操作局限在工作区中进行（没add commmit)。在master中能够看到该操作。如果分支A中进行了写操作 commit(对象区),则master中无法观察到此文件 2.如果在分支A中进行了写操作，但此操作局限在工作区中进行（没add commmit)。删除A分支是可以成功的
- 如果一个分支靠前(dev),另一个落后(master)。则如果不冲突，master可以通过merge 直接追赶dev,称为fast forward。 fast forward 本质就是 分支指针的移动。注意：跳过的中间commit,仍然会保存。
- git 在merge时，默认使用fast forward; 也可以禁止：git merge --no-ff 1.两个分支fast forward，不会归于一点commit(主动合并的分支 会前进一步) 2.分支信息完整(不丢失分支信息)
- 合并：如果冲突，需要解决冲突。解决冲突：git add XXX(告知git,冲突已解决), git commit -m XXX,注意：maseter在merge时，如果遇到冲突，并解决冲突，会进行2次提交，1次是最终提交，1次是将对方dev的提交信息也拿来了。如果一方落后，另一方前进。则落后方可以直接通过merge合并到前进方

## 版本穿梭 在多个commit之间 进行穿梭。回退、前进
- 回退上一次commit： git reset --hard HEAD^
- 回退上二次commit： git reset --hard HEAD^^
- 回退到上n次commit: git reset --hard HEAD~n
- 回退到前XX次commit：通过sha1值 直接回退 git reset --hard sha1值的前几位
- git reflog 查看记录,记录所有操作。可以帮助我们实现“后悔操作”。需要借助于良好的注释习惯

## checkout 放弃与游离
- checkout 放弃修改。放弃的是工作区中的修改。相对于暂存区或对象区
- git reset HEAD XXX 将之前增加到暂存区中的内容 回退到工作区
- git checkout sha1值 版本穿梭(游离状态)1.修改后、必须提交2.创建分支的好时机 git branch XXXX sha1值 (创建平行宇宙，不干扰之前的版本)

## 操作本地与github的分支和标签
- 删除本地分支 git branch -d dev(分支名) 或git branch -D dev(分支名)
- 删除远程分支 git push origin :dev(分支名) 或 git push --delete dev(分支名)
- 本地没有a分支，但本地却感知远端的a分支。检测：git remote prune origin --dry-run
- 清理无效的 追踪分支（本地中感知的远程分支） git remote prune origin
- Tag标签： 适用于整个项目，和具体的分支没关系
- git tag XXX(简单标签,只存储当前的commit的sha1值) 
- git tag -a XXX -m "XXX "(带注释标签m,创建一个新对象，会产生一个新的commit/sha1存储信息，其中包含了当前的commit的sha1值)
- git push origin XXX 标签名，推送
- git push origin --tags 推送所以没推送的标签
- git pull 或获取远程标签 git fetch orgin tag 
-  查看标签 git tag 删除标签 git tag -d 标签名， git show 标签名
## 问题解决
git push -u origin master 出现下面问题
```js
    ! [rejected]        master -> master (non-fast-forward)
error: failed to push some refs to 'git@github.com:yang997/learnGit.git'
hint: Updates were rejected because the tip of your current branch is behind
hint: its remote counterpart. Integrate the remote changes (e.g.
hint: 'git pull ...') before pushing again.
hint: See the 'Note about fast-forwards' in 'git push --help' for details.
```
- 先使用 git pull --rebase origin master然后再使用命令git push -u origin master