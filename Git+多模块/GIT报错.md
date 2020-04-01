# GIT报错

## git

* **fatal: refusing to merge unrelated histories**
  * 使用git merge origin/<branch> --allow unrelated histories（merge可换pull）
* **git fetch命令后，没有任何反应**
  * 本应该拉取远程仓库的一个分支；使用git branch --all命令，可以看到红色的刚拉取到的本地的远程仓库分支，再使用git merge，后面照常

## github

* ##### We found potential security vulnerabilities in your dependencies.

  * github自动创建三个分支，分别以报错的依赖命名，并在分支中更改了依赖版本；
  * 三个分支pull request给master，目的将更改依赖后的代码整合到master上；
  * pull request就是请求别人pull自己的仓库