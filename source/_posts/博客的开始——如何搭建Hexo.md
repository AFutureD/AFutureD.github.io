---
title: 博客的开始——如何搭建Hexo
date: 2016-12-27 09:43:57
tags: Hexo
categories: 技术类
---

## 概述

#### 导语

> Hexo 是一个快速、简洁且高效的博客框架。Hexo 使用 [Markdown](http://daringfireball.net/projects/markdown/)（或其他渲染引擎）解析文章，在几秒内，即可利用靓丽的主题生成静态网页。并且能一键部署到GitHub Pages。

首先，不论这篇文章主要是为了安装什么，第一步该做的是先说明一遍大致的过程，以使读者能够清楚，自己究竟在干什么、还有什么没有完成、为什么要这么做。当然，我会给出一些必要的网站，它们以官网为主，庆幸的是，这些官网都有简体中文的支持。最后，说明一下作者的系统为Mint Linux，它和Ubuntu是一样的，并且作者是一个大二菜鸟，如果有错的话，希望大家能够指出错误，我也会立即改正。

### 具体过程

1. 准备安装环境 
   * Node.js
   * Git
2. 安装Hexo
3. 开始搭建博客
   * 初始化Hexo
   * 修改全局配置文件
   * 一次简单的同步
4. 添加新文章
5. 更换主题

### 给出网址

* [Hexo官方网站](https://hexo.io/zh-cn/)
* [Material主题官网](https://material.vss.im/)
* [史上最详细的Hexo博客搭建图文教程](https://xuanwo.org/2015/03/26/hexo-intor/)

## 准备安装环境 

### 安装Node.js

很不幸运的是官方给出的 Node.js 安装方法并不是非常有效。因此我通过Baidu找到了一种简单的方法，在此给出过程。

1. 下载

   第一步很简单，就是从官网上下载二进制包。给出地址。

   * [Node.js官网下载地址](https://nodejs.org/en/download/)

2. 解压下载好的 node-v6.9.2-linux-x64.tar.xz 压缩包

   ```
   $ tar xvf node-v6.9.2-linux-x64.tar.xz 
   ```

   这样，你可以得到一个名为 node-v6.9.2-linux-x64 的文件夹。

3. 验证 Node.js 的版本

   首先进入 node-v6.9.2-linux-x64 文件夹下的 bin 目录，你会发现有两个可执行文件。如下：

   ```
   $ cd node-v6.9.2-linux-x64/bin
   $ ls
   node  npm
   ```

   接着我们来看看 Node.js 的版本

   ```
   $ ./node -v
   v6.9.2
   ```

   很好，它是最新的6.9.2版本。

4. 把二进制包放到较为规范的地方。

   什么叫较为规范的地方？举个例子，在Windows下，排除自己定义安装路径的软件，你所有的软件都会在这样一个地址下 `C:\Program Files` 。在Mint Linux上，我把它规定为 `/opt` ，这个路径包含了所有我手动安装的软件，毕竟虽然有 **apt** ，但是总有些软件不能通过apt安装。很好，下面让我们把它挪到那个规范的地方。

   ```
   $ sudo mv node-v6.9.2-linux-x64 /opt/
   $ cd /opt
   $ ls
   clion  eclipse  google  node-v6.9.2-linux-x64  pycharm  sublime_text
   ```

   由此你可以发现，我已经成功移动了文件。这里有个小问题，在执行第一句命令的时候，会提示需要密码，不要担心，直接输入root密码就行，它不是明文的，并不会显示字符。

5. 建立软链接，设置全局

   怎么在shell中直接访问呢？就是通过软链接实现。

   ```
   $ sudo ln -s /opt/node-v6.9.2-linux-x64/bin/node /usr/local/bin/node
   $ sudo ln -s /opt/node-v6.9.2-linux-x64/bin/npm /usr/local/bin/npm
   $ cd /usr/local/bin/
   $ ls |grep '^[n]'
   node
   npm
   ```

   你会发现，在 `/usr/local/bin`这个目录下已经有 **node** 、**npm** 两个文件了。

6. 验证成功

   打开terminal，输入`node -v`和`npm -v` 来检查是否成功。

   ```
   $ node -v
   v6.9.2
   $ npm -v
   3.10.9
   ```

   由此Node.js安装完成，看似很复杂，其实很简单。

### 安装Git

Git的安装是通过apt，极其便捷。

```
sudo apt-get install git
```

这样就安装完了。这就是包管理的好处。

其次，ssh的配置安装则参考[Git教程 - *廖雪峰的官方网站*](http://www.liaoxuefeng.com/wiki/0013739516305929606dd18361248578c67b8067c8c017b000/001374385852170d9c7adf13c30429b9660d0eb689dd43a000) ,这是非常好的git教程网站。

## 安装Hexo

所有必备的应用程序安装完成后，即可使用 npm 安装 Hexo。根据官方教程：

```
$ npm install -g hexo-cli
```

如果出现WARNING，那么你可以忽视它。如果出现ERROR，那么请你使用Baidu或者Bing来解决问题，作者病不能，开速有效的替你解决。

## 开始搭建博客

###  初始化Hexo

首先，我打算把博客的根地址定在 `～/Document/` 。那么开始

```
$ cd ~/Documents/
$ hexo init Blog
$ cd Blog
$ npm install
```

新建完成后，指定文件夹的目录如下：

```
.
├── _config.yml
├── package.json
├── scaffolds
├── source
|   ├── _drafts
|   └── _posts
└── themes
```

注意，这里有一些问题。执行第二个命令时常常由于网络的问题卡住，那么建议你把npm的源改为淘宝源，具体教程查看[npm设置淘宝镜像](http://www.jianshu.com/p/fb7251740107/comments/330864)

### 修改全局配置文件 

您可以在 `_config.yml` 中修改大部份的配置。当然参考[官方文档](https://hexo.io/zh-cn/docs/configuration.html)以获得最好的支持

当然你可以查看我的修改方法。

#### 网站

| 参数          | 描述      | 我的配置          |
| ----------- | ------- | ------------- |
| title       | 网站标题    | Francis'Blog  |
| subtitle    | 网站副标题   |               |
| description | 网站描述    |               |
| author      | 作者的名字   | Andy Francis  |
| language    | 网站使用的语言 | zh-CN         |
| timezone    | 网站时区    | Asia/Shanghai |

#### 网址

| 参数                  | 描述                                       | 我的配置                           |
| ------------------- | ---------------------------------------- | ------------------------------ |
| `url`               | 网址                                       | https://dongfrancis.github.io/ |
| `root`              | 网站根目录                                    | /                              |
| `permalink`         | 文章的 [永久链接](https://hexo.io/zh-cn/docs/permalinks.html) 格式 | `:year/:month/:day/:title/`    |
| `permalink_default` | 永久链接中各部分的默认值                             |                                |

#### 目录

| 参数             | 描述                                       | 我的配置             |
| -------------- | ---------------------------------------- | ---------------- |
| `source_dir`   | 资源文件夹，这个文件夹用来存放内容。                       | `source`         |
| `public_dir`   | 公共文件夹，这个文件夹用于存放生成的站点文件。                  | `public`         |
| `tag_dir`      | 标签文件夹                                    | `tags`           |
| `archive_dir`  | 归档文件夹                                    | `archives`       |
| `category_dir` | 分类文件夹                                    | `categories`     |
| `code_dir`     | Include code 文件夹                         | `downloads/code` |
| `i18n_dir`     | 国际化（i18n）文件夹                             | `:lang`          |
| `skip_render`  | 跳过指定文件的渲染，您可使用 [glob 表达式](https://github.com/isaacs/node-glob)来匹配路径。 |                  |

#### 文章

| 参数                  | 描述                                       | 我的配置      |
| ------------------- | ---------------------------------------- | --------- |
| `new_post_name`     | 新文章的文件名称                                 | :title.md |
| `default_layout`    | 预设布局                                     | post      |
| `auto_spacing`      | 在中文和英文之间加入空格                             | false     |
| `titlecase`         | 把标题转换为 title case                        | false     |
| `external_link`     | 在新标签中打开链接                                | true      |
| `filename_case`     | 把文件名称转换为 (1) 小写或 (2) 大写                  | 0         |
| `render_drafts`     | 显示草稿                                     | false     |
| `post_asset_folder` | 启动 [Asset 文件夹](https://hexo.io/zh-cn/docs/asset-folders.html) | false     |
| `relative_link`     | 把链接改为与根目录的相对位址                           | false     |
| `future`            | 显示未来的文章                                  | true      |
| `highlight`         | 代码块的设置                                   |           |

#### 分类 & 标签

| 参数                 | 描述   | 我的配置            |
| ------------------ | ---- | --------------- |
| `default_category` | 默认分类 | `uncategorized` |
| `category_map`     | 分类别名 |                 |
| `tag_map`          | 标签别名 |                 |

#### 日期 / 时间格式

Hexo 使用 [Moment.js](http://momentjs.com/) 来解析和显示时间。

| 参数            | 描述   | 我的配置         |
| ------------- | ---- | ------------ |
| `date_format` | 日期格式 | `YYYY-MM-DD` |
| `time_format` | 时间格式 | `H:mm:ss`    |

#### 分页

| 参数               | 描述                    | 我的配置   |
| ---------------- | --------------------- | ------ |
| `per_page`       | 每页显示的文章量 (0 = 关闭分页功能) | `10`   |
| `pagination_dir` | 分页目录                  | `page` |

#### 扩展

| 参数      | 描述                    | 我的配置     |
| ------- | --------------------- | -------- |
| `theme` | 当前主题名称。值为`false`时禁用主题 | material |

此theme的配置默认为landscape，我这里的material为其它主题。

#### Deployment

> deploy:
> ​     type: git
> ​     repo: git@github.com:DongFrancis/DongFrancis.github.io.git
> ​     branch: master

### 一次简单的同步

#### 本地尝试

如果以上内容你已经完成那么我们可以试着在本地测试一下，首先你必须进入博客的根目录，其次启动服务。想这样：

```
$ cd ~/Documents/Blog
$ hexo generate
$ hexo server
```

然后打开浏览器，进入地址 http://localhost:4000/ ，你会发现你的个人博客已经搭建完成！！！

#### 同步到Github Pages

> 如果你想同步到Github Pages，确保你已经完成了 **git的安装与配置**、 **git的ssh设置**、 **Github Pages的申请与建立**、**Deployment的配置** 。

很好，现在我们可以继续了。

##### 安装插件

进入Blog根目录，执行如下操作

```
$ npm install hexo-deployer-git --save
```

##### 同步——deploy

在Blog根目录，执行如下操作

```
$ hexo deploy
```

##### 验证

打开Github pages的个人主页，如 https://DongFrancis.github.io.git，你可以验证是否同步成功。

## 四、添加新文章

#### 创建一篇新文章 

在Blog根目录下，你可以使用 `new`命令来新建文章。如

```
$ hexo new "my-first-blog"
```

执行完此命令后，在`source/_posts/` 目录下会有一个新的文件 `my-first-blog.md` 。

#### 测试

毫无疑问，对于自己些的文章，你总希望确认一下是否完美，这样你才可以展示给其它人看。

具体这样来完成：

```
$ hexo generate
$ hexo server
```

很熟悉的俩句话是么？没错，这就是本地测试的俩个命令，第一句的意思是*生成文件*，第二局的意思是*打开本地服务器*  。

#### 同步到Github Pages

很简单使用 **deploy** 命令即可。

```
$ hexo deploy
```

执行完命令后，你便可以在Github Pages上查看了。

## 更换主题

我选择的是 Meterial 主题，一句话：好看！

具体的过程和[官网的教程](https://material.vss.im/start/)一样这里就不详细讲了。

## 总结

搭建博客的作用对于不同的人有不同的作用。对于我来说，是希望将自己所学的知识进一步整理与归纳，以此逐步提升自己。希望这篇教程对大家有所帮助。
