---
layout: docs
title: How to create article contents by js
subtitle: 如何动态生成文章目录
categories: note
tags: javascript
---

在搭建此次博客时，很想用javascript生成文章的目录，方便阅读，本来自己写了一个，不过看到[BeiYuu](http://beiyuu.com/)的文章[《我为什么写博客？》](http://beiyuu.com/why-blog/)的文章目录时，感觉自己写得没有他的好，于是参考他写的，写了下面的文章内容。

## 动态生成文章目录过程 ##
### 编程思路 ###
总体上，思路就是你想的那个样子：
1.  当页面加载完毕之后，使用js将h1, h2, h3...等标签中的内容和id（原文作者记录的是标题的位置）提取出来，可以存入数组。
2.  设置相应的目录的容器，说白了就是相应的HTML,CSS。
3.  使用js设置HTML a rel Attribute,不过这里原文作者是直接记录标题的位置，而不是使用href="#{id}",不过这点细节没有什么大的影响。将数组的内    容放入相应的位置。

### js部分代码 ###
由于js部分有些许长，这里直接给出我repo中的
[生成目录JS代码](https://github.com/creamidea/creamidea.github.com/blob/master/_includes/article_contents.html).
下面只是贴出如何提取出标题内容的js代码
{% highlight js %}
$.each($('h2,h3'),function(index,item){
    if(item.tagName.toLowerCase() == 'h2'){
	  var h2item = {};
	  h2item.name = $(item).text();
	  h2item.id = 'menuIndex'+index;
	  h2.push(h2item);
	  h2index++;
    }else{
	  var h3item = {};
	  h3item.name = $(item).text();
	  h3item.id = 'menuIndex'+index;
	  if(!h3[h2index-1]){
	    h3[h2index-1] = [];
	  }
	  h3[h2index-1].push(h3item);
    }
    item.id = 'menuIndex' + index
});
{% endhighlight %}
<div class="note warning">
  <strong>从代码上可以看出，必须要一个二级标题,markdown: "##  ##", HTML标签："h2",这点需要记住！</strong>
</div>

### css部分代码 ###
{% highlight css %}
/* Article Contents */
#menuIndex {
    position: fixed;
    bottom: 88px;
    right: 20px;
    width: 200px;
    overflow: auto;
    max-height: 602px;
}

#menuIndex ul {
    list-style: none;
}

#menuIndex ul li {
    font-size: 12px;
    margin-bottom: 5px;
    word-wrap: break-word;
    padding-left: 10px;
}

#menuIndex li.h1 {
    font-size: 14px;
    font-weight: normal;
    padding-left: 0;
    margin-bottom: 10px;
}

#menuIndex li.h3 {
    padding-left: 25px;
}

#menuIndex ul li.on {
    /*这里设置当前的标签的背景颜色，请根据自己的博客主色调具体情况修改*/
    background-color: rgb(37, 28, 28); 
}
{% endhighlight %}

你有任何问题，都可以{% include comments_email.html %},