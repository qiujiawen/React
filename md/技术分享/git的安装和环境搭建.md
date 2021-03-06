## git的安装和环境搭建

#### 安装homebrew

具体方法参考官网：https://brew.sh/

#### 安装git

安装homebrew 成功后，使用以下指令安装Git

```
brew install git
```

#### 配置git

配置用户名和邮箱

```
git config --global user.name "your_name"
git config --global user.email "your_email@gmail.com"
```

配置完后，使用以下指令查看Git的配置信息

```
git config --list
```


#### 密钥的生成

```
ssh-keygen -t rsa -C "your_email@youremail.com"
```


生成密钥后，在本地的/Users/当前电脑用户/.ssh目录下会生成两个文件id_rsa、id_rsa.pub，id_rsa文件保存的是私钥，保存于本地，id_rsa.pub文件保存的是公钥，需要将里面内容上传到远端仓库。


> 获取公钥字符串

```
cd ~/.ssh
cat id_rsa.pub
```


>添加到账户上

登陆GitHub，打开“Account settings”，“SSH Keys”页面；然后，点“Add SSH Key”，填上任意Title，在Key文本框里粘贴id_rsa.pub文件的内容。点“Add Key”，就应该看到已经添加的Key。

>git的过滤规则

在Git工作区的根目录下创建一个特殊的.gitignore文件，然后把要忽略的文件名填进去，Git就会自动忽略这些文件。

.gitignore文件完整代码示例：

    # Numerous always-ignore extensions
    *.bak
    *.patch
    *.diff
    *.err

    # temp file for git conflict merging
    *.orig
    *.log
    *.rej
    *.swo
    *.swp
    *.zip
    *.vi
    *~
    *.sass-cache
    *.tmp.html
    *.dump

    # OS or Editor folders
    .DS_Store
    ._*
    .cache
    .project
    .settings
    .tmproj
    *.esproj
    *.sublime-project
    *.sublime-workspace
    nbproject
    thumbs.db
    *.iml

    # Folders to ignore
    .hg
    .svn
    .CVS
    .idea
    node_modules/
    jscoverage_lib/
    bower_components/
    dist/



