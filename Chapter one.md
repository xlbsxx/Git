# 第一章 Git简介

## 1.1 简介与历史
Git是一款免费、开源的分布式版本控制系统，用于敏捷高效地处理任何或小或大的项目。

## 1.2 Git的安装
在 Windows 上安装 Git 也有几种安装方法。 我们也可以打开 https://git-scm.com/downloads 进行下载安装。安装好后我们可以打开Git bash。  另一个简单的方法是安装 GitHub Desktop。 该安装程序包含图形化和命令行版本的 Git。 它也能支持 Powershell，提供了稳定的凭证缓存和健全的换行设置。

## 1.3 初次运行Git的配置
当我们安装好Git后，还需要在Git bash或者terminal进行一些相关设置，以下设置仅需设置一次即可。

git config --global user.name "Your Name"
git config --global user.email "email@example.com"

除此之外，Git还有很多设置，包括常用编辑器等，大家可以键入以下命令查看自己的设置并进行修改。

git config --list
