title: Static Blog Setup with Github Pages and Hexo
date: 2015-03-29 22:00
categories: [Others]
tags: [tutorial, hexo]

---
## Foreword

In this tutorial you will set up a static blog with Github pages and Hexo.

### Why Github Pages?

- Stable
- Simple
- Popular
- Why not?

### What is Hexo?

[Hexo](https://github.com/hexojs/hexo) is a simple, powerful blog framework powered by Node.js.

###Why Hexo?

In my view, great content is the soul of blog. So I choose a simple way like Hexo to set up my blog, and share my experience with you.

<!-- more -->

## Start to set up your blog

### Install [Git](http://git-scm.com/) and [Node.js](https://nodejs.org/)

If you use Mac, you just need:

```
$ brew update  
$ brew install git  
$ brew install nvm
```

Add

```
$ source ~/usr/local/Cellar/x.x.x/nvm.sh
```

to the `.bashrc` or `.bash_profile` or `.zshrc` etc.  
You can use

```
$ nvm ls-remote
```

to list all the versions of node or io.

```
$ nvm install <node's version like v0.12.7>
```

Use

```
$ nvm ls
```

to list all your versions of node or iojs.  
And you can use

```
$ nvm use <node's version like 0.12.7>
```

to choose the version. If you are lazy like me. :P

```
$ nvm use 0.12
```

So easy!

If you are in the GFW, you can use `--registry` param to change your nvm and npm source. For Chinese, you can use [taobao](http://registry.npm.taobao.org):

```
$ npm install koa --registry=http://registry.npm.taobao.org
```

Or you can also install cnpm cli:

```
$ npm install cnpm -g --registry=http://registry.npm.taobao.org
```

### Install and init [Hexo](https://github.com/hexojs/hexo)

#### Install

```
$ npm install -g hexo
```
#### Init

Create a new hexo project in a folder:

```
$ hexo init <your_project_name>
$ cd <your_project_name>
$ npm install
```

Check it with:

```
$ hexo clean  
$ hexo generate  
$ hexo server
```

See the effect in your browser with the url of `localhost:4000`

###Deploy GitHub Pages

####Create a new repo

You can choose:

- your_user_name.github.com: one user can have only one this repo, and you can push to the **'master'** branch.
- Project repo, you should push to the gh-pages branch.

###Configure your Hexo

####Use your own domain

If you want to use your own domain, you should create a file named **'CNAME'**. And put it in `hexo folder/source` root. Or just use default.

####Giving the website a name, blog author, email, etc

Edit the site section of the `hexo folder/_config.yml` file:

```
# Site
title: Liao Yifan's Strive
subtitle:       —— Talk is cheap, just do it!
description: Liao Yifan's blog
author: Liao Yifan
language: default
```
####Configuring a public web address (URL)
Edit the URL section:

```
# URL
## If your site is put in a subdirectory, set url as 'http://yoursite.com/child' and root as '/child/'
url: http://saintdan.github.io/
root: /
permalink: :year/:month/:day/:title/
```

####Enabling comments

Hexo blog sites use [Disqus](https://disqus.com/) for managing comments. Disqus is a common service use to manage comments for lots of different blog platforms.

```
# Disqus
disqus_shortname: <your_disqus_shortname>
```

####Configure deploy information

```
# Deployment
## Docs: http://hexo.io/docs/deployment.html
deploy:
  type: git
  repository: <your_repository> (e.g.:git@github.com:saintdan/saintdan.github.io.git)
  branch: master
```

**you can use other configuration as their default, or edit as you wish, it's upto you.**

####Use theme

If you don't want to create your own hexo theme, you can choose the existing [themes](https://github.com/hexojs/hexo/wiki/Themes).  
BTW, I used [NexT](https://github.com/iissnan/hexo-theme-next) theme. "Less is more". :P  
Modify hexo root file `config.yml`:

```
theme: <your_theme_name>
```

###Deploy Hexo

You can use:

```
$ hexo clean
$ hexo generate
$ hexo deploy
```

to deploy it.  
See the effect in your browser with your blog URL.  

**Thanks for reading and happy coding.**