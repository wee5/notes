# GIT报错

* **fatal: refusing to merge unrelated histories**

  * 使用git merge origin/<branch> --allow unrelated histories（merge可换pull）

* ##### We found potential security vulnerabilities in your dependencies.

  * github自动创建三个分支，分别以报错的依赖命名，并在分支中更改了依赖版本；
  * 三个分支pull request给master，目的将更改依赖后的代码整合到master上；
  * pull request就是请求别人pull自己的仓库