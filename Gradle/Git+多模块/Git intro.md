# Git intro

## git和github的区别

* git是版本控制系统
* github是部署了git程序的网站，托管代码

## 本地仓库代码上传至远程仓库

* 新建文件夹作为本地仓库，拷贝代码，git bash
* **git init：**将文件夹变成git可管理的仓库
* **git status：**查看当前状态
* **git add：**将项目添加到本地仓库（文件名空格隔开，**git add .** 添加所有文件）
* **git commit -m "注释信息"：**将项目提交到本地仓库，注释信息用来解释该项目
* **git remote add origin 远程仓库地址:**关联远程仓库地址和本地仓库
* **git remote -v:**查看关联的远程仓库
* (**git fetch，git rebase origin/master,git push origin master:master**合并文件)
* (**git pull --rebase origin master:**合并文件)
* **git push (-u) origin master:master：**推送到远程仓库
* github刷新查看

## 工作目录，暂存区，head（各分支不共用）

* ```mermaid
  graph LR
  A(工作目录) --> B(暂存区)
  B --> C(head)
  D(objects) --> B
  ```

* **工作目录：**文件夹显示的目录

* **暂存区：**add命令添加进的文件，.git/index文件

* **objects（对象库）：**add添加的文件对应的对象，.git/objects目录下

* **head：**指向master分支的游标，.git/head文件

* **版本库：**.git目录下，包含暂存区，objects，head

* git add时，暂存区更新，对象库创建对应对象

* git commit时，head更新为当前暂存区目录

* git reset head时，暂存区更新为当前head目录

* git rm --cached <file>时，暂存区删除文件，工作区不变

* git checkout . 时，用暂存区指定文件替换工作区文件，危险操作

* git checkout head . 时，用head指定文件替换暂存区和工作区的文件，危险操作

* git commit -am ‘提交说明'：add和commit

* **rm -rf  .git**：删除本地仓库

## 命令

* 创建仓库
  * **git init**
  * **git clone <url>**：从现有仓库中拷贝项目
  * 查看全局设置：git config -l
  * 查看秘钥：cat id_rsa.pub
* 分支
  * **git branch**：查看分支
  * **git branch --all：**查看所有分支，可能看到**本地的远程分支**
  * **git branch <branchname>**：创建新分支，新分支复制**当前分支**全部内容（？？当前分支还是master分支？？分支暂存区还是head内容）
  * **git checkout -b <branchname>**：创建新分支，并切换到该分支，复制内容
  * **git checkout <branchanme>**：切换分支
  * **git branch -d <branchname>**：删除分支
* git status：
  * 绿色：已缓存但待提交文件
  * 红色：待缓存文件
  * 红色modified：已缓存且已修改文件
* 文件
  * **echo '内容' >> <filename>**：新建文件，并写入内容；或继续写入内容
  * **cat <file>**：查看文件
  * **ls**：查看当前目录下文件
  * **git rm <file>**：只从工作目录中删除文件
  * **git rm -f <file>**：强制删除，从工作目录和暂存区删除文件
  * **git rm --cached <filename>**：只从暂存区删除文件
  * **git mv <file> <filename>**：重命名或移动一个文件，目录，软连接
* git diff：命令显示已写入缓存与已修改但尚未写入缓存的改动的区别
  * **git diff**：查看**未缓存**的修改，即**工作区**中已缓存文件的**未缓存修改**
  * **git diff --cached**：查看**已缓存**的修改，即**暂存区**中已缓存文件的**未提交修改**
  * **git diff head**：查看**已缓存与未缓存**的修改，即**head**相较已缓存文件的**未缓存和未提交修改**
  * **git diff --stat**：显示摘要而非整个diff
  * 总结：git diff查看对象为**已缓存文件的修改**，该修改有三个阶段：**未缓存，已缓存但未提交，已提交**，以上查看刚好包含这三阶段
* 远程仓库
  * **git remote (-v)**：查看链接的远程仓库
  * **git remote add origin url**：与远程仓库建立链接
  * **git remote rm origin**：删除远程地址
* 查看提交历史（**git log**）
  * **--oneline**：查看（简介的）提交历史
  * **--graph**
  * **--reverse**
  * **--author**
  * **--since**
  * **--before**
  * **--until**
  * **--after**

## 标签

* 标签可以作为某次提交的指针
* **git tag**：查看标签
* **git tag (-a) <标签>**：创建标签（并添加标签注解）
* **git tag (-a) <标签> <提交编号>**：给某次提交追加标签
* **git tag -d <标签>**：删除标签
* **git show <标签>**：查看此版本所有修改内容
* **git log --decorate**：查看

## 本地仓库与远程仓库交互

* 前提：本地仓库为自己最新代码；远程仓库最新代码被别人推送过；
* **git fetch <alias>**：拉取远程仓库；将没有的代码复制到本地仓库
* **git merge <alias>/<branch>**：将服务器上任何更新合并到当前分支
* **git pull <alias> <branch>**：拉取远程仓库的某个分支
* **git push <alias> <local branch>:<remote branch>**：推送你的新分支与数据到某个远程仓库