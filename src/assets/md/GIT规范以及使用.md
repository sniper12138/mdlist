# GIT规范以及使用

#### 由于目前部门采用代码仓库管理代码，需要使用该工具上传到对应的代码仓库地址

  
**安装**

### 基于命令行操作：

官方Git-bash可前往
[官网下载](https://git-scm.com/download/win)，然后按默认选项安装即可。

但官方bash用户体验有点挫，推荐使用体验更好的Cmder来操作（具体有多好，体验了就知道离不开了，也可以看
[这篇介绍](https://www.jianshu.com/p/5b7c985240a7)，Cmder还自带了Git），可在
[Cmder官网](https://cmder.net/) 下载。

### 基于界面操作：

目前我们都是用Windows，推荐下载
[老乌龟客户端TortoiseGit](https://tortoisegit.org/download/)
来进行界面化操作，支持中文，体验友好。

## **使用**

这里只介绍命令行操作，玩懂了命令行，界面操作也就自然就懂了。

我们一般只提交到develop分支，如果需要提交develop（含个人develop）以外的分支（特别是master），**操作前请必须知会黄通**。

### 常用操作步骤和命令参考：

| **操作步骤** | **命令语法**                                         | **操作含义**                                                     | **示例**                                                |
|--------------|------------------------------------------------------|------------------------------------------------------------------|---------------------------------------------------------|
| 1            | git clone <仓库名>                                 | 克隆仓库到本地                                                   | git clone <http://192.168.1.246:3000/app/tt_js_sdk.git> |
| 2            | git checkout -b <新分支名>                         | 在本地创建一个新分支，并切换到这个分支                           | git checkout -b develop                                 |
|              | git checkout <分支名>                              | 切换本地分支                                                     | git checkout develop                                    |
| 3            | git pull origin <远程分支名>                       | 拉取远程仓库某个分支的文件到本地（保证提交到线上的代码是最新的） | git pull origin develop                                 |
| 4            | git add <文件名>                                   | 提交文件到暂存区（可用 . 提交所有）                              | git add .                                               |
| 5            | git commit -m <提交原因>                           | 填写本次更新的内容                                               | git commit -m "本次更新的内容"                          |
| 6            | git push -u <远程仓库名> <本地分支>:<远程分支> | 把本地某个分支的文件，推送到远程仓库指定的分支下                 | git push -u origin develop:develop                      |

### **辅助命令参考：**

| **命令语法**  | **操作含义**                                                      |
|---------------|-------------------------------------------------------------------|
| git branch -a | 查看当前的分支情况（本地/远程分支，以及激活的分支情况）           |
| git remote -v | 查看当前存在的远程仓库及分支                                      |
| git status    | 查看当前git工作区情况，可以看到工作区中有哪些文件还未添加到暂存区 |
| git ls-files  | 查看暂存区中有哪些文件                                            |
| rm .git/index | 清空暂存区                                                        |

**提前须知**

master:
存储了正式发布的历史,为主分支(保护分支)，不能直接在master上进行修改代码和提交

develop:作为功能的集成分支,开发完成需要提交测试的功能合并到该分支

release: 发布分支，主要用于测试或修复bug

master_fixBug: 基于master分支创建的解决紧急bug的分支

其余分支为每个人对应各自的开发分支，无特殊情况，不允许私自另外建分支。

## **本地分支篇常用指令**

```
1.git branch <分支名字> 创建分支 (不切换分支)

2.git checkout -b <分支名字> 创建分支且切换到该分支 （ps:建议使用这个）

3.git merge <对应分支名字> 合并对应分支到该分支上
（ps:如需远程仓库也合并，可在本地合并到该分支上后上传）

4.git branch 查看当前有多少分支

5.git checkout <分支名字> 切换到该分支
(当需要跳转到对应分支时使用，注意这时该分支需要已存在)
```

## **拉取远程仓库分支流程**

```
1.本地新建文件夹

2.cd 进新建的文件夹内，然后 git init 初始化

3.通过 git remote add origin 《对应远程仓库》 与远程仓库建立连接

4.git fetch <拉取对应分支名字>

5.git checkout -b <创建一个与远程仓库分支同名的本地分支>

6.git merge FETCH_HEAD 把缓存区的内容合并到当前分支

7.或者git pull origin <远程分支名称> 直接拉取对应内容到当前分支
（ps:这个步骤等于 fetch+merge）
```

## **项目初始化**

```
1.git init

2.创建一个README.md

3.git checkout -b <分支名>

3.git add *

4.git config --global user.name “你的用户名”

5.git config --global user.email “你的邮箱”

6.git commit -m "first commit"

7.git remote add origin 《对应远程仓库》 与远程仓库建立连接

8.git push -u origin <分支名>

9.如果本地已创建master，release，master_fixBug，develop分支。则可通以下方法为远程仓库创建对应分支：

9.1.git push --set-upstream origin <分支名>

9.2.git push origin <分支名>：<分支名>

PS：

1.如果无分支要求，则第三步可跳过，初始化分支为master，否则，为你传的分支名

2.如果使用的是ssh传输，则第四五步可跳过

3.要使用第九步的话，需要本地先建立对应分支，否则会报错

4.第一次初始话，可先使用测试分支，然后通过第九步建立其余分支
```

**更新**

PS：上传是注意好自己分支与远程分支是否对应，如不对应，本地分支请先切换好，避免上传错分支  
正常单人开发：  
优先使用：add-&commit-&push （个人可不用上传前pull）  
多人公用仓库：  
优先使用：pull -status -add-commit-push（一般情况下，以这个为主）

## **分支管理**

#### 1.develop为集成开发分支

· 自己分支自测完毕；  
· review通过再合并；

#### 2.本地分支管理

· 本地分支要做到勤提交，分小功能提交，提交的节点尽可能小；  
· 本地分支merge到develop分支时，必须先merge develop到本地分支，自测通过再提交；

#### 3.注意事项

· 开发者相同版本尽量不要修改相同功能，提前划分或协商清楚；  
· 如果修改代码涉及多人功能，提交完毕请及时告知相关人员更新代码；  
· 开发者每天更新develop分支内容到本地分支，避免大规模merge;
