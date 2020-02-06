# Git 删除分支

git 删除远程除 develop, master 以外的分支
`git branch -r| grep -v -E 'master|develop' | sed 's/origin\///g' | xargs -I {} git push origin :{}`

删除本地包含 feature 的分支
`git branch | grep 'feature' | xargs git branch -D`

如果有些分支无法删除，是因为远程分支的缓存问题，可以使用
`git remote prune origin --dry-run` 测试

测试完成后执行
 `git remote prune origin`  

批量删除本地 tag
`git tag | xargs -I {} git tag -d {}`
批量删除远程 tag
`git tag | xargs -I {} git push origin :refs/tags/{}`
用到命令说明
grep -v -E 排除 master 和 develop
-v 排除
-E 使用正则表达式
xargs 将前面的值作为参数传入 git branch -D 后面
-I {} 使用占位符 来构造 后面的命令
