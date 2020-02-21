# GO的工程文件

## 工作区和GOPATH
在下载安装包之后，安装成功`GO`语言的时候，我们需要来配置三个环境变量：
+ <font color=#3eaf7c>GOROOT</font>:`Go`语言安装根目录的路径，也就是`Go`语言的安装路径
+ <font color=#3eaf7c>GOPATH</font>:若干工作区目录的路径，就是我们自己定义的工作空间
+ <font color=#3eaf7c>GOBIN</font>:`GO`程序生成可执行文件的路径

现在我们就有一个非常重要的面试问题：<font color=#DD1144>设置GOPATH有什么意义</font>？

答：<font color=#1E90FF>可以简单的将GOPATH理解成GO语言的工作目录，它的值是一个目录的路径，也可以是多个目录的路径，每个目录都代表GO语言的一个工作区</font>

我们需要利用这些工作区，去放置`Go`语言的<font color=#DD1144>源码文件</font>（source file），以及安装后的<font color=#DD1144>归档文件</font>（archive file，即以`.a`为扩展名的文件）和<font color=#DD1144>可执行文件</font>（executable file），关于`GOPATH`和工作区,我们需要知道下面三个知识点:

### 1. GO源码的组成方式
+ `Go`语言的源码也是以代码包为基本组织单位的。在文件系统中，这些代码包其实是与目录一一对应的。由于目录可以有子目录，所以代码包也可以有子包。

+ 一个代码包中可以包含任意个以`.go`为扩展名的源码文件，这些源码文件都需要被声明属于同一个代码包。代码包的名称一般会与源码文件所在的目录同名。如果不同名，那么在构建、安装的过程中会以代码包名称为准.

+ 每个代码包都会有导入路径。代码包的导入路径是其他代码在使用该包中的程序实体时，需要引入的路径。在实际使用程序实体之前，我们必须使用`import`先导入其所在的代码包.

+ <font color=#DD1144>在工作区中，一个代码包的导入路径实际上就是从src子目录，到该包的实际存储位置的相对路径。（参照第二小节（源码安装后的结果）中的那张图）</font>

### 2. 源码安装后的结果
+ 我们都知道，源码文件通常会被放在某个工作区的`src`子目录下。那么在安装后如果产生了归档文件（以`.a`为扩展名的文件），就会放进该工作区的 `pkg`子目录；如果产生了可执行文件，就可能会放进该工作区的`bin`子目录。

+ 比如一个代码包的源码是放在：`GOPATH/工作区1/src/github.com/labstack/echo`中，则它安装的时候产生的归档文件的路径就是：`GOPATH/工作区1/pkg/linux_adm64/github.com/labstack/echo.a`

+ 归档文件的相对目录与`pkg`目录之间还有一级目录，叫做平台相关目录。平台相关目录的名称是由`build`（也称“构建”）的目标操作系统、下划线和目标计算架构的代号组成的。比如，构建某个代码包时的目标操作系统是`Linux`，目标计算架构是64位的，那么对应的平台相关目录就是   `linux_amd64`。

<img :src="$withBase('/gocore_one_gopath.png')" alt="工作区和GOPATH">

### 3. 构建和安装的过程
构建使用命令`go build`，安装使用命令`go install`。构建和安装代码包的时候都会执行编译、打包等操作，并且，这些操作生成的任何文件都会先被保存到某个临时的目录中。

+ 如果构建（`go build`）的是库源码文件，那么操作后产生的结果文件只会存在于临时目录中。这里的构建的主要意义在于检查和验证（或者说对第三方的包代码进行校验）。

+ 安装（`go install`）操作会先执行构建，然后还会进行链接操作，并且把结果文件搬运到指定目录。
  + 进一步说，如果安装的是库源码文件，那么结果文件（实际上就是归档文件）会被搬运到它所在工作区的`pkg `目录下的某个子目录中。
  + 如果安装的是命令源码文件，那么结果文件会被搬运到它所在工作区的 `bin`目录中，或者环境变量GOBIN指向的目录中。

## 命令源码文件
如果说到<font color=#DD1144>命令源码文件</font>，你可能会一脸蒙蔽，不知道是什么东西，但是说到`main`函数，你大概不会陌生，实际上命令源码文件的定义如下：
+ <font color=#1E90FF>命令源码文件是程序的运行入口，是每个可独立运行的程序必须拥有的。我们可以通过构建或安装，生成与其对应的可执行文件，后者一般会与该命令源码文件的直接父目录同名。</font>
+ <font color=#DD1144>如果一个源码文件声明属于main包，并且包含一个无参数声明且无结果声明的main函数，那么它就是命令源码文件</font>


## 库源码文件


**参考资料**

1. [Go语言核心36讲](https://time.geekbang.org/column/intro/112)
2. [Go模块简明教程(Go语言依赖包管理工具)](https://segmentfault.com/a/1190000016146377)