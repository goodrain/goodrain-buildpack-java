Heroku buildpack for Java [![Build Status](https://travis-ci.org/heroku/heroku-buildpack-java.svg)](https://travis-ci.org/heroku/heroku-buildpack-java)
=========================

云帮 Java 语言 Maven 项目的源码构建核心部分是基于[Heroku buildpack for java](https://github.com/heroku/heroku-buildpack-java) 来实现的。

## 工作原理

当buildpack在您应用代码的根目录下检测到包含`pom.xml`文件，它会被识别为Java程序。buildpack会根据您的 `pom.xml`执行构建并且下载依赖。在构建期间为了更快解决依赖关系会将`.m2`目录(本地maven库)缓存。并且`mvn`与`.m2`在您的slug文件运行时都可以使用。

## 文档

以下文章了解更多：

- [云帮支持Java](https://www.rainbond.com/docs/stable/user-lang-docs/java/lang-java-overview.html)
- [云帮部署maven应用](https://www.rainbond.com/docs/stable/user-lang-docs/java/lang-java-maven.html)

## 配置

### 选择一个JDK版本

在您应用的根目录创建 `system.properties` 文件，设置`java.runtime.version=1.7`来指定JDK版本：

```bash
    $ ls
    Procfile pom.xml src
    
    $ echo "java.runtime.version=1.7" > system.properties
    
    $ git add system.properties && git commit -m "Java 7"
    
    $ git push origin master
```
### 指定一个Maven版本

在`system.properties` 文件中您也可以设置`maven.version`来指定maven版本：

```bash
java.runtime.version=1.7
maven.version=3.1.1
```

支持的maven版本包括`3.0.5`、`3.1.1` 、`3.2.5`、`3.3.1`与 `3.3.9`。

### 自定义maven

您可以通过指定如下三个环境变量自定义maven的使用：

+ `MAVEN_CUSTOM_GOALS`: 默认值为 `clean install` 
+ `MAVEN_CUSTOM_OPTS`: 默认值为 `-DskipTests=true` 
+ `MAVEN_JAVA_OPTS`: 默认值为 `-Xmx1024m` 


设定方法如下：

[应用]-[设置]-[自定义环境变量] 添加如下变量
```bash
MAVEN_CUSTOM_GOALS="clean package"
MAVEN_CUSTOM_OPTS="--update-snapshots -DskipTests=true"
set MAVEN_JAVA_OPTS="-Xss2g"
```

授权
-------
根据 MIT 授权获得许可。 请参阅LICENSE文件
