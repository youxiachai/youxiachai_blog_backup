title: Hexo 主题制作
date: 2013-04-09 05:00:00
tags: hexo
---

##主题相关变量##
###site `<% site %>`###

``` js
{
"posts": " 所有文章，根据发表日期降序排列 [Collection事件](http://zespia.tw/hexo/zh-CN/docs/collection.html#collection)",

"pages": " 所有页面，根据发表日期降序排列 [Collection事件](http://zespia.tw/hexo/zh-CN/docs/collection.html#collection)",

"categories ": "所有分类，根据字母顺序排列, [Taxonomy 事件](http://zespia.tw/hexo/zh-CN/docs/collection.html#taxonomy)",

"tags ": "所有标签，根据字母顺序排列[ Taxonomy 事件](http://zespia.tw/hexo/zh-CN/docs/collection.html#taxonomy)"
}
```
<!-- more -->
例如获取所有文章标题:

`<% site.posts.each(function(item){ %>
 <%= item.title%>
<% }); %>`

###Page `<% page %>`###
注意 **页面的资料，内容根据不同页面而有所差异，由 Generator 所控制**

####下面为page,post 对象内容:####

**post**
``` js
{
"title": "文章标题",

"date": "文章的发布日期（[Moment.js 库](http://momentjs.com/)）",

"updated": "文章的更新日期（[Moment.js 库](http://momentjs.com/)）",

"comments": " 开启此文章的留言功能 boolean 值",

"layout": " 文章布局",

"content": "文章内文",

"excerpt": " 文章摘要（内文中 <!-- more --> 之前的内容）",

"source": "about/index.html 文件的相对路径",

"path": "about/index.html 文章的相对路径",

"ctime": "2013-04-05T14:38:36.000Z",

"mtime": "2013-04-05T14:38:41.000Z",

"permalink": "http://yoursite.com/about/index.html 完整的网络访问路径",

"full_path": "D:\\hexo11/source/about/index.html 原始文件路径",

"categories" : "{}",

"tags": "{}"

"_id": 1
}
```

page 和 post 的差别不大，仅在于 page 没有categories和tags变量。
有些变量貌似文档没写,这里就补充了一下...

**注意,这里的page属于单个 collection 迭代的单个对象**


**Generator控制 的page**

基础布局模板:

**page为  [Collection事件](http://zespia.tw/hexo/zh-CN/docs/collection.html#collection)**

- layout
- archive
- category
- index
- tag

例如在以上模板中可以用collection事件来获得具体的page内容:

`<% page.posts.each(function(item){ %>
 <%= item.title%>
<% }); %>`

**page为  post的内容的**

- page
- post
- 自定义模板

在以上模板中可以直接`<%= page.title %>`获得对应的内容.


###config `<% config %>`###

 全局设定，即[_config.yml](http://zespia.tw/hexo/zh-CN/docs/configure.html)的内容


###theme `<% theme %>`###
主题设定，即主题文件夹内_config.yml的内容，根据不同主题而有所差异

###__（双底线）`<% __('string') %>`###
- 取得 [国际化](http://zespia.tw/hexo/zh-CN/docs/global-variables.html#i18n)（i18n） 字串 参考默认主题的 `languages/`下的文件

例如:

`<%= __('home')%>`



##渲染流程##
hexo v1.1.2的主题渲染,有两种渲染方式

###第一种基础布局渲染###

![](/images/renderpage1.png)
你也可以在主题中自定其他布局，例如link或photo之类的，若找不到自定的布局的话，则会根据 Generator 的不同，使用相对应的布局代替。
（至少要有index布局)

###第二种带layout 布局###
与第一种不同的是,多了一个全局的模板,layout.ejs 文件可以用这个代码`<%- body %>`把**page对象传递到下面的模板**

![](/images/renderpage2.png)



##加载动态组件##
对于一个静态博客而言,最大的不便就是无法进行评论,不过关于这块,其实,已经有很多解决方案,下面以中国的多说为例

我们只需要在相应的模板加载对应的代码即可...例如


<script src="https://gist.github.com/youxiachai/5349668.js" async="true"></script>
