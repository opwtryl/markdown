# CSS XPATH  BeautifulSoup JSONPATH总结

## css

[学习资料：MDN CSS SELECTOR](https://developer.mozilla.org/en-US/docs/Web/CSS/CSS_Selectors)

<table>
    <tr>
        <td>类型</td>
        <td>语法</td>
        <td>解释</td>
    </tr>
    <tr>
        <td>all</td>
        <td>*</td>
        <td>所有元素</td>
    </tr>
    <tr>
        <td>element</td>
        <td>div</td>
        <td>标签为 div 的元素</td>
    </tr>
    <tr>
        <td>class</td>
        <td>.first</td>
        <td>class="first" 的元素</td>
    </tr>
    <tr>
        <td>id</td>
        <td>#password</td>
        <td>id="password" 的元素</td>
    </tr>
    <tr>
        <td rowspan="7">attribute</td>
        <td>[attr]</td>
        <td>包含“attr”属性，不管它的值是什么</td>
    </tr>
    <tr>
        <td>[attr=val]</td>
        <td>属性attr="val"</td>
    </tr>
    <tr>
        <td>[attr~=val]</td>
        <td>attr="first val other"，注意是空格隔开</td>
    </tr>
    <tr>
        <td>[attr^=val]</td>
        <td>attr="value" "val"开头</td>
    </tr>
    <tr>
        <td>[attr$=val]</td>
        <td>attr="firstval" "val"结尾 </td>
    </tr>
    <tr>
        <td>[attr*=val]</td>
        <td>attr="firstvalue" 包含“val” </td>
    </tr>
    <tr>
        <td>[attr|=val]</td>
        <td>attr="val"或者attr="val-first" attr等于“val”或者以“val-”开头 </td>
    </tr>
    <tr>
        <td rowspan="5">relationship</td>
        <td>A B</td>
        <td>A的所有后代元素里面的B元素</td>
    </tr>
    <tr>
        <td>A,B</td>
        <td>A或B元素</td>
    </tr>
    <tr>
        <td>A>B</td>
        <td>A的子元素里面的B元素</td>
    </tr>
    <tr>
        <td>A+B</td>
        <td>A的下一个兄弟B元素</td>
    </tr>
    <tr>
        <td>A~B</td>
        <td>A后面的所有兄弟B元素</td>
    </tr>
    <tr>
        <td rowspan="8">Pseudo-class</td>
        <td>:first-child</td>
        <td>父节点的第一个子节点，注意不是选择当前节点的第一个子节点</td>
    </tr>
    <tr>
        <td>:first-of-type</td>
        <td>属于兄弟元素中其类型的第一个元素</td>
    </tr>
    <tr>
        <td>:matches()</td>
        <td>符合列表中任意一个选择器</td>
    </tr>
    <tr>
        <td>:not()</td>
        <td>不符合列表中任意一个选择器</td>
    </tr>
    <tr>
        <td>:nth-child()</td>
        <td>:nth-child(1)第1个子元素,:nth-child(2n+1) 第奇数个子元素</td>
    </tr>
    <tr>
        <td>:nth-of-type()</td>
        <td>兄弟元素中其类型的第n个元素</td>
    </tr>
    <tr>
        <td>:only-child</td>
        <td>属于某个父元素的唯一一个子元素</td>
    </tr>
    <tr>
        <td>:only-of-type</td>
        <td>元素没有其他相同类型的兄弟元素</td>
    </tr>
    <tr>
        <td rowspan="5">Pseudo-element</td>
        <td>::after</td>
        <td>创建一个伪元素，作为已选中元素的最后一个子元素。</td>
    </tr>
    <tr>
        <td>::before</td>
        <td>创建一个伪元素，作为已选中元素的第一个子元素。</td>
    </tr>
    <tr>
        <td>::first-letter</td>
        <td>已选中元素的第一个字符</td>
    </tr>
    <tr>
        <td>::first-line</td>
        <td>已选中元素的第一行</td>
    </tr>
    <tr>
        <td>::selection</td>
        <td>用户鼠标选择的高亮部分</td>
    </tr>
</table>

## XPATH

[学习资料：MSDN XPath Syntax](https://msdn.microsoft.com/en-us/library/ms256471(v=vs.110).aspx)

[学习资料：XPath/CSS Equivalents](https://en.wikibooks.org/wiki/XPath/CSS_Equivalents)

<table>
    <tr>
        <td>分类</td>
        <td>语法</td>
        <td>解释</td>
    </tr>
    <tr>
        <td rowspan="8">特殊字符</td>
        <td>/</td>
        <td>子元素</td>
    </tr>
    <tr>
        <td>//</td>
        <td>后代元素</td>
    </tr>
    <tr>
        <td>.</td>
        <td>当前元素</td>
    </tr>
    <tr>
        <td>..</td>
        <td>父元素</td>
    </tr>
    <tr>
        <td>*</td>
        <td>所有元素</td>
    </tr>
    <tr>
        <td>@</td>
        <td>获取元素属性名称，或获取包含某属性的元素</td>
    </tr>
    <tr>
        <td>()</td>
        <td>表达式</td>
    </tr>
    <tr>
        <td>[]</td>
        <td>过滤器，也可以表示索引</td>
    </tr>
    <tr>
        <td rowspan="10">操作符</td>
        <td>and</td>
        <td>与</td>
    </tr>
    <tr>
        <td>or</td>
        <td>或</td>
    </tr>
    <tr>
        <td>not()</td>
        <td>非</td>
    </tr>
    <tr>
        <td>=</td>
        <td>等于</td>
    </tr>
    <tr>
        <td>!=</td>
        <td>不等于</td>
    </tr>
    <tr>
        <td>&lt; *</td>
        <td>小于</td>
    </tr>
    <tr>
        <td>&lt;= *</td>
        <td>小于或等于</td>
    </tr>
    <tr>
        <td>&gt; *</td>
        <td>大于</td>
    </tr>
    <tr>
        <td>&gt;= *</td>
        <td>大于或等于</td>
    </tr>
    <tr>
        <td>|</td>
        <td>并集</td>
    </tr>
    <tr>
        <td rowspan="12">节点轴</td>
        <td>ancestor::</td>
        <td>祖先元素</td>
    </tr>
    <tr>
        <td>ancestor-or-self::</td>
        <td>祖先元素或自身</td>
    </tr>
    <tr>
        <td>attribute::</td>
        <td>获取所有属性值</td>
    </tr>
    <tr>
        <td>child::</td>
        <td>子元素</td>
    </tr>
    <tr>
        <td>descendant::</td>
        <td>后代元素</td>
    </tr>
    <tr>
        <td>descendant-or-self::</td>
        <td>后代元素或自身</td>
    </tr>
    <tr>
        <td>following::</td>
        <td>当前节点后面的所有节点</td>
    </tr>
    <tr>
        <td>following-sibling::</td>
        <td>当前节点后面的所有兄弟节点</td>
    </tr>
    <tr>
        <td>parent::</td>
        <td>父节点</td>
    </tr>
    <tr>
        <td>preceding::</td>
        <td>当前节点前面的所有节点</td>
    </tr>
    <tr>
        <td>preceding-sibling::</td>
        <td>当前节点前面的所有兄弟节点</td>
    </tr>
    <tr>
        <td>self::</td>
        <td>当前节点</td>
    </tr>
    <tr>
        <td rowspan="18">函数</td>
        <td>count()</td>
        <td>满足条件的元素的个数</td>
    </tr>
    <tr>
        <td>last()</td>
        <td>满足条件的最后一个元素</td>
    </tr>
    <tr>
        <td>position()</td>
        <td>满足条件的第n个元素</td>
    </tr>
    <tr>
        <td>concat</td>
        <td>连接字符串</td>
    </tr>
    <tr>
        <td>contains</td>
        <td>符合某条件的元素</td>
    </tr>
    <tr>
        <td>normalize-space</td>
        <td>清除元素的前后空格</td>
    </tr>
    <tr>
        <td>starts-with</td>
        <td>以某字符串开头</td>
    </tr>
    <tr>
        <td>string</td>
        <td>转换成字符串</td>
    </tr>
    <tr>
        <td>string-length</td>
        <td>返回字符串长度</td>
    </tr>
    <tr>
        <td>substring</td>
        <td>按索引截取字符串</td>
    </tr>
    <tr>
        <td>substring-after</td>
        <td>截取某子字符串后面的字符串</td>
    </tr>
    <tr>
        <td>substring-before</td>
        <td>截取某子字符串前面的字符串</td>
    </tr>
    <tr>
        <td>translate</td>
        <td>把字符串的字符一一转换</td>
    </tr>
    <tr>
        <td>ceiling</td>
        <td>大于等于n的最小整数</td>
    </tr>
    <tr>
        <td>floor</td>
        <td>小于等于n的最大整数</td>
    </tr>
    <tr>
        <td>number</td>
        <td>转换成数值</td>
    </tr>
    <tr>
        <td>round</td>
        <td>正数四舍五入取整，负数 大于或等于-0.5归零</td>
    </tr>
    <tr>
        <td>sum</td>
        <td>求和</td>
    </tr>
    <tr>
    </tr>
</table>

## BeautifulSoup

[学习资料：Beautiful Soup 文档](https://www.crummy.com/software/BeautifulSoup/bs4/doc.zh/index.html)

<table>
    <tr>
        <td>功能</td>
        <td>语法</td>
        <td>解释</td>
    </tr>
    <tr>
        <td rowspan="16">遍历文档树</td>
        <td>.contents</td>
        <td>列表方式表示子节点</td>
    </tr>
    <tr>
        <td>.children</td>
        <td>生成器方式表示子节点</td>
    </tr>
    <tr>
        <td>.descendants</td>
        <td>子孙节点</td>
    </tr>
    <tr>
        <td>.string</td>
        <td>如果只有一个子节点，输出子节点的文本。否则返回None。</td>
    </tr>
    <tr>
        <td>.strings</td>
        <td>生成器方式表示多个子孙节点的文本</td>
    </tr>
    <tr>
        <td>.stripped_strings</td>
        <td>生成器方式表示多个子孙节点的文本，去除空白内容</td>
    </tr>
    <tr>
        <td>.parent</td>
        <td>父节点</td>
    </tr>
    <tr>
        <td>.parents</td>
        <td>祖先节点</td>
    </tr>
    <tr>
        <td>.next_sibling</td>
        <td>下一个兄弟节点</td>
    </tr>
    <tr>
        <td>.previous_sibling</td>
        <td>上一个兄弟节点</td>
    </tr>
    <tr>
        <td>.next_siblings</td>
        <td>后面所有兄弟节点</td>
    </tr>
    <tr>
        <td>.previous_siblings</td>
        <td>前面所有的兄弟节点</td>
    </tr>
    <tr>
        <td>.next_element</td>
        <td>下一个被解析的对象(字符串或tag),需要与next_sibling区分</td>
    </tr>
    <tr>
        <td>.previous_element</td>
        <td>上一个被解析的对象(字符串或tag),需要与previous_sibling区分</td>
    </tr>
    <tr>
        <td>.next_elements</td>
        <td>生成器方式表示后面所有被解析的对象</td>
    </tr>
    <tr>
        <td>.previous_elements</td>
        <td>生成器方式表示前面所有被解析的对象</td>
    </tr>
    <tr>
        <td rowspan="3">搜索文档树</td>
        <td>.select</td>
        <td>按 CSS 搜索</td>
    </tr>
    <tr>
        <td>.find</td>
        <td>当前tag的第一个子节点</td>
    </tr>
    <tr>
        <td>.find_all</td>
        <td>当前tag的所有子节点</td>
    </tr>
    <tr>
    </tr>
</table>

## jsonpath

暂时没找到支持 jsonpath过滤器的 python库，有个 objectpath 可以达到部分效果。

[学习资料：jsonpath语法 https://goessner.net/articles/JsonPath/index.html](https://goessner.net/articles/JsonPath/index.html)

[学习资料：python jsonpath-rw文档](https://github.com/kennknowles/python-jsonpath-rw)

[学习资料：python objectpath 文档](http://objectpath.org/reference.html)

语法|解释
---|---|
$|根节点
@|当前节点
. or []|子节点
..|后代节点
*|所有节点
[,]|并集
[start: end :step]|切片
?()|过滤器
()|表达式

## 实践

```python
# 解析html文档

from requests_html import HTML
from bs4 import BeautifulSoup

html_doc = """
<html>
<head><title>The Dormouse's story</title></head>
<body>
<p class="title"><b>The Dormouse's story</b></p>
<p class="story">Once upon a time there were three little sisters; and their names were
<a href="http://example.com/elsie" class="sister" id="link1">Elsie</a>,
<a href="http://example.com/lacie" class="sister" id="link2">Lacie</a> and
<a href="http://example.com/tillie" class="sister" id="link3">Tillie</a>;
and they lived at the bottom of a well.
</p>
<p class="story">...</p>
</body>
</html>
"""

html = HTML(html=doc)
soup=BeautifulSoup(doc)

# 找出所有的 p标签节点，其中 html.find() ，soup.select()是按照css语法搜索文档的，soup("p)是 soup.find_all("p")的简写
html.find("p"),html.xpath("//p"),soup.find_all("p"),soup("p"),soup.select("p")

# 找出所有 class='sister'的节点，注意 BeautifulSoup 里面用的是 class_
html.find('.sister'),html.xpath("//*[@class='sister']"),soup.find_all(class_='sister')

# 按 id 搜索
html.find('#link1'),html.xpath("//*[@id='link1']"),soup.find_all(id='link1')

# 搜索 name 参数的值可以使用任一类型的 过滤器 ,字符串,正则表达式,列表,方法或是 True .
html.find("[href]"),html.xpath("//*[@href]"),soup.find_all(href=True)


html.find("[href^='http']"),html.xpath("//*[starts-with(@href,'http')]"),soup.find_all(href=re.compile("^http"))

html.find("[href*='example']"),html.xpath("//*[contains(@href,'example')]"),soup.find_all(href=re.compile('example'))

html.find("[href$='tillie']"),html.xpath("//*[substring(@href,string-length(@href)-5)='tillie']"),soup.find_all(href=re.compile('tillie$'))

html.find("p b"),html.xpath("//p//b")

html.find("p,b"),html.xpath("//*[self::p or self::b]")

html.find("p.title + p"),html.xpath("//p[@class='title']/following-sibling::p[1]")

html.find("p.title ~ p"),html.xpath("//p[@class='title']/following-sibling::p")

#结果是 [],[<Element 'head' >]，因为 body 不是父节点的第一个子节点，所以返回 []
html.find("body:first-child")，html.find("head:first-child")

# 结果是 [<Element 'head' >]，返回节点的第一个子节点
html.find("html>*:first-child"),html.find("html>*:nth-child(1)")

# 结果是 [<Element 'title' >, <Element 'b' >]，返回 符合父节点的唯一子节点 的节点
html.find("*:only-child")

```

```python
# 解析 json文件

import jsonpath_rw
import objectpath

j={ "store": {
    "book": [ 
      { "category": "reference",
        "author": "Nigel Rees",
        "title": "Sayings of the Century",
        "price": 8.95
      },
      { "category": "fiction",
        "author": "Evelyn Waugh",
        "title": "Sword of Honour",
        "price": 12.99
      },
      { "category": "fiction",
        "author": "Herman Melville",
        "title": "Moby Dick",
        "isbn": "0-553-21311-3",
        "price": 8.99
      },
      { "category": "fiction",
        "author": "J. R. R. Tolkien",
        "title": "The Lord of the Rings",
        "isbn": "0-395-19395-8",
        "price": 22.99
      }
    ],
    "bicycle": {
      "color": "red",
      "price": 19.95
    }
  }
}
tree=objectpath.Tree(j)

# 结果都是 ['Nigel Rees', 'Evelyn Waugh', 'Herman Melville', 'J. R. R. Tolkien']，tree.execute() 返回的是生成器
[match.value for match in jsonpath_rw.parse('$.store.book[*].author').find(j)]
list(tree.execute('$.store.book.author'))

# 返回所有的 author
[match.value for match in jsonpath_rw.parse('$..author').find(j)]
list(tree.execute('$..author'))

# 返回第3本书
[match.value for match in jsonpath_rw.parse('$..book[2]').find(j)]

# 返回前两本书
[match.value for match in jsonpath_rw.parse('$..book[:2]').find(j)]

# 包含 isbn 的书
list(tree.execute('$..book[@.isbn is not None]'))

# 价格低于10的书
list(tree.execute('$..book[@.price<10]'))
```
