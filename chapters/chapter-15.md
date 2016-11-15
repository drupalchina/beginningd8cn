#第十五章 使用Drush#

**译者：蓝眼泪**

>本章提供了一个Drush的基本概述，它是一个简化了创建和管理Drupal8网站的命令行工具。使用Drush的时候，以前往往需要你登陆到网站上，导航到网站管理页面，填写一个表单的任务，现在可以通过简单的命令行界面进行了。Drush可以使你不用登录每一个服务器和网站，实现远程管理一个或多个站点来执行日常维护的任务。Drush主要负责管理模块、主题、用户配置文件和常见的任务管理，如运行cron，创建备份和清理缓存。你甚至可以使用Drush执行SQL命令。

##安装Drush##
Drush是在其核心的一套shell脚本（Unix/Linux）或bat脚本（Windows），结合PHP脚本处理大部分管理Drupal网站的任务。安装脚本和PHP在Unix/Linux和Windows上都是一个相对简单的过程。

##在Unix，Linux或OS X上安装Drush##
在Unix，Linux或OS X上安装Drush，要遵循一下步骤：

1. 从 https://github.com/drush-ops/drush 上下载Drush。在Github上Drush所在页面右侧你会看到一下Download Zip的按钮，如果你熟悉Git（见第16章），你可能想要克隆Git仓库以方便管理Drush的更新。有关克隆仓库的信息，请参见第16章。
2. 将压缩文件drush.zip放在网站根目录外的目录下。在大多数web服务器上，网站根目录是像public_html或www_root或docroot这样的目录。查看你根目录下的web服务器详细说明文档。为了安全起见，你可能不想把它放在web根目录下，因为它将可被任何人使用和执行。
3. 执行以下命令，使drush命令可执行  
`$ chmod u+x /path/to/drush/drush`
4. 配置系统使他能够识别Drush。有三种方式：  
	a. 创建一个符号链接到已存在一路径中的Drush可执行目录中，例如：  
	`$ ln -s /path/to/drush/drush /usr/bin/drush`
	
	b. 明确地将Drush可执行的添加到系统变量中，它是在shell配置文件中定义的，名称为：.profile，.bash_profile，.bash_aliases，或.bashrc,位于你的主文件夹下。例如：  
	`export PATH="$PATH:/path/to/drush:/usr/local/bin"`  
	
	你的系统将从左到右搜索路径选项，直到找到结果。
	  
	c. 增加Drush别名：如果你想使用两种版本的Drush这个方法也是很方便的，如Drupal7开发使用Drush5或6（稳定版），Drupal8开发使用Drush7（主分支）。向你的Drush7可执行的添加一个别名，把它添加到你的shell配置文件里（见前面的选项列表）：  
	`$ alias drush-master=/path/to/drush/drush` 
	 
	要在你的当前会话中应用选项b和c，可以登出并重新登录或使用下边的命令重新加载bash配置文件：  
	`$ source .bashrc`

___

注意：如果你不走这一步，你将需要（不方便的）使用完整路径在可执行文件下运行Drush命令，`/pash/to/drush/drush`，或导航到 `/path/to/drush` 然后运行 `./drush`。将需要`-r`或`-l`选项（见下方的使用）。

___

5.测试系统可以找到Drush：  
	`$ which drush`

6.在Drush根目录下，运行Composer获取依赖关系：  
	`$ composer install`

要查看Composer的更多信息，请访问 https://getcomposer.org/doc/00-intro.md

##在Windows上安装Drush##
要在Windows上安装Drush，请访问 https://www.drupal.org/node/594744 ,该页面上有如何在Windows上安装Drush的最新介绍。对于不同Windows版本和不同Drush版本来说安装方法是不同的。

##Drush命令##
执行Drush命令是相对简单和直接的。从允许你访问操作系统命令行的终端窗口或其他工具导航到你Drupal网站的根目录下，尝试一下Drush，在命令提示符下输入drush status并点击回车，Drush返回的内容应该像下面这样：  

```
Drupal version : 8.0-dev  
Site URI : http://default  
Database driver : mysql  
Database username : d8  
Database name : d8  
Database : Connected  
Drupal bootstrap : Successful  
Drupal user : Anonymous  
Default theme : bartik  
Administration : seven  
theme  
PHP executable : /usr/bin/php  
PHP configuration :  
PHP OS : Darwin  
Drush version : 7.0-dev  
Drush configuration :  
Drush alias files :  
Drupal root : /Applications/MAMP/htdocs/d8  
Site path : sites/default  
File directory path : sites/default/files  
Temporary file : /Applications/MAMP/tmp/php  
directory path  
Active config path : sites/default/files/config_lFEpXnCuJgRcTN0kwUlVqp7FAr  
JU7Cj4s2AbZN9Fg6W22BksQHWkPzhJGqU4cfs0Ix2UD7YEqg/active  
Staging config path : sites/default/files/config_lFEpXnCuJgRcTN0kwUlVqp7FAr  
JU7Cj4s2AbZN9Fg6W22BksQHWkPzhJGqU4cfs0Ix2UD7YEqg/staging
```

status命令返回一个有关你当前Drupal8安装信息的有用的列表，另一个重要例子是使用`drush pm-download`命令行下载一个模块或主题：  
`drush pm-download calendar`  
这个命令将在你的网站上下载日历模块的最新生产版本，还有一种缩短版的命令行`drush dl calendar`.
要在命令行启动日历模块（例子），你需要执行  
`drush pm-enable calendar  `
或在短版本里执行：`drush en calendar`

要禁用日历模块，使用  
`drush pm-disable calendar ` 
或使用短版本，执行：`drush pm calendar`

如果日历模块的新版本或安全补丁已发布，而你想要应用这个更新，那么请运行  
`drush pm-update calendar  `
就像你看到的，通过简单快捷的命令行工具可以完成需要通过在管理界面几次点击才能完成的工作。

表15-1提供了公共Drush命令的完整列表。如需更完整的介绍请访问[http://drush.org](http://drush.org)

表15-1 Drush核心命令
  
<table style="border-collapse:collapse;"><tr><th style="width:20%;">命令</th><th style="width:80%;">描述</th></tr><tr><td>archive-dump</td><td>备份代码、文件、数据库到一个文件中</td></tr><tr><td>archive-restore</td><td>扩展站点压缩到一个Drupal网站</td></tr><tr><td>cache-clear</td><td>清除特定的缓存，或者所有Drupal缓存</td></tr><tr><td>cache-get</td><td>获取一个缓存对象并将它显示出来</td></tr><tr><td>cache-set</td><td>将一个对象缓存为JSON或var_export()格式</td></tr><tr><td>core-config</td><td>编辑drushrc，网站别名，和drupal的settings.php文件</td></tr><tr><td>core-cron</td><td>在指定站点的所有有效模块中运行cron</td></tr><tr><td>core-execute</td><td>执行一个shell命令，通常会使用网站别名</td></tr><tr><td>core-quick-drupal</td><td>以最小的配置和依赖下载、安装、服务、登录到Drupal</td></tr><tr><td>core-requirements</td><td>如果有的话，会提供你Drupal安装时的错误信息</td></tr><tr><td>core-rsync</td><td>使用SSH远程同步Drupal与另一台服务器</td></tr><tr><td>core-status</td><td>如果有的话，提供当前Drupal安装的鸟瞰图</td></tr><tr><td>core-topic</td><td>在一个给定的主题上阅读详细文档</td></tr><tr><td>drupal-directory</td><td>返回到一个给定模块/主题目录的路径</td></tr><tr><td>help</td><td>打印此帮助消息，查看更多Drush帮助信息</td></tr><tr><td>image-flush</td><td>刷新所有导出图像为给定的风格</td></tr><tr><td>php-eval</td><td>在引导Drupal之后评估任意PHP代码（如果可用）</td></tr><tr><td>php-script</td><td>运行PHP脚本</td></tr><tr><td>queue-list</td><td>返回所有定义的队列列表</td></tr><tr><td>queue-run</td><td>按名称运行一个特定的队列</td></tr><tr><td>search-index</td><td>索引其余的搜索项目，而不清除索引</td></tr><tr><td>search-reindex</td><td>强制搜索索引重建</td></tr><tr><td>search-status</td><td>显示总共仍有多少项目待索引</td></tr><tr><td>self-update</td><td>检查是否有更新的Drush版本中提供</td></tr><tr><td>shell-alias</td><td>打印所有已知的shell别名记录</td></tr><tr><td>site-alias</td><td>打印所有网站别名和本地站点的别名记录</td></tr><tr><td>site-install</td><td>使用指定的安装配置文件与模块/主题/配置一起安装Drupal</td></tr><tr><td>site-reset</td><td>重置持续站点设置</td></tr><tr><td>site-set</td><td>为当前会话中持续的工作设置一个站点别名</td></tr><tr><td>site-ssh</td><td>通过SSH的交互式会话或运行一个shell命令连接到Drupal网站服务器</td></tr><tr><td>test-clean</td><td>清除临时表和文件</td></tr><tr><td>test-run</td><td>运行测试。注意，您必须使用--uri选项</td></tr><tr><td>updatedb</td><td>应用所需要的任何数据库更新（如运行update.php）</td></tr><tr><td>usage-send</td><td>向统计记录站点发送匿名Drush使用情况，使用统计包括Drush命令名称及Drush选项名称，但不包括参数或选项的值。</td></tr><tr><td>usage-show</td><td>显示Drush已记录但尚未发送的使用信息。使用统计包括Drush命令名称及Drush选项名称，但不包括参数或选项的值</td></tr><tr><td>variable-delete</td><td>删除一个变量</td></tr><tr><td>variable-get</td><td>获取部分或全部网站的变量和值的列表</td></tr><tr><td>version</td><td>显示Drush版本</td></tr><tr><td>watchdog-delete</td><td>删除watchdog消息</td></tr><tr><td>watchdog-list</td><td>显示可用的消息类型和严重程度。一个提示会要求选择显示watchdog消息。</td></tr><tr><td>watchdog-show</td><td>显示watchdog信息</td></tr></table>

表15-2 字段命令
  
<table style="border-collapse:collapse;"><tr><th style="width:20%;">命令</th><th style="width:80%;">描述</th></tr><tr><td>field-clone</td><td>克隆一个字段及其所有实例</td></tr><tr><td>field-create</td><td>创建字段和实例。返回字段编辑链接</td></tr><tr><td>field-delete</td><td>删除字段及其实例</td></tr><tr><td>field-info</td><td>查看有关字段，字段类型，和小部件的信息</td></tr><tr><td>field-update</td><td>返回字段编辑网页的链接</td></tr></table>

表15-3 项目管理的命令

<table style="border-collapse:collapse;"><tr><th style="width:20%;">命令</th><th style="width:80%;">描述</th></tr><tr><td>pm-disable</td><td>禁用一个或多个扩展（模块和主题）</td></tr><tr><td>pm-download</td><td>从Drupal.org或其他来源下载项目</td></tr><tr><td>pm-enable</td><td>启用一个或多个扩展（模块和主题）</td></tr><tr><td>pm-info</td><td>显示一个或多个扩展（模块和主题）的详细信息</td></tr><tr><td>pm-list</td><td>显示可用扩展（模块和主题）的列表</td></tr><tr><td>pm-refresh</td><td>刷新更新状态信息</td></tr><tr><td>pm-releasenotes</td><td>打印给定项目的发行说明</td></tr><tr><td>pm-releases</td><td>打印给定项目的发布信息</td></tr><tr><td>pm-uninstall</td><td>卸载一个或多个模块</td></tr><tr><td>pm-update</td><td>更新Drupal核心和贡献的项目以及应用任何挂起的数据库更新（和PM-更新代码+数据库更新一样）</td></tr><tr><td>pm-updatecode</td><td>更新Drupal核心和贡献的项目到最新推荐的版本</td></tr></table>

表15-4 SQL命令

<table style="border-collapse:collapse;"><tr><th style="width:20%;">命令</th><th style="width:80%;">描述</th></tr><tr><td>sql-cli</td><td>使用Drupal的凭据打开SQL命令行界面</td></tr><tr><td>sql-connect</td><td>一个连接到数据库的字符串</td></tr><tr><td>sql-create</td><td>创建数据库</td></tr><tr><td>sql-drop</td><td>删除一个给定的数据库中的所有表</td></tr><tr><td>sql-dump</td><td>使用mysqldump或等效的操作导出Drupal数据库为SQL</td></tr><tr><td>sql-query</td><td>执行对站点数据库的查询</td></tr><tr><td>sql-sync</td><td>复制和导入源数据库到目标数据库。通过rsync进行转移</td></tr></table>

表15-5 用户命令 

<table style="border-collapse:collapse;"><tr><th style="width:20%;">命令</th><th style="width:80%;">描述</th></tr><tr><td>user-add-role</td><td>添加角色到指定的用户帐户</td></tr><tr><td>user-block</td><td>阻止指定的用户（S）</td></tr><tr><td>user-cancel</td><td>取消具有指定名称的用户帐户</td></tr><tr><td>user-create</td><td>创建具有指定名称的用户帐户</td></tr><tr><td>user-information</td><td>打印关于指定用户（S）的信息</td></tr><tr><td>user-login</td><td>向指定用户帐户显示一个一次性登录链接（默认为ID1）</td></tr><tr><td>user-password</td><td>为具有指定名称的用户账户设置或重置密码</td></tr><tr><td>user-remove-role</td><td>移除指定用户帐户的角色</td></tr><tr><td>user-unblock</td><td>解锁指定的用户</td></tr></table>

##本章小结

Drush向那些喜欢命令行操作的用户提供了一个快速且容易的方法来管理网站。还有很多更高级的Drush功能，如编写Drush脚本同时执行多个任务。欲了解更多信息，请访问http://drush.org 。

接下来，第16章介绍了如何使用Git来管理Drupal源代码。如果你喜欢Drush，你一定会喜欢的Git！