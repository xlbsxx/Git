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
## objects目录
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
合起来`40fa006a9f641b977fc7b3b5accb0171993a3d31`为SHA-1 哈希值
