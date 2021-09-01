## Guide

You can write regular [markdown](http://markdowntutorial.com/) here and Jekyll will automatically convert it to a nice webpage.  I strongly encourage you to [take 5 minutes to learn how to write in markdown](http://markdowntutorial.com/) - it'll teach you how to transform regular text into bold/italics/headings/tables/etc.

## Link

[](){:target="_blank"}

## Youtube

[Embed youtube to markdown, GitLab, GitHub](http://embedyoutube.org/){:target="_blank"}

<br>

[![IMAGE ALT TEXT HERE](https://img.youtube.com/vi/0HTAKT-JIaA/0.jpg)](https://www.youtube.com/watch?v=0HTAKT-JIaA){:target="_blank"}

## Image

![](https://www.hauchenglee.com/assets/images//)

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

## Boxs

