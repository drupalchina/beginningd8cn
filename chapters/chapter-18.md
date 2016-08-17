# 第十八章 响应式网站#

翻译：蹊

就在几年前，智能手机和平板电脑并不存在，如今我们生活在一个分析家预测下的手机和平板电脑的网站流量于2014年超过笔记本和台式电脑的世界里。我们生活在一个移动端的世界，作为Drupal网站建设者，重要的是要处理日益增长的智能手机和平板电脑的使用，在我们建立新站点的时候，要开始优先考虑移动端。 如果大多数的用户访问我们的网站是在较小的屏幕上，无论他们使用什么设备，都能提供了不起的用户体验是很明智的。 		

幸运的是，Drupal 8 能处理在不同宽度屏幕上渲染站点的挑战（手机，phablets，平板电脑，笔记本电脑，台式电脑，电视）在这一章里，我们将着眼于利用Drupal 8响应式工具和响应式主题，以及步入基本的CSS主题处理响应性。
 
##定义响应式
 
你听说过“响应式”和“移动友好度”，但是网站响应式到底意味着什么？简单的说，响应式意味着你的网站在不同宽度屏幕的多种设备上可以浏览和使用，而无须滚动很远，在小型设备上阅读，内容依旧大小合适。

响应式也意味着，你的用户访问网站时，用户体验自动适应设备。例如,如果你的站点的某些元素在小型设备上显示不佳，你可以选择访问者使用小型设备时不显示这些元素，但是访问者使用笔记本和台式机时仍可以显示。响应式也可能意味着利用设备的功能，如手势或滑动浏览你的网站，而不是要求更多传统的指向-点击的界面。无论你选择哪种定义，响应式设计的基本概念是使你的站点在所有设备均可用。

##如何设计响应式网页
 
响应网页设计作品通过结合检测网站访问者使用的设备的屏幕宽度，利用CSS定义元素基于屏幕的宽度如何在页面上显示。比如一个页面,该页面有三个等宽列，每个占屏幕宽度的33% 。如果你在配有15英寸屏幕1680像素的笔记本电脑的页面浏览，每列宽约560像素则该网站会看起来不错（见图18-1）。

![图18-1 普通笔记本的屏幕分辨率](../images/pic-18-1.png) 

**图18-1. 普通笔记本的屏幕分辨率**

打开iPhone上的同一页，列缩小到320像素的33%，约106像素。列变得狭长，列中的文本变得看不到——最好的是浏览者必须滚动到左边或右边，才能看到非常小的文本列和图片（见图18-2）.

![图18-2 普通iphone4的屏幕分辨率](../images/pic-18-2.png) 

**图18-2. 普通iphone4的屏幕分辨率**

更好的方法是检测到用户正在用320像素宽的设备浏览，用每列320像素或100%宽度取代水平三列横跨页面，各列堆积在顶部。就像在15英寸的笔记本电脑上看一样，每列几乎是一样宽，在iPhone4上依然可以看到（见图18-3）。

![图18-3 列堆积](../images/pic-18-3.png) 

**图18-3. 列堆积**

Media Queries 是一个CSS工具，结合浏览器报告屏幕的宽度，让你专门为给定宽度的设备写css。 media query 的一般格式是：

@media (max-width: 600px) {

.body {

font-size: .9em;

}

}

query 始于@media和最大宽度：600px的指定范围内，{}里面的css只能在600px或更小的浏览器窗口被应用。你也可以指定你查询的宽度范围：

@media (min-width: 321px) and (max-width: 599px) {
 
.body {

font-size: .7em;

}

}

在这种情况下，任何设备的屏幕宽大于等于321px和小于等于599px之间css会包含{}的应用。在前面的两个例子中，设备上的任何文本在600px宽以内字体有 .9em，设备在321px到599px宽会有 .7em，（解释em的单元将在之后章节“普通css变成响应式”这一段提到）。

当你看到市场的各种设备，你会发现你可能需要在你的css文件里用几个 media queries 去解决你的站点在不同设备上的渲染。 media queries 的个数取决于你的页面在屏幕上缩放的时候，内容的格式出现多少变化。如果你的站点是单栏设计，那么你能够侥幸不使用 media queries，但是多数站点不止一栏，甚至单栏站点也需要为横向菜单做出调整。
 
##制作你的响应式站点

制作你的Drupal8 响应式站点有三个基本选择：
 
-	从 Drupal.org 选择一个现成的主题，含有 media queries 定义的跨设备工作的主题样式表和css。
-	选择一个初始主题，比如，从 Drupal.org 下载Zen这个主题，在该主题的响应式框架上构建。　　
-	采用无响应式的主题，添加 media queries 和 width-specific CSS，使得站点在任何宽度都能渲染正常。

每种方法都有其优点，取决于您的CSS的合适程度和你识别页面上的元素需要做出改变的能力，这种改变基于访问站点的游客的屏幕宽度
 
我用得最多的解决方案是第二个选项，从Zen建立子主题，然后定制它来满足我的客户的需求。无论您选择哪种方式， 都值得购买 Jonathan Fielding 的这本书 “Beginning Responsive Web Design with HTML5 and CSS3” (Apress, 2014)，去指导你沿着这条路往前走。

##浏览器工具帮助你测试你的响应式主题
 
在选择和安装所需的主题后，最好下载浏览器插件，这样可以很容易看到你的站点在不同宽度设备上的效果（除非你足够富有去购买市场上每一种设备）。Chrome有一个很好的工具叫做响应式网页设计测试仪（Responsive Web Design Tester）。谷歌商店提供了一个免费的插件，提供常用设备的下拉式列表。单击工具栏上的工具图标,你会看到设备列表(见图18-4)。选择列表后，一个新的Chrome窗口就会打开，显示你的站点在那个设备上的效果。Firefox和Safari也有类似插件，工作一样好。

![图18-4 chrome浏览器的 Responsive Web Design Tester 插件](../images/pic-18-4.png) 

**图18-4. chrome浏览器的 Responsive Web Design Tester 插件**

有了浏览器的工具，下一步是看你的网站在不同的屏幕宽度和回答下列问题：

1. 在页面上哪些元素需要调整合适的显示（例如，列，图像和字体大小）
1. 对于较小的屏幕，哪些元素应该从页面中删除？重点应该是对小屏幕的用户来说最重要的内容。
1. 哪些导航元素需要改变，使得导航对没有鼠标指向和点击小型设备来说更易操作。

很可能，你会发现有的元素在不同的屏幕宽度下，为了在小屏幕宽度下可用，需要做出改变。为了使用能够改变显示元素的能力，你需要找到你的主题的样式表（通常出现在主题/ < themename > / CSS文件夹）和加上需要能够调整css的 media queries 。以下是 media queries 的推荐列表，在如今市场上的多数屏幕宽度上可用。

/* Smartphones (portrait and landscape) ----------- */ 

@media only screen and (min-device-width : 320px) and (max-device-width : 480px)

{ /* Styles */ }

/* Smartphones (landscape) ----------- */

@media only screen and (min-width : 321px)

 { /* Styles */ }


/* Smartphones (portrait) ----------- */

@media only screen and (max-width : 320px)

 { /* Styles */ }


/* iPads (portrait and landscape) ----------- */

@media only screen and (min-device-width : 768px) and (max-device-width : 1024px)

{ /* Styles */ }


/* iPads (landscape) ----------- */

@media only screen and (min-device-width : 768px) and(max-device-width : 1024px) and
(orientation : landscape) { /* Styles */ }

/* iPads (portrait) ----------- */

@media only screen and (min-device-width : 768px) and (max-device-width : 1024px) and (orientation : portrait) 

{ /*Styles */ }


/* Desktops and laptops ----------- */

@media only screen and (min-width : 1224px) 

{ /* Styles */ }

/* Large screens ----------- */

@media only screen and (min-width : 1824px)

 { /* Styles */ }

/* iPhone 4 ----------- */

@media only screen and (-webkit-min-device-pixel-ratio : 1.5), only screen and (min-device-pixel-ratio : 1.5)

 { /* Styles */ }

你会注意到，一些 media queries 指定所使用的设备的方向，纵向或横向。这只是另一种工具，用于收缩将被应用到设备上的css。

根据你使用的主题，media queries 将被放在 style.css 或者layout.css的结尾。如果你使用的主题已经通过 media queries 支持响应式，在这上面构建是个好主意。在Bartik主题的情况下，看着CSS目录，你会看到三个CSS文件：layout.css，style.css和colors.css。如果检查 layout.css 文件，你会看到主题的创建人已经在css 文件里放了两个 media queries。

@media all and (min-width: 560px) and (max-width: 850px)

@media all and (min-width: 851px)

这两个媒体查询中包含了一些CSS，他们覆盖了主题生成的出现在不同页面的元素。这两个 media queries 用于调整所有页面的元素，从而在所有设备上可用的用例，但是这里还有需要你去调整针对iphone4的特定元素的用例。加入以下 media query 和css相关条目，在iphone4或更小的屏幕的设备上能特殊变化。

@media all and (max-width: 320px)

你可能要调整的常见元素是列的宽度、字体大小和图像大小。s用浏览器的响应式页面设计工具查看站点，这会帮你识别哪些元素需要改变。

##修改普通css处理响应式

当设计一个响应式网站时，第一要考虑的事情是需要改变固定的像素宽度和基于点的字体大小。多年来，我们一直在设计像素完美的站点，但响应式设计，我们需要将我们的思维从精确的像素转向百分比和行长单位(ems)。如果你是一个有经验的设计师和css开发者，改变你看待设计和用css实现设计是最难的一个方面。一旦您完成了转型，这个过程将是比较简单的——但是首先，我们要思考页面上的元素尺寸将要占用的显示宽度的百分比。如果你使用固定的像素宽度，其结果将是你的访问者使用小屏幕设备时将必须水平滚动才能看到你的网站完整宽度。举个例子，用你的智能手机查看 whitehouse.gov 。不幸的是，这个网站没有响应式（到2015年初），需要你用你的手指向右滚动，才能看到该网站的整个宽度。

首先要检查的页面的任何元素，当你在小屏幕上访问网站时，是使网站缩小以适应窗口还是需要你向右滚动才能看到页面的完整宽度。Drupal站点的典型元素包括 regions，blocks，panel panes，和 views 经常被要求进行调整。这些元素通常会被设为固定像素宽度，要改成响应式，需要设成百分比宽度。

要确定改变哪些CSS元素，使用谷歌 Chrome Responsive Web Design Tester，只需右键单击您要更改的元素，然后选择检查元素（ Inspect Element） 选项。（见图18-5）

![图18-5 使用chrome的 Inspect Element 工具](../images/pic-18-5.png) 

**图18-5. 使用Chrome的 Inspect Element 工具**

媒体查询，浏览器响应式设计工具（如Chrome 的 Responsive Web Design Tester）以及通过右键审查元素快速定位CSS的方法使得制作响应式主题成为相对容易的任务。剩下的唯一一件事情就是添加带有媒体查询的{}的CSS选择器和改变CSS的属性，比如把宽度调到一个合适的值，浏览器完成剩下的事情。

从响应式设计到表现适宜的跨越设备宽度，有一个学习曲线。旅程从第一步开始，你现在有足够的知识兴致盎然的走下去。

##总结

在今天这样一个移动世界，建立一个网站，如果不是响应移动设备的网站，那么你的大部分站点访问者会在用户体验上有挫败感。花写时间增加一些媒体查询和css覆写 css文件 ，您的网站的访问者会更开心。虽然不可能总是取悦所有的人，至少试着适配你的网站访客可能会用的设备是一个很好的主意。正如你在本章中看到的，它确实不难，而且实际上很有趣！