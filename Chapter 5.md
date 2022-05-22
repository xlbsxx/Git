# Git内部原理
## .git的目录结构
在创建一个名为`test`的新仓库后，新仓库会自动创建一个`.git`目录，目录结构如下：
```
├── *config               配置信息(比如本地repo是否大小写敏感, remote端repo的url, 用户名邮箱等) 
├── description           无需关心（仅供GitWeb程序使用）
├── *HEAD                 目前被检出的分支
├── index                 保存暂存区信息
│
│
├── hooks/                钩子脚本（执行Git命令时自动执行一些特定操作）
├── info/                 包含一个全局性排除文件
│   └── exclude           放置不希望被记录在 .gitignore 文件中的忽略模式
├── logs/                 记录所有操作
├── *objects/             存储所有数据内容
│   ├── SHA-1/            保存git对象的目录（三类对象commit, tree和blob）
│   ├── info/
│   └── pack/             
└── *refs/                存储指向数据（分支、远程仓库和标签等）的提交对象的指针
    ├── heads/           
    ├── remotes/         
    └── tags/            
```
## objects目录——存储
初始化仓库后：objects目录下只有子目录`pack`和`info`，但均为空。

在添加五个文件后，查看objects目录，会发现新增了5个子目录。
```
.git/objects
├── 40
│   └── fa006a9f641b977fc7b3b5accb0171993a3d31
├── 5b
│   └── dcfc19f119febc749eef9a9551bc335cb965e2
├── 67
│   └── f0856ccd04627766ca251e5156eb391a4a4ff8
├── 87
│   └── db2fdb5f60f9a453830eb2ec3cf50fea3782a6
├── ac
│   └── f63c316ad27e8320a23da194e61f45be040b0b
├── info
└── pack
```
其中前两个字符用于命名子目录，例如`40`
校验和余下的 38 个字符用于文件名，例如`fa006a9f641b977fc7b3b5accb0171993a3d31`

合起来`40fa006a9f641b977fc7b3b5accb0171993a3d31`为SHA-1 哈希值。

这时可以手动执行`git gc`命令，对文件进行打包。

这时查看objects目录，会发现很多子目录不见了，而`info`和`pack`目录非空。
```
.git/objects
├── info
│   ├── commit-graph
│   └── packs
└──  pack
    ├── pack-XXX.idx
    └── pack-XXX.pack
 ```
* 包文件`pack-XXX.pack`：包含了刚才从文件系统中移除的所有对象的内容；

* 索引文件`pack-XXX.idx`：包含了包文件的偏移信息。通过索引文件可以快速定位任意一个指定对象
## refs目录 —— 引用
Git把一些常用的 SHA-1 值存储在文件中，用文件名来替代，这些别名就称为引用。有三种引用类型：heads, remotes和tags。
eg.
```
.git/refs
├── heads         记录本地分支的最后一次提交
│   ├── master
│   └── test
├── remotes       记录远程仓库各分支的最后一次提交
│   └── origin
│         └── main
├── tags
│   └── v1.0
└── stash
```
### HEAD引用
HEAD引用有两种类型：
* 分支级别：存储在`.git/refs/heads`目录下,指代本地分支的最后一次提交，有多少个分支，就有多少个同名的HEAD引用。文件内容：commitHash
* 代码库级别：存储在`.git/HEAD`文件，指代当前代码所处的分支；（拓展：也可指代commitHash（称为分离HEAD））。文件内容：符号引用 - 例如 ref: refs/heads/master
### remote引用
* 存储位置： .git/refs/remotes 目录下
* 指代内容：远程仓库各分支的最后一次提交
* 注意点：用于记录远程仓库；文件是只读的，乱改就崩了
### tag引用
tag主要用于发布版本的管理：一个版本发布之后，我们可以为git打上 v1.0 v2.0 ... 这样的标签
* 存储位置：.git/refs/heads 目录下
* 指代内容：tag可以指向任何类型（更多的是指向一个commit，赋予它一个更友好的名字）
* 文件内容：SHA-1值
### stash
* 存储位置：.git/refs/stash 文件
* 指代内容：当你想转到其他分支进行其他工作，又不想舍弃当前修改的代码时 - stash可把当前的修改暂存起来
## config文件 —— 引用规范
一些常用命令：
* 连接远程仓库： `git remote add <远端名origin> <url>`
* 拉取分支：	`git fetch <远端名origin> <远端分支名>:<本地分支名>`
* 将远程的 main 分支拉到本地的mymaster 分支：	`git fetch origin main:mymaster`
* 将本地的master分支推送到远端的topic分支：	`git push origin master:topic`
* 删除远端分支topic：	法1：将<src>留空：`git push origin :topic`  法2：Git v1.7.0新语法：`git push origin --delete topic`

##  config文件 —— 环境变量
Git有三种环境变量：

1）系统变量

适用范围：对所有用户都适用
命令选项：`git config --system`

2）用户变量

适用范围：只适用于该用户
命令选项：`git config --global`

3）本地项目变量

适用范围：只对当前项目有效
命令选项：`git config --local`
存储位置：`.git/config`
一些常用命令：
* 查看所有配置：
* 配置用户名：`git config --global user.name "你的用户名"`
* 配置邮箱：`git config --global user.email "你的邮箱"`
