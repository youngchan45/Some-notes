git存放文件的思路其实和svn差不多，都是在本地新建一个文件夹用以存放项目。首先新建文件夹，然后把仓库链接放进去（约等于svn的checkout），然后再在这个文件夹里面存放项目相关的东西，后续要提交项目的话，应该进入到这个文件夹里面才可以add

### 上传代码

1. 现在github上新建仓库（记得默认添加readme.md文件）

2. 在本地新建一个总的文件夹project

3. 打开文件夹，打开git，进行初始化和克隆仓库

   ```
   git init
   git clone https://github.com/youngchan45/one.git
   ```

4. 之后便可以在project总文件夹里面进入仓库，在仓库里面新建文件夹1-demo1、文件夹2、文件夹3...以存放项目1、项目2、项目3

5. 在文件夹demo1里面进行项目开发，将代码提交上去github

   ```
   git add .
   git commit -m '提交注释'
   git push origin master
   ```

### 更新代码

1. 

# 稍微复杂git（经实践顺序有点错误）

------



## 一.  准备工作

1. 下载安装Git，右键点击Git Bash Here

2. 首次配置git需要验证**账号**、**邮箱**和设置**ssh key**

   2.1 验证账号和邮箱：$ git config --global user.name"xxxxx"

   ​							       $ git config --galobal user.email"xxxxxxxx@xxxx"

   2.2 设置ssh key

   ​		2.2.1 首先检查是否已有密钥：$ cd ~/. ssh

    													    $ ls

   ​				 若已生成，则返回三个文件![](C:\Users\Young\Desktop\WeChat 圖片_20190519175013.png)

    		2.2.2 若未生成，输入 $ ssh-keygen -t rsa -C "xxxxxxxxx@xxxx"

   ​		 2.2.3 生成后按3次回车（默认路径、默认不用密码登录、确认默认不用密码登录）

   ​		 2.2.4 前往放置密钥的文件夹，用记事本或者notepad++打开id_rsa.pub后，复制密钥内容

   ​		 2.2.5 在github的settings --> SSH and GPG keys 中添加密钥

   ------

   ## 二.  上传本地项目至Github

   1. 进入项目文件夹

      （傻瓜式）：关闭先前打开的git，前往项目文件夹，在项目文件中右键选择Git Bash Here

      （命令式）：在git中打开文件夹 $ cd d:github 

      若为多级文件夹，则需要一级级地输入cd打开（百度不到其他更为简便的命令）![](C:\Users\Young\Desktop\WeChat 圖片_20190519182818.png)

   2. 初始化 $ git init ，初始化成功后文件夹中多了一个隐藏文件夹 .git

   3. 添加文件到暂存区

      添加全部文件： $ git add .

      添加单独文件：$ git add index.js

      添加多个文件：使用空格分隔 git add index.js index.css

      添加文件夹：$ git add img/

   4. 提交暂存区的文件到本地仓库，双引号内为注释，必填，方便版本回顾和管理  $ git commit -m"first commit"

   5. 将本地仓库和远程仓库连接（只需要连接一次）$ git remote add origin https://github.com/name/name_cangku.git

      如果出现错误：fatal: remote origin already exists，则执行 $git remote rm origin 后再次执行连接

   6. 把本地仓库提交到远程仓库 $ git push origin master

      **PS: 在github上新建仓库一定要勾选添加README.md文件！**

   ------

   

   ## 三. 将github上的仓库关联至本地

   1. 复制仓库地址

      新仓库直接复制；

      已有仓库则点击：![](C:\Users\Young\Desktop\WeChat 圖片_20190519184056.png)

   2. 执行 $git remote add origin https: //github.com/youngchan45/xxx.git

   3. 上传本地代码 $ git push -u origin master

      