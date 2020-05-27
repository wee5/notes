# Gradle：idea导入gradle项目



## 准备

### 下载gradle全量包

* 在gradle项目的gradle.properties中查看gradle项目版本

* 在https://services.gradle.org/distributions/下下载对应版本gradle全量包

* 设置环境变量

  * ```
    GRADLE_HOME=H:\Gradle-3.0
    GRADLE_USER_HOME=H:\Gradle-rep
    PATH=%GRADLE_HOME%\bin
    ```

### 阿里云镜像替换mvn中央仓库

* 在%GRADLE_HOME%\init.d下新建文件init.gradle

* init.gradle内容

  * ```gradle
    allprojects {
        repositories {
            def REPOSITORY_URL = 'http://maven.aliyun.com/nexus/content/groups/public/'
            all { ArtifactRepository repo ->
                if (repo instanceof MavenArtifactRepository && repo.url != null) {
                    def url = repo.url.toString()
                    if (url.startsWith('https://repo1.maven.org/maven2') || url.startsWith('https://jcenter.bintray.com/')) {
                        project.logger.lifecycle "Repository ${repo.url} replaced by $REPOSITORY_URL."
                        remove repo
                    }
                }
            }
            maven {
                url REPOSITORY_URL
            }
        }
    }
    ```



## 导入gradle项目

* 打开项目，选中build.gradle
  * ![avatar](图片引用\Snipaste_2020-05-09_16-19-10.png)

* idea开始构建项目，下载依赖，大约20分钟后，项目构建完成



## 启动项目

* 配置本地服务器（local server：tomcat），添加artifact，找到**对应**的war:explore；配置其他
* 启动项目

