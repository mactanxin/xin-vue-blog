# Git使用指南
description: Git开发流程使用指南
## 初始化流程
### 切换至本地要clone git仓库的目录
```shell
cd /target_dir/
```
### clone develop分支
```shell
git clone -b develop https://username:password@git.github.com:7443/组织名/仓库名
```
### 添加upstream地址

```shell
git remote add upstream https://username:password@@git.github.com:7443/组织名/仓库名
```

## 开发流程
### 切换到develop分支
```
cd develop
git checkout develop
```
### 在develop上创建自己的开发分支
```shell
git checkout -b feature/myfeature develop
```
#本地开发后提交到feature分支 
```shell
git add . 
```
### 提交修改内容
#### 在提交修改内容前需要确保当前本地分支已同步upstream的最新版本
```shell
git fetch upstream
git checkout develop
git merge upstream/develop
# 如果远程版本有更新, 需要rebase自己的分支
git checkout newfeature
git rebase develop
```
#### 完成rebase后提交代码
```
git commit -m ‘注释’ 
git push -u origin feature
```
### 删除本地feature分支

```
git branch -d feature/myfeature
```
## 删除远程分支
```
git push origin :feature
```

