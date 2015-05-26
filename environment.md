# Lua 运行环境  

## 本地环境搭建  

在本地搭建 Lua 编程语言的开发运行环境，你需要在你的计算机上安装如下三个软件：(1) 文本编辑器。(2)  Lua 解释器。（3）Lua 编译器。  

### 文本编辑器 
 
文本编辑器用来编辑你的程序代码。有如下几款常用的文本编辑器软件：Windows notepad、Brief、Epsilon、EMACS、vim/vi。
  
在不同的操作系统中有各自不同的编辑器，而且编辑器的版本不一样。例如，Notepad 主要用在 Windows 系统中，vim/vi 不仅可以用于 Windows 系统也可以用于 Linux 和 UNIX 操作系统。 
 
用文本编辑器编辑的文件被称为源文件。源文件中包含程序的源代码。Lua 程序的源文件经常以 .lua　作为其后缀名。
  
开始编写程序之前，请确保您已经安装好一个文本编辑软件，并且曾经有过写代码，将其存入文件，生成并执行的经验。  

### Lua 解释器  

Lua 解释器是一个能让您输入 Lua　命令并立即执行的小程序。它在执行一个　Lua 文件过程中，一旦遇到错误就立即停止执行，而不像编译器会执行完整个文件。  

### Lua 编译器  

如果将 Lua 扩展到其它语言或者应用中时，我们需要一个软件开发工具箱以及 Lua 应用程序接口兼容的编译器。  

## 在 Windows 系统安装 Lua  

在 Windows 系统环境可以安装一个叫 SciTE 的 Lua 开发 IDE (集成开发环境)。它可以在这儿下载：<a href="http://code.google.com/p/luaforwindows/">http://code.google.com/p/luaforwindows</a>。
  
运行下载的可执行程序就可安装 Lua 语言的 IDE 了。  

在这个 IDE 上，你可以创建并生成 Lua 代码。
  
如果你希望在命令行模式下安装 Lua，你则需要安装 MinGW 或者 Cygwin，然后在 Ｗindows 系统中编译安装 Lua。  

## 在 Linux 系统安装 Lua  

使用下面的命令下载并生成 Lua 程序：
  
```
$ wget http://www.lua.org/ftp/lua-5.2.3.tar.gz
$ tar zxf lua-5.2.3.tar.gz
$ cd lua-5.2.3
$ make linux test
```  

在其它系统上安装 Lua 时，比如 aix，ansi，bsd，generic，linux，mingw，posix，solaris，你需要将 make linux test 命令中的 linux 替换为相应的系统平台名称。
 
假设我们已经有一个文件 helloWord.lua ，文件内容如下：  
 
```
print("Hello World!")
```  

我们先使用 cd 命令切换至 helloWord.lua　文件所在的目录，然后生成并运行该文件：　　

```
$ lua helloWorld
```  

执行上面的命令，我们可以看到如下的输出：  

```
hello world
```  

## 在 Mac OS X 系统安装 Lua  

使用下面的命令可以在 Mac OS X 系统生成并测试 Lua：  

```
$ curl -R -O http://www.lua.org/ftp/lua-5.2.3.tar.gz
$ tar zxf lua-5.2.3.tar.gz
$ cd lua-5.2.3
$ make macosx test
```  

如果你没有安装 Xcode 和命令行工具，那么你就不能使用 make 命令。你先需要从 mac 应用商店安装 Xcode，然后在 Xcode 首选项的下载选项中安装命令行工具组件。完成上面的步骤后，你就可以使用 make 命令了。  
make macosx test 命令并不是非执行不可的。即使你没有执行这个命令，你仍可以在你的 Mac OS X 系统中使用 Lua。
  
假设我们已经有一个文件 helloWord.lua ，文件内容如下：  

```
print("Hello World!")
```  

我们先使用 cd 命令切换至 helloWord.lua　文件所在的目录，然后生成并运行该文件：　　

```
$ lua helloWorld
```  

执行上面的命令，我们可以看到如下的输出：  

```
hello world
```  

## Lua IDE  

正如前面提到的那样，Windows 系统中 SciTE 是 Lua 创始团队提供的默认的 Lua 集成开发环境（IDE)。 此外，还有一款名叫 ZeroBrane 的 IDE。 它具有跨平台的特性，支持 Windows、Mac 与 Linux。  
同时，许多 eclipse 插件使得 eclipse 能成为 Lua 的 IDE。IDE 中像代码自动补全等诸多特性使得开发变得简单了很多，因此建议你使用 IDE 开发 Lua 程序。同样，IDE 也能像 Lua 命令行版本那样提供交互式编程功能。