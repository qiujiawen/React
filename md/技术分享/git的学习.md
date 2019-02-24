### 记录和收集一些git的常用命令

Git 是一个管理你的「代码的历史记录」的工具

#### git的三个区

>工作区：我们建立的项目文件夹即工作区 ，我们在工作区修改增加代码

>暂存区：在.git 文件夹里面还有很多文件，其中有一个index文件就是暂存区；作用是作为过渡层，避免误操作，保护工作区和版本区，分支处理。
       
>版本区：初始化git版本库之后会生成一个隐藏的文件 .git ，
       可以将该文件理解为git的版本库 repository。

### 设置配置

    git config --global user.name [name]  //git的用户名
    git config --global user.email [邮箱地址]  //git的登录账号
    git config --list  //查看配置的信息

### 创建一个工作区

    git init 

### 克隆项目

    git clone [url]

### 查看当前文件的状态
    git status 

### 把文件添加到仓库中暂存区

    git add [fileName] 　//把[fileName]文件添加到暂存区 
    git add . 　　　　　　//将工作区所有文件提交到暂存区


### 代码提交到仓库

    git commit -m "注释文本"
    git commit -a -m "注释文本"     //跳过使用暂存区域,直接提交

### 上传日志

    git log                　　　　//查看历史区的上传日志（显示不全按回车继续，输入q退出）
    git log --oneline             //查看历史区的上传日志（单行显示）
    git log --graph               //查看历史区的上传日志（图形化显示）
    git reflog                    //查看所有的提交

### 对比

    git diff                　　　　//查看工作区与暂存区的区别
    git diff head                  //查看工作区与历史区的区别
    git diff --cached              //查看历史区与暂存区的区别

### 撤销

    git reset HEAD [fileName]       //暂存区的内容撤销到工作区
    git checkout [fileName]         //用暂存区中的内容或者版本库中的内容覆盖掉工作区
    git commit --amend              //误提交撤销回来修改好后重新提交

### 删除

    git rm [fileName]               //工作区某文件已删除，提交到暂存区的该文件才能删除
    git rm -f [fileName]            //某文件在工作区和暂存区存在时运行命令可以同时删除
    git rm --cached [fileName]      //某文件在工作区和暂存区存在时只删除暂存区的文件

### 恢复

    git checkout commitId [fileName]   //恢复某一指定文件
    git reset --hard commitId       //恢复某一指定版本
    git reflog                      //查看所有的提交

### 同步到远程仓库

    git remote                      //查看远程仓库的名字，默认是origin
    git remote  -v                  //查看远程仓库的地址
    git push origin master          //同步到远程仓库的master分支上

### 多人协作，解决冲突的情况(更新代码)

    git fetch                       //多人协作同步远程仓库起冲突时，运行命令文件不合并需手动操作
    git diff master origin/master   //步骤一、查看本地仓库和远程仓库的区别
    git merge origin/master         //步骤二、合并本地仓库和远程仓库
    
    git pull                        //多人协作同步远程仓库起冲突时，运行命令文件合并需手动操作，省略步骤一二


### 开源项目的协作

    fork
    pull request


### git的分支

    git branch                      //列出所有本地分支

### 创建分支

    git branch [branchName]         //新建一个分支，但依然停留在当前分支
    git branch -b [branchName]      //新建一个分支，并切换到该分支
    git branch --merged             //查看已经合并到当前分支的所有分支
    git branch --no-merged          //查看没有合并到当前分支的所有分支

### 切换分支
    git checkout [branchName]       //切换到指定分支，并更新工作区
    git checkout -b [branchName]    //新建和切换到[branchName]分支
    git checkout -b [branchName] master  //基于master新建[branchName]分支，并切换
    
### 删除分支

    git branch -d [branchName]      //在当前分支上删除已经合并的分支
    git branch -D [branchName]      //在当前分支上强制删除没有合并的分支
    
### 合并分支

    git merge [branchName]          //将[branchName]分支合并到当前分支


### 分支同步到远程仓库

    git push origin [branchName]
    git push --all origin           //推送所有分支到远程仓库中
    //GitHub直接创建删除

### github上的标签

    //GitHub直接创建
    git tag                         //列出现有标签
    git tag v1.0.0                  //新建标签  

### 删除远程仓库的.idea文件

```
git status                                       //查看当前状态，确认是否已经删除了iml文件
git rm -r --cached .idea                         //--cached不会把本地的.idea删除
git commit -m 'delete .idea dir'
git push -u origin master
```