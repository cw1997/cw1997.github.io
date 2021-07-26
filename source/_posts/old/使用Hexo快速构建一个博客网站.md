---
title: 使用Hexo快速构建一个博客网站
date: 2018-11-22 00:42:02
tags: 
categories: blog
---

# 本地构建

首先安装好node.js，然后跟着我敲命令就对了。

我以macOS为例，Windows用户推荐使用bash shell

- `npm install -g hexo-cli # 安裝hexo命令行工具，安装之后就可以直接使用hexo命令了。因为node自动设置了PATH环境变量，你输入hexo命令，操作系统会自动在node包目录下寻找到对应的node.js脚本执行`
- `hexo init blog # 在当前目录下创建一个名为blog的目录，并且把blog目录作为整个博客站点的web目录，然后初在该目录下始化生成npm的依赖关系脚本`
- `cd blog # 进入blog目录，这里cd是change directory的意思，可不是“给你一张过去的CD”`
- `npm install # 根据之前生成的npm依赖关系声明文件package.json，自动下载并且安装对应的依赖库文件。这些依赖库文件构成了hexo的核心程序代码。（据我测试，在新版本的hexo-cli下，使用hexo init blog命令其实是会自动下载并且安装好hexo的相关依赖和核心程序代码，所以这一步在新版本上是否有必要是指的怀疑的。不过为了保险起见，重复install一次也没什么影响。）`
- `hexo generate # 执行后，自动根据配置文件和已有的博客内容构建public目录。` 
- `hexo server # 执行后，会自动在本地的4000端口上开启一个web服务器，请在浏览器地址栏输入 http://127.0.0.1:4000/ 访问，如果成功显示博客站点，则表示无问题。` 

安装好一户的目录结构如下

	masaweideMacBook-Pro:blog changwei$ ls -al
	total 320
	drwxr-xr-x   13 changwei  staff     416 11 22 00:26 .
	drwxr-xr-x    8 changwei  staff     256 11 22 00:15 ..
	drwxr-xr-x    9 changwei  staff     288 11 22 00:26 .deploy_git
	-rw-r--r--    1 changwei  staff      65 11 22 00:15 .gitignore
	-rw-r--r--@   1 changwei  staff    1954 11 22 00:31 _config.yml
	-rw-r--r--    1 changwei  staff     174 11 22 00:42 db.json
	drwxr-xr-x  303 changwei  staff    9696 11 22 00:17 node_modules
	-rw-r--r--    1 changwei  staff  146130 11 22 00:17 package-lock.json
	-rw-r--r--    1 changwei  staff     482 11 22 00:19 package.json
	drwxr-xr-x    8 changwei  staff     256 11 22 00:26 public
	drwxr-xr-x    5 changwei  staff     160 11 22 00:15 scaffolds
	drwxr-xr-x    3 changwei  staff      96 11 22 00:15 source
	drwxr-xr-x    3 changwei  staff      96 11 22 00:15 themes

- _config.yml yml格式。配置文件。
- package.json json格式。hexo程序代码依赖关系声明文件，被npm所用，除非需要自行安装或者修改插件，否则一般不要随意修改。
- scaffolds.folder 文件夹。里面有数个md文件，定义了每次新建一个文章的时候，默认包含的内容。
- source.folder 文件夹。除 _posts 文件夹之外，开头命名为 _ (下划线)的文件 / 文件夹和隐藏的文件将会被忽略。Markdown 和 HTML 文件会被解析并放到 public 文件夹，而其他文件会被拷贝过去。
- themes.folder 文件夹。存储博客主题。可以认为是游戏中的skin，皮肤，或者界面样式。内含数个文件夹，每个文件夹为一个主题。文件夹名字为主题名字。除非需要手动修改主题，否则一般情况下不要随意修改。
- public.folder 文件夹。整个博客站点对外发布的目录。整个目录就是web目录，在这个目录下的文件都会被发布到web站点上。都可以被所有用户访问。一般情况下，他会通过hexo内建的脚本自动生成，不需要我们手动修改。

# 将该博客站点发布到GitHub Pages

安装git客户端

- `git config --global user.name "cw1997" # 设置git的全局用户名，为你github的用户名，请根据你的信息修改，不要照抄我的。`
- `git config --global user.email "867597730@qq.com" # 设置git的全局email为你github的注册邮箱，请根据你的信息修改，不要照抄我的。`
- `npm install hexo-deployer-git --save # 保证当前目录为blog（使用pwd检查），然后执行该命令安装hexo发布到git的一个依赖包。`

- 在web界面登陆你的github账号，创建一个名字为 `{你的github用户名}.github.io.git` 的仓库

修改_config.yml，将 deploy 配置项修改为：

	deploy:
	  type: git
	  repo: https://github.com/cw1997/cw1997.github.io.git
	  branch: master

其中repo后面记得修改为你自己的github用户名，不要照抄我的 “cw1997”

- `hexo generate # 执行构建操作，构建public目录`
- `hexo deploy # 执行发布操作，会提示输入用户名密码，记得输入你的github用户名和github登陆密码`
- 打开 `{你的github用户名}.github.io.git` 欣赏的博客

# 给博客设置一个独立域名

- 首先购买一个域名，并且知道如何访问域名控制面板进行域名解析操作
- 进入你的github仓库设置页面，URL为 `https://github.com/{你的github用户名}/{你的github用户名}.github.io/settings`
- 在下面找到  `Custom domain` ，将他设置为你自己的网站域名之后点 SAVE 按钮保存
- 进入域名解析面板，新增一条解析记录，类型为 CNAME ，记录名字为你想要的博客的二级域名，例如你想要`www.changwei.me`访问，那么记录名字就是`www`。记录值为`{你的github用户名}.github.io.git`。然后保存即可，等待一段时间，待DNS解析生效后即可使用自己的域名访问了。



# 参考资料

- https://hexo.io/zh-cn/docs/index.html
- https://zhuanlan.zhihu.com/p/26625249
- https://haoshuai6.github.io/2016-11-25-Hexo-github-cname.html
- https://blog.amaork.me/2017/10/28/Hexo-%E7%BB%91%E5%AE%9A%E4%BA%8C%E7%BA%A7%E5%9F%9F%E5%90%8D/