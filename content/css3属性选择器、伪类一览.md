+++
date = "2015-10-30T11:57:29+08:00"
menu = "main"
title = "css3属性选择器、伪类一览 by dongyu"

+++

## css3属性选择器、伪类一览 ##

> 注： 部分例子代码使用scss（啥？看不懂scss？拜拜）。
 
**属性选择器**

1.*开始子串属性选择器*：`E[attr^='value']{}`
    
    a[title^='website'] {}
    <p>one<a href="www.baidu.com" title="website"></a></p>
    <p>two<a href="www.baidu.com" title="anther website"></a></p>
解释：该规则会匹配第一个段落中的a元素，因为它的title属性字符串以单词website开头。而第二个段落a的title虽包含了website但由于不是开头单词，所以不会匹配。

2.*结束子串属性选择器*：`E[attr$='value']{}`

    a {
      &[href$='.doc'] {
        background: #fff;
      }
      &[href$='.pdf'] {
        background: red;
      }
    }
    <a href='/example.doc'>i am a doc</a>
    <a href='/example.pdf'>i am a pdf</a>
解释：该规则会匹配以相应单词结尾的元素段落,所以.doc规则会匹配第一个元素a,而.pdf会匹配第二个元素a。

3.*任意子串属性值选择器*：`E[attr*='value']{}`
    
    a[title*='website'] {}
    <p>one<a href="www.baidu.com" title="website"></a></p>
    <p>two<a href="www.baidu.com" title="anther website"></a></p>
解释：该规则会匹配两个段落元素里面的a元素，因为他们的title都包含有website

4.*多属性选择器*：`E[attr^='value'][attr$='value'] {}`

    a[href*='http://'][href$='.com'] {}
    <p>one<a href="http://www.baidu.com" title="website"></a></p>
    <p>two<a href="www.baidu.com" title="anther website"></a></p>
解释：该规则的意思是匹配属性值href以http://开头且以.com结尾的元素，故第一个元素a会被匹配

5.*普通兄弟连接符*： `E ~ F {}`

    h2 ~ p {
      font-weight: bolder;
    }
    <p>i am p1</p>
    <h2>today is good</h2>
    <p>i am p2</p>
    <p>i am p3</p>
    <blockquote>
      <p>i am p4</p>
    </blockquote>
解释：该规则匹配同一层级的位于h2元素之后的任意p元素，所以p2,p3字体会变粗。而p1不在h2之后，p4和h2不在同一层级，所以不会有变化

*特别提示： 属性选择器对属性的大小写不敏感，而且所有主流的浏览器都兼容，可以放心大胆的使用。*

**伪类**  

*1.nth-of-type与nth-child*

    div>* {
      &:nth-of-type(2) {
        color: red;
      }
      &:nth-child(2) {
        color: white;
      }
    }
    <div>
      <h2>who win?</h2>
      <p>exo</p>
      <p>or</p>
      <p>tfb</p>
    </div>
解释： 两者基本用法一致，唯一不同的地方在于元素个数不受或受元素类型影响。例子中第二个p元素文字变红，因为nth-of-type忽略了h2进行计算，而第一个元素p文字变白，因为nth-child从h2开始计算。

一些补充用法：  
匹配基数项：`E:nth-child(odd) {}`  
匹配偶数项：`E:nth-child(even) {}`  
自定义,n从1开始: `E:nth-child(2n) {}`

*2.nth-last-of-type与nth-last-child*

解释: 这两个是从元素的末尾开始计算，其余用法与用例1中一致。这里列举一个实用的例子

    //问题：假设想让最后两行的元素p文字变成红色应该怎么写？
    <div>
      <p>hahaha</p>
      <p>exo</p>
      <p>or</p>
      <p>tfb</p>
    </div>
    //解答(加一个-号使得nth-last-of-type文档树计算向前走（什么意思？n代入1，2，3，4算一算就知道了）)
    p:nth-last-of-type(-n+2) {
        color: red;
    }

*3.first-child、first-of-type、last-child、last-of-type*

first-of-type： 匹配父元素的基于类型的第一个子元素  

    div>*:first-of-type {
      color: red;
    }
    <div>
      <h2>who win?</h2>
      <p>exo</p>
      <p>or</p>
      <p>tfb</p>
    </div>
解释： 第一个p元素文字和h2元素都会变红

first-child： 匹配父元素的基于结构上的的第一个子元素  

    div>*:first-of-type {
      color: red;
    }
    <div>
      <h2>who win?</h2>
      <p>exo</p>
      <p>or</p>
      <p>tfb</p>
    </div>
解释： h2元素会变红，p元素无变化

last-child、last-of-type指的是父元素的最后一个子元素，其他与上述两者一致。

*4.only-child、only-of-child*

only-child: 匹配父元素内有且只有一个子元素项(结构上数目1)  
only-of-child： 匹配父元素内有且只有一个不同类型的子元素项(类型上数目1)

    p {
      &:only-child {
        color: red;
      }
      &:only-of-type {
        font-size: 50px;
      }
    }
    <p>i am p1</p>
    <span>i am span</span>
    <span>i am span too</span>
    <blockquote>
      <p>i am p2</p>
    </blockquote>
表现： p2文字会变为红色和50px，p1会变为50px，其余无变化  
解释：由于p1在该相同层级内类型唯一，所以only-of-type会起作用，而该层级元素个数不为1，所以p1文字大小无变化。p2在层级中无论数目或类型都唯一，所以对only-child与only-of-type都有响应。

*5.target，打开内部链接伪类*

    div {
      display: none;
      &:target {
       display: block;
      }
    }
    <a href='#demo'>clickme</a>
    <div id='demo'>haha</div>
解释： 点击clickme按钮之后haha会出现，该效果可以制作不需要依赖js的tag切换效果。

*6.empty 空元素伪类*

    div:empty {
      background: red;
      width: 100px;
      height: 100px;
    }
    <div></div>
    <div>123456</div>
解释: 该规则只会匹配第一个div元素

*7.E: not(F) { } 否定元素伪类*

    //问题： 除了第一个p元素之外其他p元素文字要变成红色怎么玩？
    <div>
      <p>i am p1</p>
      <p>i am p2</p>
      <p>i am p3</p>
      <p>i am p4</p>
      <p>i am p5</p>
    </div>
    //解答： so easy,
    p:not(:first-child) {
      color: red;
    }

*特别提示： 伪类webkit,firefox,opera和ie9+浏览器兼容（啥？旧版ie也想用？可以的，不过需要依赖js的补丁包，这里就不做介绍了）*