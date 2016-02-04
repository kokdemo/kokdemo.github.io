title: "hexo主题开发"
date: 2016-02-04 15:13:40
tags:
- hexo
- jade
categories: 
- 技术笔记
---

hexo作为一个比较受欢迎的静态blog生成器，其个性主题也是比较受大家欢迎的点。只要有一定的js，css，jade的开发栈就可以实现一个功能强大的主题效果。

当然如果要好看则是另外一回事了……

<!-- more -->
如果你并不想要折腾主题的话，我推荐以下几个有趣强大的主题，直接拖到hexo的theme文件夹就好。（什么，你不会？建议你可以换简单一些的blog，比如wordpress？）

1. `NexT` 简洁而强大 https://github.com/iissnan/hexo-theme-next
2. `Jacman` 比较俏皮的风格 https://github.com/wuchong/jacman
3. `Apollo` 足够简单，我目前用的就是我自己修改过的apollo主题（增强了一点功能，原版的apollo没有tag和category……） https://github.com/pinggod/hexo-theme-apollo

更多的主题见这里 https://hexo.io/themes/

言归正传，因为hexo本身就比较简单，因此分几步来讲：

### hexo的路由机制

hexo能够自动解析的页面有

- `index` 首页
- `post` 文章
- `page` 分页
- `archive` 日期归档
- `category` 分类归档
- `tag` 标签归档
   
这里的每一个页面都对应了主题文件夹下，layout的一个文件。因此建立主题的时候，这些文件都是可选的。当然，index是必选的。
    
### hexo的变量

实际上，如果你就这么简单的创建了文件，并不会加载出文件来，hexo在渲染文件的时候，会根据页面上的一些变量来进行操作。  
这部分的内容hexo已经给好了，比如说全局变量就有：

- `site` 网站变量
- `page` 页面内容
- `config` 网站设置，也就是写在_config.yml中的内容
- `theme` 主题设置，也就是写在theme/_config.yml中的内容
- `path` 页面路径
- `url` 完整网址
- `env` 环境变量
 
更多的不在这里赘述，看官方文档 https://hexo.io/zh-cn/docs/variables.html

我就用Apollo的一段模板来解释一下吧。

```jade
    //- Post Page
    mixin post(item)
        .post
            article.post-block
                h1.post-title
                    != item.title 
                +postInfo(item)
                .post-content
                    != item.content
```

其中`mixin post(item)` 是jade的语法，指重用jade块，这里不详细说了，有兴趣的可以戳 https://cnodejs.org/topic/5368adc5cf738dd6090060f2

这里的参数就是一篇文章`post`，因此，我们就可以代码块中调用`item.title`文章标题，`item.content`文章内容。

同样的，只要我想，我也可以给里面插入`item.date`页面建立日期，`item.updated`页面更新日期，`item.excerpt`页面摘要。
 
同样的页面，如果在NexT中的写法如下：

> 这里本来会有NexT的swig代码，然而不知道为什么hexo文章中只要有了这些代码立马报错……
我也是醉了，就这样吧，关于NexT代码的分析就无疾而终了……
有兴趣的看这里 https://github.com/iissnan/hexo-theme-next/blob/master/layout/archive.swig

NexT的功能就多很多了，这里读取了页面变量`page`中的所有文章`posts`，是一个数组，数组中的每一项都对应着一个文章，所以就可以看到这里读取了`post.date`作为年份，当然这里使用了一个辅助函数。

我在开发的过程中还找到了一些有用的变量，比如上面说到的 `page.posts` 中的每一个文章中，都有一个 `tags.data` 属性，这也是一个数组，可以读取这篇文章中所有的tag名称。
`categories`属性则是一个对象，可以用foreach的方法进行访问。

### hexo的辅助函数

上一段你可能已经看到了`date()`函数，这就是hexo 提供的辅助函数，比较常用的函数有：

- `url_for()` 生成一个全局可用的路径
- `js()` 导入一个js   
- `image_tag()` 插入图片
- `is_home()` 判断是不是首页，这类函数还有很多
- `date()` 插入格式化的日期
- `tagcloud()` 插入标签云

善用这些辅助函数，可以让你少些很多冗余的代码，因此在开脑洞施工之前，先看看这个功能官方是不是已经做了。


那么第一篇就写这么多，更多的也可以看hexo的官网。

祝你折腾的愉快