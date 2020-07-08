# idea内置git



本地仓库关联远程仓库，需要从clone开始



## 项目关联仓库

新建远程仓库，复制地址

用任意项目中，VCS>checkout from version control>git>粘贴远程仓库地址>一直下一步>create project

此时新项目中没有任何模板>file>project structure>modules>+>new module>spring initializr（添加合适的模板）

此时的项目即有本地仓库，又关联了远程仓库

### 另一种方式

这种方式有点问题，实际上并未关联到远程仓库，使用命令时可以解决git pull origin branchname --allow-unrelated-histories，但内置git无法解决

新建项目

将项目目录作为本地仓库：VCS>import into version control>create git repository

VCS>GIT>git remote>+>粘贴远程仓库地址

此时也形成了项目，本地仓库，远程仓库的关系，但是拉取时会报错



## 代码管理

添加至暂存区：控制台>version control>local changes>unvsersioned files>选中需要add的文件，VCS>add

提交：VCS>commit

拉取：VCS>GIT>pull

上传：VCS>GIT>push



## 分支管理

### 新分支上开发

右下角分支>new branch>此时切换到新建分支>在新建分支上开发

查看其它分支前，应先将当前分支提交，避免其它分支上出现当前分支未提交的修改

### 分支的合并

第一种：将分支直接push到远程仓库，并新建相应的分支

第二种：切换到master分支上，将新分支合并到master分支，再push到远程仓库master分支



## 版本管理



## 常见问题

提交时出现unversioned files，并且不能提交该文件的修改

​	原因：该修改未add

​	解决：项目开始前就“始终add修改”，或手动add



分支修改后，master也被修改

​	原因：分支树展示的是提交后的结果，（因为commit之后，该分支就在本地仓库确定了，而本地仓库和远程仓库是同级别的，所以不需要push）

​	解决：每次在分支修改后，提交一下再查看其它分支，就不会出现其他分支出现同样修改的情况了

​	补充：**分支树显示提交后的结果，并不同分支的每次提交，并不是用来切换分支的，切换分支到右下角**