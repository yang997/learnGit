# learnGit
GitHub Notes
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