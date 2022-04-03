## Guide

You can write regular [markdown](http://markdowntutorial.com/) here and Jekyll will automatically convert it to a nice webpage.  I strongly encourage you to [take 5 minutes to learn how to write in markdown](http://markdowntutorial.com/) - it'll teach you how to transform regular text into bold/italics/headings/tables/etc.

There's actually a lot more to Markdown than this. See the official [introduction][4] and [syntax][5] for more information. However, be aware that this is not using the official implementation, and this might work subtly differently in some of the little things.

[3]: http://www.markitdown.net/
[4]: http://daringfireball.net/projects/markdown/basics
[5]: http://daringfireball.net/projects/markdown/syntax

## Link

URLs can be made in a handful of ways:

* A named link to [MarkItDown][3]. The easiest way to do these is to select what you want to make a link and hit `Ctrl+L`.
* Another named link to [MarkItDown](http://www.markitdown.net/)
* Sometimes you just want a URL like <http://www.markitdown.net/>.
* go to section heading in the current page: [txt](#an-h2-header)
* Here's a footnote [^1].

[^1]: Some footnote text.

<br>

target blank: (not recommend)

[](){:target="_blank"}

## video

### youtube link

link

[Embed youtube to markdown, GitLab, GitHub](http://embedyoutube.org/){:target="_blank"}

<br>

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/0HTAKT-JIaA/0.jpg)](https://www.youtube.com/watch?v=0HTAKT-JIaA){:target="_blank"}

### youtube iframe

https://www.classynemesis.com/projects/ytembed/

<iframe src="https://www.youtube.com/embed/SxwudSpWDZ0" width="560" height="315" frameborder="0"></iframe>

### bilibili iframe

一般：

嵌入代码：

<iframe src="//player.bilibili.com/player.html?aid=18979019&bvid=BV1GW411H7X4&cid=30953782&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>

style：

style="width: 100%; height: 500px; text-align: center; padding: 20px 0;"

参数：&as_wide=1&high_quality=1&danmaku=0
- &page=1
- &as_wide=1
- &high_quality=1
- &danmaku=0

全部：

<iframe src="//player.bilibili.com/player.html?aid=18979019&bvid=BV1GW411H7X4&cid=30953782&page=1&as_wide=1&high_quality=1&danmaku=0" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true" style="width: 100%; height: 500px; text-align: center; padding: 20px 0;"> </iframe>

Ref:
- [哔哩哔哩视频显示在Github的Markdown博客页方法 - 哔哩哔哩](https://www.bilibili.com/read/cv3204833)
- [Bilibili 视频适应页面宽度 - 浩仔博客](https://sunete.github.io/tutorial/bilibili-video-adapts-to-the-width/)
- [Wordpress网页直接插入bilibili视频方法 - 哔哩哔哩](https://www.bilibili.com/read/cv4646159)

## Image

![](https://hauchenglee.github.io/assets/images//)

![](https://hauchenglee.github.io/assets/images//)

## Reference

> data
>
> Ref: []()

Ref: []()

Ref:
- []()
- []()

Reference:
- []()

## Comment

[//]: <>

## FENCED CODE BLOCKS

Here's a code chunk:

~~~
var foo = function(x) {
  return(x + 5);
}
foo(3)
~~~

And here is the same code with syntax highlighting:

```javascript
var foo = function(x) {
  return(x + 5);
}
foo(3)
```

And here is the same code yet again but with line numbers:

{% highlight javascript linenos %}
var foo = function(x) {
  return(x + 5);
}
foo(3)
{% endhighlight %}

here is console log: use `console` instead of `shell script`

```shell script
ERROR! NOT Use

use console
```

```console
terminal
```

http://www.rubycoloredglasses.com/2013/04/languages-supported-by-github-flavored-markdown/

## Table

### table 1

<table>
    <thead>
        <tr>
            <th></th>
            <th></th>
            <th></th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td></td>
            <td></td>
            <td></td>
        </tr>
        <tr>
            <td></td>
            <td></td>
            <td></td>
        </tr>
    </tbody>
</table>

### table 2

<table>
    <thead>
        <tr>
            <th>Layer 1</th>
            <th colspan="2">水平merge</th>
        </tr>
    </thead>
    <tbody>
        <tr>
            <td rowspan="4">垂直merge</td>
            <td rowspan="2">需要冒號設定數值</td>
            <td>L3 Name A</td>
        </tr>
        <tr>
            <td>L3 Name B</td>
        </tr>
        <tr>
            <td rowspan="2">L2 Name B</td>
            <td>L3 Name C</td>
        </tr>
        <tr>
            <td>L3 Name D</td>
        </tr>
    </tbody>
</table>

|--|--|--|--|--|--|--|--|
|♜ |  |♝ |♛ |♚ |♝ |♞ |♜ |
|  |♟ |♟ |♟ |  |♟ |♟ |♟ |
|♟ |  |♞ |  |  |  |  |  |
|  |♗ |  |  |♟ |  |  |  |
|  |  |  |  |♙ |  |  |  |
|  |  |  |  |  |♘ |  |  |
|♙ |♙ |♙ |♙ |  |♙ |♙ |♙ |
|♖ |♘ |♗ |♕ |♔ |  |  |♖ |

## Boxs



## footnote



---