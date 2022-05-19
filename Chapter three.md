# 第三章 分支管理
## 3.1创建分支
创建分支：`git branch +分支名`

查看现有分支：`git branch`
```
# 现有的分支
issue102
*master
```
带*表示我们当前所处该分支上。

## 3.2分支切换
切换分支：`git checkout +分支名`

## 3.3分支合并
合并分支:`git merge +分支名`

eg.将`issue102`合并到`master`上：
```
# 切换回主分支
git checkout master
# 使用git merge 进行合并
git merge issue102
# git branch --no-merged
# 查看所有未合并工作的分支
```

## 3.4 分支推送到远程
查看远程库的详细信息：`git remote -v`

推送本地分支到远程：`git push origin +分支名`
（origin)为远程主机名
## 3.5分支的删除
删除本地分支：`git branch -d +分支名`

删除远程分支：`git branch origin --delete +分支名`
## 3.6分支重命名
重命名分支的名称：`git branch -m +旧名字 +新名字`

将改名后的分支推送到远程：
```
git branch -m +旧名字 +新名字   # 将本地的分支进行重命名
git push origin +新名字               # 将新的分支推送到远程        
git push --delete origin +旧名字      # 删除远程的旧的分支 
```
