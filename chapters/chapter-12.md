# 模块剖析----第十二章:模块剖析 #
 /*
翻译:
UmingZhang
*/ 

虽然不看看模块的内部，也可以建立相对复杂的Drupal8站点，但有可能是你需要了解一些模块的内部运作，以便充分的利用该模块提供的特性。在这一章中，我会通过带领你创建一个简单的Drupal 8 的模块，来带你在Drupal 8组成的模块中进行一次高层次的畅游。

**你的第一个Drupal的8模块**

别担心，我们将搞得很简单！虽然我们的例子模块只做一件事，但它做的非常好：在页面上显示文本“Hello Drupal 8 World!”。我们在这一节的努力的结果将是一个类似于图12-1的页面。


![Figure 12-1. Hello Drupal 8 World！](http://i.imgur.com/Sg9ZXve.png)
> 图 12-1.你好，Drupal 8世界





**第1步：创建模块目录**

第一步是创建一个目录，构成你的模块的文件将被保存在这个目录中。所有贡献模块（非核心）保存在你的Drupal8个网站根目录下的模块目录中。如果您已经安装超越Drupal8核心自带的模块，你就会看到该目录下的模块。

使用操作系统的文件管理器，或从一个终端窗口，在命令提示符下，导航到你的网站的*modules*目录，并创建一个新的命名为*custom *的目录。 Drupal最佳实践一书指出，那些不是从Drupal.org下载的自定义模块，应该存放在一个叫做*custom*的子目录中。导航到custom目录并在*custom*目录下创建一个名为*hello*一个新的目录。

**第2步：创建模块信息文件**

下一步骤是创建一个*hello.info.yml*文件。此文件告诉Drupal有关你的模块的信息，并且此文件为网站*Administration*部分的*Extend*页面提供显示信息。使用您喜欢的文本编辑器，创建一个具有以下内容的*hello.info.yml*文件：
  
    name: Hello
    type: module
    description: 'My first Drupal 8 module.'
    package: Awesome modules
    version: 1.0
    core: '8.x'

第一行规定了模型的名称，它将出现在模块页面上称。第二行指出，我们正在创建一个module模块（module）。 （主题，例如，将使用theme来作为类型的值）。第三行获取了模块的描述，它也会出现在扩展页上。包装领域提供了分组模块组合机制。例如，如果你访问你的站点的扩展页面，你会看到一个用核心的标题罗列的模块列表。我们将使用一些不重复的名字对我们的模块命名，并将其放置在一个名为*Awesome *模块的封装包中。如果你正在写一个模块，例如，创建新的Web服务功能，您应该使用创建Web服务的其他包名，以确保网站管理员可以很容易地找到你的模块。Version 是模块创建的一个版本号，而core指定了此模块适用于Drupal哪个版本？ 在我们的例子中，我们写的这个模块适用于Drupal8。

**第3步：创建模块文件**

我们的Hello模块的模块文件将做一件事情：返回一些在我们模块所提供的页面上显示的文字。模块文件可以做很多事情，但对于我们的例子中，我们目标只定位为基础。

 

使用您喜欢的文本编辑器，用以下代码创建一个名为hello.module的新文件：
    
    <?php
    use Drupal\Core\Routing\RouteMatchInterface;
    function hello_hello_world() {
    return t('Hello Drupal 8 World!');
    }



 该文件是以开放的PHP标签“<？PHP”开始的，因为所有的模块都写在PHP编程语言。我们的模块文件的特定元素是：
    
    function hello_hello_world () {

这里定义了一个PHP函数，可以从其他模块进行调用该函数。在本例中，它是一个命名hello_hello_world（）简单的函数。在我们的案例hello中，第一“hello”是模块的名字。作为一个Drupal的编码标准，所有功能应与模块的名称，后面跟着一个描述性的函数名称开头。

同样，我们的函数做一件事情：返回一个文本字符串给调用此功能的代码。在Drupal函数t（）中,我已经封装这个返回文本。如果你启用了多语言功能（见第13章），此函数可以翻译括号内（翻译注：“t（）”的括号）的任何文字。这是另一套Drupal的编码标准来包装t（）函数转换的所有文本值。

虽然我们的模块很简单，它说明了什么模块做的基本功能。模块文件是任何模块的主力，它可以简单如我们的示例模块，也可以复杂如您的模块的功能和技术要求。

**第4步：创建模块的路由文件**

 

Drupal的8的基础是Symfony，一个PHP框架，简化了复杂的基于PHP的应用程序的创建过程，如Drupal的。 Symfony框架提供了创建一个模型 - 视图 - 控制器（MVC）的应用程序的机制，其中Model代表应用程序操作的基础数据，View定义了应用程序的用户界面，控制器是应用程序的主力，包括
用户路由请求和返回信息到用户视图中。在这个过程中的下一个步骤是创建我们的模块的路由文件，它定义一个访问者将如何访问我们的模块的功能和去显示什么返回值。

在同一目录下，用你喜欢的文本编辑器创建模块的路由文件。在我们的例子中，路由文件将被命名为help.routing.yml。该文件的内容应该是

    hello.content:
    path: '/hello'
    defaults:
    _controller: '\Drupal\hello\Controller\HelloController::sayhello'
    requirements:
    _permission: 'TRUE'

代码的第一行代表我们的模块（hello）的名称。下一行表示一个终端用户将使用访问该模块提供的功能的路径，这个路径是“/hello”。defaults部分提供返回给终端用户的内容的的内容源，这是HelloController中的sayHello函数（稍后详细讨论）。requirements部分定义访问者必须拥有什么权限才能访问我们的模块;在这里我们只用TRUE这个词，因为任何人都可以访问此页面。查看模块如何限制访问的详细信息（www.drupal.org/project/examples）。


**第5步：创建模块的控制器**

 Drupal的8遵循的Symfony的和PHP 5的面向对象的方法。Drupal的8采用了一个关键关键概念是一个名为PSR-4的标准，它定义代码是如何加载到内存中。使用Drupal的早期版本的一个问题是，当许多代码不需要时，它们还是被加载到内存中。 PSR-4解决这一问题，其中一个使能器就是所谓的命名空间。在我们的路由文件中，与_content关联的值是以 \Drupal\hello\Controller开始的，\Drupal\hello\Controller是一个命名空间。 PSR-4定义了一个命名空间直接映射到应用程序的文件结构。 Symfony要求，我们所有的命名空间目录的保存在src的目录下，src驻留在我们的模块的根目录中。
 
因此，行动起来，创建控制器目录，他是我们的模块的下一个组件将保存的地方。而在我们的hello模块的根目录下，创建一个新目录命名为src，在src目录下创建一个新目录命名为Controller。在Controller目录中，我们现在已经准备好为我们的应用程序创建控制器，它是我们的应用程序的“交通警察”。在你喜欢的文本编辑器中创建一个名为HelloController.php文件。我们的控制器的内容应该是
 
 
    <?php
    namespace Drupal\hello\Controller;
    use Drupal\Core\Controller\ControllerBase;
    class HelloController extends ControllerBase {
      public function sayhello() {
    return array(
      '#markup' => hello_hello_world(),
      );
    }
      }
 
我们的控制器文件的开头用PHP开始标签开始，因为控制器是用PHP编写的。第二行定义了，我们正在使用我们的控制器的命名空间：
 
    namespace Drupal\hello\Controller;
 
第三行定义了我们要继承Drupal的核心ControllerBase类，而不必一切从头开始编写：
 
    use Drupal\Core\Controller\ControllerBase;
 
回忆第4步，在我们的模块的路由文件中，我们用'\Drupal\hello\Controller\HelloController::sayhello'调用返回显示内容的功能。类HelloController 是HelloController:: SayHello的第一部分：
 
    class HelloController extends ControllerBase {


**其他模块文件**
最后一行是从你的路由器调用的后半部分（HelloController:sayhello）。该行代码定义了当访问者访问我们的网站上/hello 地址时，返回信息到所显示的页面的功能。

    public function sayhello() {

其余的代码只需调用我们在模块文件hello_hello_world()中创建的函数。在Drupal知道如何在页面上显示的渲染阵列中，该函数返回文本“Hello Drupal 8 World!”，然后把它返回给我们的模块路由器。

    return array(
    '#markup' =hello_hello_world(),
    );

保存此文件，我们已经准备好启用我们的新模块！访问该网站管理部分的扩展（译者注：Extend ）页面，向下滚动，直到看到Awesome Modules 部分（见图12-2）。

![Figure 12-2. Our Hello Drupal 8 World! module on the Extend page](http://i.imgur.com/rDKaSoh.png)




> 图12-2。我们的Drupal 8世界！扩展页的模块

勾选模块名称旁边的方框，然后单击“保存配置”按钮来启用模块.模块启用后，您就可以测试你的第一个Drupal的8模块了！要执行模块，浏览到您的主页，并在地址的末尾添加“/hello”（在我们的模块的路由文件中，我们已经定义了这个路径）。先前在图12-1中，你就应该已经看到输出显示结果了。

**其他模块文件**

我们的Hello模块是一个很简单的模块，其目的是帮助你度过构造Drupal模块的最初学习曲线。现在你了解的非常基础，你可以看看其他贡献模块，甚至是Drupal核心，看看更复杂的模块是如何创建的，学习增加的复杂度的相关文件。在www.drupal.org/ developing/modules/8，你可以试着编写更复杂的模块。我强烈推荐从www.drupal.org/ project/examples上下载Examples模块。在这个模块中，你会发现，许多展示如何构造Drupal8模块的样板，这是创建Drupal的8模块伟大的起点。

**概要**

恭喜！你已经写了一个Drupal8模块，并获得了成为一个Drupal模块开发忍者的第一个条带。本章的目的是给你一个概述，不过有了这个腰带，你将有了探索其他模块的充足知识。虽然Drupal模块开发的学习曲线比较陡，但漫漫长路你已经踏出了第一步。
