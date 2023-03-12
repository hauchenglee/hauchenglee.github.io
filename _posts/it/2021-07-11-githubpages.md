---
layout: post
title: Github Pages
category: it
tags: [note]
---

## Introduction

为何选择github：

优点：
- 定制性强，属于静态网站
- 学习git、web等开发与使用

缺点：
- 麻烦：因为要在本地编辑然后部署代码，无论如何也比不上在网页上编辑一下就发出去的快速
- 开发难度高：有一定的开发难度，学习周期长，需要有一定的web开发经验与知识
- 百度SEO差：google上还好，github限制了百度的索引

Ref: [如何开始写技术博客，怎么选择？ - 知乎](https://www.zhihu.com/question/24629410){:target="_blank"}

## Install

Jekyll: 是一个静态网站生成器（官方推荐）

Step:
- Install ruby: [Download Ruby](https://www.ruby-lang.org/en/downloads/){:target="_blank"}
- install jekyll: `gem install jekyll`
- change directories to your app, THEN run: `bundle install`

error message:
- Could not locate Gemfile: [ruby on rails - bundle install returns "Could not locate Gemfile" - Stack Overflow](https://bit.ly/3kIsxIL){:target="_blank"}
- Could not find 'bundler': [Why do I get `Could not find 'bundler' (1.17.3)` after upgrading Ruby? - Stack Overflow](https://bit.ly/31VvWwe){:target="_blank"}

## Test

1. Open Git Bash.

2. Navigate to the publishing source for your site. For more information about publishing sources, see "[About GitHub Pages](https://docs.github.com/en/pages/getting-started-with-github-pages/about-github-pages#publishing-sources-for-github-pages-sites){:target="_blank"}."

3. Run `bundle install`.

4. Run your Jekyll site locally.

```$ bundle exec jekyll serve```

```
 Configuration file: /Users/octocat/my-site/_config.yml
            Source: /Users/octocat/my-site
       Destination: /Users/octocat/my-site/_site
 Incremental build: disabled. Enable with --incremental
      Generating...
                    done in 0.309 seconds.
 Auto-regeneration: enabled for '/Users/octocat/my-site'
 Configuration file: /Users/octocat/my-site/_config.yml
    Server address: http://127.0.0.1:4000/
  Server running... press ctrl-c to stop.
```

To preview your site, in your web browser, navigate to http://localhost:4000.

Ref: [Testing your GitHub Pages site locally with Jekyll - GitHub Docs](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll){:target="_blank"}

## Reference

Jekyll：
- [安装 - Jekyll • 简单静态博客网站生成器](https://jekyllcn.com/docs/installation/){:target="_blank"}
- [Jekyll 语法简单笔记](http://github.tiankonguse.com/blog/2014/11/10/jekyll-study.html){:target="_blank"}

教程：
- [GitHub Pages 搭建教程 - 少数派](https://sspai.com/post/54608){:target="_blank"}
- [搭建一个免费的，无限流量的Blog----github Pages和Jekyll入门 - 阮一峰的网络日志](http://www.ruanyifeng.com/blog/2012/08/blogging_with_jekyll.html){:target="_blank"}
- [Github Pages + jekyll 全面介紹極簡搭建個人網站和部落格 - IT閱讀](https://www.itread01.com/iqhklc.html){:target="_blank"}

模板与主题：
- [怎样做一个漂亮的 GitHub Pages 首页？ - 知乎](https://www.zhihu.com/question/20376047){:target="_blank"}
- [Jekyll Themes - jekyllthemes.dev](https://jekyllthemes.dev/){:target="_blank"}
- [Jekyll Themes - jekyllthemes.org](http://jekyllthemes.org/){:target="_blank"}
- [TeXt Theme - TeXt Theme](https://tianqi.name/jekyll-TeXt-theme/){:target="_blank"}

---
