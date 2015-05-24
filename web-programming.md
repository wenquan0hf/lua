#Lua——Web 编程  

Lua 是一种非常灵活的语言，它经常被用大各种平台上，包括 web 应用。Kepler 社区成立于 2004 年，一直致力于为 Lua 提供开源的 web 组件。  
尽管已经推出了许多的 Lua web 应用框架，但是我们还是想主要介绍一下由 Kepler 社区开发的 web 开发组件。  

##应用与框架  

<ul>
	<li>Orbit 是一个基于 WSAPI 的 MVC web 框架（译注： MVC，模型-视图-控制器）。</li>
	<li>WSAPI 是一套从 Lua web 应用中抽象出来的 web 服务 API，它是许多其它项目的基础。</li>
	<li>Xavante 是一个提供 WSAPI 的 Lua Web 服务器。</li>
	<li>Sputnik 是 Kepler 项目中开发的 wiki/CMS 构架，它也基于 WSAPI。</li>
	<li>CGILua 支持 LuaPages 和 LuaScripts 网络页面的创建，基于 WSAPI，不过已经不再提供支持。</li>
</ul>  

在本教程中，我们会让你了解到在 Web 应用开发中 Lua 可以完成哪些工作。了解更多安装和使用说明，可以参阅<a href="http://www.keplerproject.org/">kepler website</a>  

###Orbit  

Orbit 是一个 MVC 类型的 Lua web 框架。它完全抛弃了 CGILua 的脚本即应用的模型，在此模型中每个 Orbit 应用都可以放在一个文件中，如果你愿意，每个应用也可以被分割在多个文件到。  
所有的 Orbit 应用都支持 WSAPI 协议，所以它们也就兼容 Xavante, CGI 和 Fastcgi。它自带了一个启动器，以启动一个 Xavante 实例便于开发。  
安装 Orbit 最简单的方式是使用 LuaRocks。 luarocks 用命令行的方式安装 orbit。因此，首先你需要安装<a href="http://luarocks.org/en/Download">luaRocks</a>。  
如果你没安装所需的依赖，下面的步骤引导你在 Unix/Linux 环境下搭建 Orbit 环境。  

####安装 Apache  

连接服务器，安装 Apache2。  

```
$ sudo apt-get install apache2 libapache2-mod-fcgid libfcgi-dev build-essential
$ sudo a2enmod rewrite
$ sudo a2enmod fcgid
$ sudo /etc/init.d/apache2 force-reload
```  

####安装 LuaRocks  

```
$ sudo apt-get install luarocks
```  

####安装 WSAPI，FCGI，Orbit，Xavante  

```
$ sudo luarocks install orbit
$ sudo luarocks install wsapi-xavante
$ sudo luarocks install wsapi-fcgi
```  

####配置 Apache2  

```
$ sudo raj /etc/apache2/sites-available/default
```  

在配置文件的 <Directory /var/www/> 节中增如下的节。如果下面节中有 AllowOverride None， 你需将 None 改为 All，如此 .htaccess 才能覆盖本地配置。  

```
<IfModule mod_fcgid.c>
    AddHandler fcgid-script .lua
    AddHandler fcgid-script .ws
    AddHandler fcgid-script .op
    FCGIWrapper "/usr/local/bin/wsapi.fcgi" .ws
    FCGIWrapper "/usr/local/bin/wsapi.fcgi" .lua
    FCGIWrapper "/usr/local/bin/op.fcgi" .op
    #FCGIServer "/usr/local/bin/wsapi.fcgi" -idle-timeout 60 -processes 1
    #IdleTimeout 60
    #ProcessLifeTime 60
</IfModule>
```  

重启服务器使得配置更改生效。  
为了使你的应用可以运行，你需要在你的 Orbit 应用根目录下的 .htaccess 文件中添加 +ExecCGI，在本例中根目录为 /var/www。  

```
Options +ExecCGI
DirectoryIndex index.ws
```  

####示例——Orbit  

```
#!/usr/bin/env index.lua
-- index.lua
require"orbit"

-- declaration
 module("myorbit", package.seeall, orbit.new)

-- handler
function index(web)
  return my_home_page()
end

-- dispatch
myorbit:dispatch_get(index, "/", "/index")

-- Sample page
function my_home_page()
   return [[
    <head></head>
    <html>
    <h2>First Page</h2>
    </html>
    ]]
end
```  

现在，你可以启动你的浏览器访问 http://localhost:8080 就可以看到下面的结果：  

```
First Page
```  

Orbit 还提供了另外选项，该选项使得 Lua　代码可以生成 html。  

```
#!/usr/bin/env index.lua
-- index.lua
require"orbit"

function generate()
    return html {
        head{title "HTML Example"},
        body{
            h2{"Here we go again!"}
        }
    }
end

orbit.htmlify(generate)

print(generate())
```  

####创建表单  

简单的表单创建代码如下：  

```
#!/usr/bin/env index.lua
require"orbit"

function wrap (inner)
    return html{ head(), body(inner) }
end

function test ()
    return wrap(form (H'table' {
        tr{td"First name",td( input{type='text', name='first'})},
        tr{td"Second name",td(input{type='text', name='second'})},
        tr{ td(input{type='submit', value='Submit!'}),
            td(input{type='submit',value='Cancel'})
        },
    }))
end

orbit.htmlify(wrap,test)

print(test())
```  

你可以在<a href="http://keplerproject.github.io/orbit/example.html">官网</a>找到 orbit 的详细教程。  

###WSPAI  

正如前面所说的，WSAPI 是大多数项目的基础，它内嵌了大量的特性。你现在可以在下面的这些系统平台上使用 WSAPI：  
<ul>
	<li>Windows</li>
	<li>UNNIX 类系统</li>
</ul>

WSAPI 支持的服务器和接口包括：  

<ul>
	<li>CGI</li>
	<li>FastCGI</li>
	<li>Xavante</li>
</ul>

WSAPI 提供了大量库方便我们使用 Lua 进行 Web 应用的开发。下面列出了其运行的部分特征：  
<ul>
	<li>请求处理</li>
	<li>输出缓存</li>
	<li>认证机制</li>
	<li>文件上传</li>
	<li>请求隔离</li>
	<li>复用</li>
</ul>
下面是 WSAPI 的一个简单例子。  

```
 #!/usr/bin/env wsapi.cgi

module(..., package.seeall)
function run(wsapi_env)
   local headers = { ["Content-type"] = "text/html" }
   
   local function hello_text()
      coroutine.yield("<html><body>")
      coroutine.yield("<p>Hello Wsapi!</p>")
      coroutine.yield("<p>PATH_INFO: " .. wsapi_env.PATH_INFO .. "</p>")
      coroutine.yield("<p>SCRIPT_NAME: " .. wsapi_env.SCRIPT_NAME .. "</p>")
      coroutine.yield("</body></html>")
   end

   return 200, headers, coroutine.wrap(hello_text)
end
```  

很容易看出来，上面的代码生成了一个简单的 html 页面。同时，你可以看到使用协程可以将 html 语句一条一条的返回给调用函数。最终返回的是 html 状态码（200）、头部以及 html 页面。  

####Xavante  

Xavante 是一款支持 HTTP 1.1 的 Lua web 服务器。它采用模块化的结构设计，使用 URI 映射处理程序的方式进行路由。 Xavante 目前支持：  

<ul>
	<li>文件处理程序</li>
	<li>重定向处理程序</li>
	<li>WSAPI 处理程序</li>

</ul>  
文件处理程序用于一般文件；重定向处理程序实现 URI 重映射；WSAPI 处理程序用于 WSAPI 应用。  
使用示例如下：  

```
require "xavante.filehandler"
require "xavante.cgiluahandler"
require "xavante.redirecthandler"

-- Define here where Xavante HTTP documents scripts are located
local webDir = XAVANTE_WEB

local simplerules = {

    { -- URI remapping example
      match = "^[^%./]*/$",
      with = xavante.redirecthandler,
      params = {"index.lp"}
    }, 

    { -- cgiluahandler example
      match = {"%.lp$", "%.lp/.*$", "%.lua$", "%.lua/.*$" },
      with = xavante.cgiluahandler.makeHandler (webDir)
    },
    
    { -- filehandler example
      match = ".",
      with = xavante.filehandler,
      params = {baseDir = webDir}
    },
} 

xavante.HTTP{
    server = {host = "*", port = 8080},
    
    defaultHost = {
    	rules = simplerules
    },
}
```  

如果使用 Xavante 虚拟机，xavante.HTTP 需要被修改为如下内容：  

```
xavante.HTTP{
    server = {host = "*", port = 8080},
    
    defaultHost = {},
    
    virtualhosts = {
        ["www.sitename.com"] = simplerules
    }
}
```  

##Lua web 组件  

<ul>
	<li>Copas：基于协程的分配器，可用于 TCP/IP 服务器。</li>
	<li>Cosmo：一个安全模板引擎，保护应用免受来自模板的任何代码攻击。</li>
	<li>Coxpcall：封装 Lua 原生的 pcall 和 xpcall　函数，提供协程兼容的版本。</li>
	<li>LuaFileSystem：以可移植的方式访问底层目录结构和文件属性。</li>
	<li>Rings:提供在 Lua　中创建新的　Lua state 的方法。</li>
</ul>

##结束语  

根据我们的需求，我们可以找到很多适合我们的 Lua web　框架和组件。下面列出了另外一些可用的框架：　　

<ul>
	<li>Moontalk：提供高效的开发方式，为 Lua　开发的动态 web 应用提供容器，适于各种复杂程度的应用开发。</li>
	<li>Lapis：使用 MoonScript(或 Lua)的 web 应用框架，它可以运行在 OpenResty 服务器中。</li>
	<li>Lua Server Pages：一个 Lua 脚本引擎插件，此插件提供了完善的嵌入 web 开发方案。</li>
</ul>

充分利用这些 Web 框架，它可以帮助你实现更加丰富的 web 功能。