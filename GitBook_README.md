
# GitBook安装使用记录

### 安装 GitBook

```
npm install gitbook-cli -g

gitbook -V
```

###  Gitbook文档目录结构

```
GitBook 基本的目录结构如下所示
|- book.json		//电子书的配置文件
|- README.md		//电子书的主要说明文件
|- SUMMARY.md		//电子书的目录
|- chapter-1/		//电子书的章节1文件夹(chapter-1是文件夹名称，可以自定义)
	|- README.md	    //章节1的说明文件
 	|- 文档1.md		//章节下面的小节1
    |- 文档2.md		//章节下面的小节2
|- chapter-2/		//电子书的章节2文件夹(chapter-2是文件夹名称，可以自定义)
	|- README.md	    //章节2的说明文件
 	|- 文档1.md		//章节下面的小节2
    |- 文档2.md		//章节下面的小节2
```

###  Gitbook初始化

```
gitbook init
```

###  编写目录

```
# Summary
* [教程导读](README.md)
* [day01—环境搭建&快速入门](day01—Java开发环境/README.md)
    * [环境搭建](day01—Java开发环境/环境搭建.md)
    * [入门案例](day01—Java开发环境/基础语法.md)
    * [基础语法](day01—Java开发环境/入门案例.md)
    * [课后练习](day01—Java开发环境/课后练习.md)
* [day02—类型转换&运算符](day02—类型转换&运算符/README.md)
    * [类型转换](day02—类型转换&运算符/类型转换.md)
    * [运算符](day02—类型转换&运算符/运算符.md)
    * [if语句](day02—类型转换&运算符/if语句.md)
```

###  生成各小节md文件

```
gitbook init
```

###  编译生成静态网页

```
gitbook build
```
### 编译并预览静态网页

```
gitbook serve
```

### Gitbook配置文件

Gitbook有一个配置文件book.json，在该配置文件中可以配置各种插件


