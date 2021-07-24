---
title: HEXO博客备份
date: 2021-07-24 12:46:48
tags:
    - 博客
    - hexo
categories: blog
highlight_shrink: false
---
### 前言
少年不知备份好啊，突然心血来潮将电脑系统重做了，自信满满的将一些文件copy到了u盘里（^_^）,此时的我还沉浸在“新电脑”的狂喜之中。一天之后准备记录一下最近的学习和生活，没料到拷贝到的文件有丢失(QAQ)。导致只能重新建站。

所以在这里记录一下备份博客的全过程。

### 备份
#### 瞅瞅我们需要备份的文件
一下是备份的目录结构：
```
.
├── _config.yml
├── db.json
├── node_modules
├── package.json
├── package-lock.json
├── public
├── scaffolds
├── source
└── themes
    └── Butterfly
```
以上文件中的内容分别为：
1. `_config.yml`文件: hexo 的配置文件，主题等插件配置需要在该文件中修改。
2. `node_modules`文件夹: 这个就不过多解释了，依赖的包都在该文件夹中。
3. `package,json`文件: 所有依赖的包信息都写在这里。
4. `packge-lock.json`文件: 依赖模块安装记录。详情见：![npm-package-locks](https://www.npmjs.cn/files/package-locks/)
5. `public`文件夹: 包含一些生成的网页静态文件。
6. `scaffolds`文件夹: 包含创建的文章、分类、标签界面的模板。博客的定制修改会对模板进行修改。
7， `source` 文件夹: 我们的心血都在这里。
8. `themes` 文件夹: 其中butterfly是我们的主题。

参考Hexo 初始化官方的备份列表。仓库地址为![hexojs/hexo-starter](https://github.com/hexojs/hexo-starter)
```
scaffolds
source
themes
.gitignore
.gitmodules
_config.yml
package.json
```
对比一下，它丢弃了：
1. `node_modules` & `package-lock.json`,这两部分的内容，只要保留`package.json`,执行`npm install`安装即可。
2. `public`文件夹通过执行`hexo g`就可以生成网页内容。
可以重新生成的文件都可以不上传，因为使用特定命令就可以将其恢复。 
   
它增加了 .gitmodules，那它的作用又是什么呢？其实 hexojs/hexo-starter 是通过 Git 的 Submodule 功能来下载主题模块，本身仓库并不备份主题文件。考虑下我们需要如何备份主题文件目录，有两个方案：

1. 一个方案是将其内容全部上传进行备份，这样可以保证原主题的更新不会影响你原先配置的效果。
2. 另一个方案是像 hexo-starter 仓库一样通过 Git 的 Submodule 功能来管理，这样可以进行主题的更新。

这里我选择和 Hexo 初始化仓库一样使用 Git 子模块的方式进行主题的备份处理。不过通过子模块管理的方式我们恢复时仅会同步到 Next 主题的原文件，没法直接同步我们对主题配置文件或其他文件的修改，因为我们没有权限提交修改到 Next 仓库中。因此我创建了了个 themes_custom 文件夹来存放对应主题的修改的备份，这样子我们同步之后，只需要对比一下内容手动把这些配置应用过去就可以快速完成对主题的配置。

最终备份文件列表如下：
```
scaffolds
source
themes
themes_custom/butterfly
.gitignore
.gitmodules
_config.yml
package.json
```
#### 备份流程
1. 先修改 .gitignore 文件，查看之后由于原文件已经忽略了 public 和 node_modules 文件夹，因此仅需要添加 package-lock.json 到忽略清单中。

2. 我们可以删除不使用的主题或者把主题路径添加到忽略列表中。

3. 创建 themes_custom/butterfly 文件夹，将对主题进行的配件修改的文件拷贝一份到这里

4. 执行以下命令，在本地创建备份仓库：
    ```
    
    ```