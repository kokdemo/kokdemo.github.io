title: 关于hexo静态blog的搭建，美化
date: 2014-04-30 14:53:18
tags:
- hexo
- ghost
- nodejs
- blog
categories:
- 技术
---

最近看我的wordpress博客越来越不爽了……

<--! more -->
主要是因为wordpress真的太大，繁琐的厉害，最重要的是，它的反垃圾评论功能似乎没有以前那么好用了。

因此每次登陆的时候都能看到满坑满谷的垃圾评论……
实在是删不过来啊……

在某人的忽悠下，我进了hexo的坑，也就是目前这个博客所用的静态生成器。hexo可以看作是octopress思路的一种扩展，即用markdown写作，然后生成静态网页，托管到github page上。

可是octopress使用ruby写的，对于广大js屌丝们来说不是很友好……于是就有了台湾朋友写的这个hexo。

我fork的主题来自于jacman，而jacman是pacman的fork，那么我这个主题应该算是fork自pacman了……

###hexo

hexo的安装就是使用

    npm install -g hexo
tips：安装之前请先注意下你的npm的安装方式，如果npm是apt-get安装的旧版本，你有必要重新安装一次。

    curl http://npmjs.org/install.sh | sh
    或者：
    curl http://npmjs.org/install.sh | sudo sh
tips：什么，你没有curl，那么你还是先用apt-get安一个curl好了……

安装好hexo之后就可以开始生成你的第一个blog了。

cd到目标文件夹之后

    hexo init //生成基本的结构
    hexo generate //生成静态网页
    hexo server //运行blog，在localhost:4000上。
    hexo clean //清楚hexo生成的网页和数据库，出错的时候可以试一下。
    hexo new [post] <title> //一目了然，就是写新文章，post是文章类型的一种。
以上就是基本命令。

hexo的基本结构如下：

    .
    ├── .deploy
    ├── public //生成的静态网页
    ├── scaffolds //layout模板文件目录，其中的md文件可以添加编辑
    ├── scripts //扩展脚本目录，平常里面只有一个处理图片的js
    ├── source //你的文章保存的地方，你还可以在里面自建文件夹放页面什么的
     |   ├── _drafts
     |   └── _posts //存放的位置
    ├── themes //主题
    ├── _config.yml //主要的配置文件，务必要读一下，把里面的参数改成你需要的
    └── package.json

生成静态网页之后就可以扔到你的主机上/github page上，同时hexo还提供了直接部署的命令，deploy，有兴趣的你可以自行google一下。

关于wordpress-->hexo的迁移主要是依靠一个插件hexo-migrator-wordpress。

1. 首先，需要在Word Press导出博客，会得到一个xml文件，把它拷贝到hexo博客目录下，比如叫wordpress.xml。
2. 安装hexo-migrator-wordpress这个插件

首先用npm安装

    npm install hexo-migrator-wordpress --save

3. 导入

运行

    hexo migrate wordpress wordpress.xml
会看到导入了一些Word Press里的文章和页面，看看source里的文件，发现给转成了markdown格式的，这时候多半还需要一些格式微调。

另外，文章里面的图片会指向原来的博客资源，文章链接也是绝对的。因此将来删除wordpress的时候最好保留下来其中存图片的文件夹……

>此时又有人告诉我，ghost比hexo更好用，你快去试试吧……

###ghost

ghost是最近很火的一个基于nodejs的博客，不像hexo那样只是一个生成器，它有一个比较简陋的后台，可以直接在里面用markdown写东西，同时可以看到预览。

ghost的官网 https://ghost.org/ ，上面逼格相当高……

安装的方法就是从官网上下载安装包，解压之后，cd到那个文件夹，然后

    npm install --production
就行了，然后运行呢

    npm start
ok，这时候你在浏览器里打开 http://localhost:2368 就能看到界面了，默认的主题也是非常酷炫，比hexo不知道高到哪里去了……

访问 http://localhost:2368/ghost 进入后台，你可以看到很多选项，包括换掉图片，更改标题副标题，自我信息什么的，还有文章的控制，删除/修改都有，算是比较齐全。

ghost的主题也和hexo类似，里面都包括了模板，css，js，图片等等东西，你有兴趣可以自己改改看，我当时fork了ghostium这个主题，自己做了一些修改。

>问题来了，这不是一个静态blog，那我应该把它部署在哪？

当我发现这个问题之后纠结的不行，想要在vps上用，还需要先学习一下lxc……于是我还是换回hexo了……

现在有一些主机商开始提供ghost的主机服务了，如果你有这个机会，可以尝尝鲜！

因为hexo是静态的，放到哪里都行。

>问题2，ghost不支持分类，怎么办？

其实很多人直接通过控制tag来为文章分类了……我自己不是很习惯，因为之前还有好多篇文章都没改……

###markdown
最近比较流行用markdown来写作，我用sphinx的时候，觉得rst其实也不错……

markdown中常用的标题，列表，引用，代码格式，这篇文章里都用到了，我在这写一点不常用的……

1. markdown中<\code><\pre> 都是可以正常用的。此处为了正确显示，加了一个反斜杠。
2. 链接[an example][id]\(http://example.com/ "Title")

   [id]: http://example.com/  "Optional Title Here"，说到这个，我更喜欢rst里的链接格式一些……
3. 图片![Alt text]\(/path/to/img.jpg "Optional title")





