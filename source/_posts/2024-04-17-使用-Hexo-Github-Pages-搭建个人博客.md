---
title: 使用 Hexo + Github Pages 搭建个人博客
date: 2024-04-17 16:09:20
tags:
---

# 方案优势
- 简单易用
  
  `Hexo` 是一个轻量级的静态网站生成器，它使得创建和管理博客变得非常简单。我们只需要掌握基本的 [Markdown](https://daringfireball.net/projects/markdown/) 语法就可以撰写文章，而不需要深入了解 `HTML`、`CSS` 或 `JavaScript` 等前端技术。

- 快速部署

  静态网站的优势之一是加载速度快。由于 [GitHub Pages](https://pages.github.com/) 提供了免费的静态网页托管服务，意味着我们可以在几秒内将博客部署到全球，提供快速的访问速度和良好的用户体验。
  
- 版本控制

  通过结合 `Git` ，我们可以利用 `GitHub` 进行版本控制。我们可以追踪文章的历史变更、协作编写内容，并且可以在出现问题时轻松回滚到之前的版本。
  
- 免费或低成本

  `GitHub Pages` 为静态网站提供免费的托管服务，尤其对于个人项目和小型网站来非常合适。

基于以上的优势以及我的实际情况，`Hexo` + `Github Pages` 目前是适合我这种个人博主的。

<!-- more -->


# 开发环境
我的开发环境是 `MacOS`，如果你是 `Windows`，请结合参考其他教程。从 [Hexo文档](https://hexo.io/zh-cn/docs/) 可以得知，安装 `Hexo` 之前，需要安装 [Node.js](https://dev.nodejs.cn/learn/how-to-install-nodejs/)（建议Node.js 12.0及以上版本）以及 [Git](https://git-scm.com/)。
如果你跟我一样是 `MacOS` 环境，可以使用 [nvm](https://github.com/nvm-sh/nvm)， `nvm` 允许你轻松切换 `Node.js` 版本，并安装新版本以尝试在出现问题时轻松回滚。
    

# 操作步骤
## 1、GitHub 仓库
创建仓库，仓库名称格式要求：`<username>.github.io` 如果你的用户名为 `wuyanzu` ，那么仓库名为 `wuyanzu.github.io`，注意如果用户名包含大写字母，仓库名中需要转为小写。
![](git-create-repository1.png)

将线上仓库克隆到本地，后面我们需要将 `Hexo` 生成博客工程放到这个文件夹中。以便部署到 `GitHub` 上。
```
git clone git@github.com:vampire-locker/vampire-locker.github.io.git
```
![](git-clone2.png)


## 2、安装 Hexo
如果已安装，请忽略。
```
npm install hexo-cli -g
```


## 3、生成博客工程
如果已新建博客工程的根文件夹（文件夹名字自定义，此处以 `blog` 为例）
```
cd blog && hexo init && npm install
```

如果未新建博客工程的根文件夹
```
hexo init blog && cd blog && npm install
```
生成的博客工程如下：
![](hexo-init2.png)


## 4、生成静态文件
命令可以简写为 `hexo g`
```
hexo generate
```
生成的博客静态文件存放在 `/blog/public/`下
![](hexo-generate2.png)
![](hexo-generate3.png)


## 5、启动服务器
启动本地服务器，命令可简写为 `hexo s`。默认情况下，访问网址为 `http://localhost:4000/`
```
hexo server
```
如果前面操作没问题，此时可以打开
![](hexo-server2.png)


## 6、将博客工程放入仓库
将博客工程 `blog` 下的所有文件放进 `Git` 仓库 `vampire-locker.github.io` 文件夹中
![](move-file1.png)

提交到线上仓库
```
git add . && git commit -m 'blog init' && git push
```
![](git-push1.png)

聪明的你一定会发现，前面我们提到生成的静态文件存放在 `public` 文件夹下，可是并没有被提交到线上。
![](git-push2.png)

查看一下 /vampire-locker.github.io/.gitignore/ 文件，原来 `public` 文件夹被加入了忽略名单。因此我们后面才需要将 `deploy` 的 `branch` 配置为 `gh-pages` 分支。即原始的博客工程文件提交到 `main` ，而生成的静态网站的文件提交到 `gh-pages` 分支。
![](git-ignore.png)



## 7、部署网站
在 `_config.yml` 中配置静态网站要部署的地址和分支
```
deploy:
  type: git
  repo: git@github.com:vampire-locker/vampire-locker.github.io.git
  branch: gh-pages
```

安装插件 `hexo-deployer-git`
```
cd vampire-locker.github.io &&
npm install hexo-deployer-git --save
```

开始部署，命令可简写为 `hexo d`
```
hexo deploy
```
![](hexo-deploy3.png)

部署完成，可以发现 `gh-pages` 分支已经被部署到 `Github` 上。
![](hexo-deploy4.png)
![](hexo-deploy5.png)

## 8、GitHub Pages
在仓库的 `Settings` -> `Pages` 中，将 `Pages site` 指向分支根目录，按 `save` 按钮进行保存。首次配置可能需要10分钟左右才能生效。
![](github-pages.png)

## 9、完成
打开`https://username.github.io/`，`username` 修改为你的实际名称。如果打开成功，说明部署完成。
![](hexo-deploy6.png)

# 工程结构
| 文件/文件夹名称         | 描述                                                         |
|------------------------|--------------------------------------------------------------|
| `_config.landscape.yml`   | 默认主题 `landscape` 的配置文件。                                  |
| `_config.yml`           | `Hexo` 工程的配置文件，包含了网站的全局设置，如标题、描述、语言、主题等。 |
| `.deploy_git`           | `Git` 相关，`hexo deploy` 相关。                                     |
| `.git`                  | `Git` 仓库的隐藏目录，包含了所有的版本控制信息。                     |
| `.github`               | `GitHub` 相关配置和工作流。                                         |
| `.gitignore`            | 列出了不被 `Git` 跟踪的文件和文件夹。                               |
| `db.json`               | 存储网站数据的数据库文件，例如文章列表，评论等。                     |
| `node_modules`          | 包含了项目所依赖的 `Node.js` 模块，通过 `npm` 安装。                 |
| `package-lock.json`    | 记录项目依赖的具体版本，确保依赖版本一致。                           |
| `package.json`          | `Node.js` 项目的核心文件，包含项目元数据和依赖列表，定义脚本命令。   |
| `public`               | 包含构建后的静态文件，如 `HTML`、`CSS` 和 `JavaScript` 文件。       |
| `README.md`            | 项目的描述文件。                                                 |
| `scaffolds`            | 包含 `Hexo` 的模板文件，用于生成新页面或文章的基础结构。             |
| `source`               | 包含 `Hexo` 网站的源文件，如 `Markdown` 格式的文章、静态页面等。     |
| `themes`               | 包含 `Hexo` 主题相关的文件，定义网站外观和布局。                       |


# 项目配置
## _config.yml 文件
| 配置         | 描述                                                         |
|------------------------|--------------------------------------------------------------|
| title | 网站标题 |
| subtitle | 网站的副标题 |
| description | 网站的描述，通常用于SEO |
| keywords | 网站的关键词，用于搜索引擎优化 |
| author | 网站作者的名字 |
| language | 网站使用的语言代码，如中文：zh-CN 英语：en |
| timezone | 网站所用的时区 |
| - | - |
| url | 网站的URL地址 |
| permalink | 文章链接的格式 |
| permalink_defaults | 默认的链接参数 |
| pretty_urls | 漂亮的URL设置，如是否已出尾随的 index.html 和 .html |
| - | - |
| source_dir | 源文件目录 |
| public_dir | 构建后的静态文件目录 |
| tag_dir | 标签目录 |
| archive_dir | 归档目录 |
| category_dir | 分类目录 |
| code_dir | 代码下载目录 |
| i18n_dir | 国际化目录 |
| skip_render | 跳过渲染的文件或文件夹 |
| - | - |
| new_post_name | 新文章的文件名格式 |
| default_layout | 默认的文章布局 |
| titlecase | 是否将文章标题转换为标题大小写 |
| external_link | 外部链接的设置，如是否在新标签页中打开 |
| filename_case | 文件名大小写的设置 |
| render_drafts | 是否渲染草稿 |
| post_asset_folder | 是否启用文章资源文件夹 |
| marked | Markdown解析器的设置 |
| relative_link | 是否启用相对链接 |
| future | 是否启用未来日期的文章 |
| syntax_highlighter | 代码高亮设置 |
| highlight | highlight.js的设置 |
| prismjs | Prism.js的设置 |
| - | - |
| index_generator | 博客索引页面的生成设置，如路径、每页显示的文章数和文章排序方式 |
| - | - |
| default_category | 默认分类 |
| category_map | 分类的映射 |
| tag_map | 标签的映射 |
| - | - |
| meta_generator | 是否由Hexo生成的元数据 |
| - | - |
| date_format | 日期的显示格式 |
| time_format | 时间的显示格式 |
| updated_option | 控制更新时间的显示方式 |
| - | - |
| per_page | 每页显示的文章数 |
| pagination_dir | 分页目录的格式 |
| - | - |
| include | 包含的文件或文件夹 |
| exclude | 排除的文件或文件夹 |
| ignore | 忽略的文件或文件夹 |
| - | - |
| theme | 使用的主题 |
| - | - |
| deploy | 部署的设置，包括部署类型、仓库地址和分支名称 |


# 总结
1、需要安装 `Node.js` 和 `Git`；
2、`GitHub` 仓库名必须为你的 `GitHub用户名` + `.github.io`，名字含大写字母需转为小写；
3、「原始的博客工程文件」和「生成的静态网站文件」分别部署到不同分支，主要是为了方便管理。当然你完全可以一起提交到同一分支；


# 参考
[1、Hexo 官方文档](https://hexo.io/zh-cn/docs/)

[2、为 Github Pages 站点配置发布源](https://docs.github.com/en/pages/getting-started-with-github-pages/configuring-a-publishing-source-for-your-github-pages-site)