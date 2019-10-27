---
description: 爬虫解析常用
---

# lxml速查

## xpath基本规则

| 表达式 | 描述 |
| :--- | :--- |
| . | 当前节点 |
| .. | 当前节点的父节点 |
| / | 当前节点的直接子节点 |
| // | 当前节点的子孙节点 |
| \[@attrib='value'\] | 选取属性具有给定值的节点 |
| text\(\) | 获得当前节点text的文本内容（紧跟当前节点的文本内容） |
| string\(\) | 获得当前节点的所有文本内容（无论在什么位置，只要是该节点内的文本） |
| \[contains\(@attrib,'value'\)\] | 选取属性中含指定值的节点 |

## node基本操作

| 属性 | 描述 |
| :--- | :--- |
| .tag | 标签名称 |
| .text | 获得当前节点text的文本内容（紧跟当前节点的文本内容） |
| .tail | 获得节点后面的文本（只是文本，包括空白符，不算节点） |
| .is\_text | 判断是否存在text |
| .is\_tail | 判断是否存在tail |
| .getparent\(\) | 获得父节点 |
| .getprevious\(\) | 获得前一个兄弟节点 |
| .getnext\(\) | 获得后一个兄弟节点 |
| .get\('attrib'\) | 获得节点的attrib对应的具体值（节点的属性本质上是个字典） |
| .keys\(\) | 获得节点的所有属性 |
| .values\(\) | 获得节点的所有属性对应的值 |
| .iter\('tag1','tag2',\) | 迭代所有tag节点，默认不填迭代所有 |

## 序列化

> etree.tostring\(node,method='text',with\_tail=True,encoding='utf-8'\)

* method：text只打印文本，xml打印节点tag以及文本，节点内的所有文本都会被打印，区别于node.text
* with\_tail：默认打印node.tail

## DEMO

```python
# coding:utf-8
from lxml import etree

text = '''
<div>
    <ul>
         <li class="item-0">aa<a href="link1.html">第一个</a>xxx</li>
         <li class="item-1  a"><a href="link2.html">second item</a></li>
         <li class="item-0 b" name='xx'>bbb<a href="link5.html">a属性<b>ssss</b>aa</a>xxx</li>aaa
     </ul>
 </div>
'''
html = etree.HTML(text, etree.HTMLParser(encoding='utf-8'))

result = html.xpath("//*")
print result

result = html.xpath("//li")
print result

result = html.xpath("//li/a[@href='link5.html']")[0]
print result.text, result.tag, result.getparent(), result.getprevious(), result.getnext()

result = html.xpath("//li[@class='item-0']")[0]
print result.text, result.tag, result.getparent(), result.getprevious(), result.getnext()

result = html.xpath("//li/text()")
print result

result = html.xpath("//li[contains(@class,'b') and @name='xx']")

print result[0].tail

print etree.tostring(result[0],method='text',with_tail=False,encoding='utf-8')
```

## 解析CDATA

parser默认会丢弃**&lt;!\[cdata\[......\]\]&gt;**的标签和其中的内容，但是在解析微信数据时，就会有问题

```python
parser = etree.xmlparser(strip_cdata=false)
root = etree.fromstring(str_xml, parser) 
```

[lxml处理CDATA的备忘](http://www.makaidong.com/%E5%8D%9A%E5%AE%A2%E5%9B%AD%E7%83%AD/34970.shtml)

## 参考资料

* [逝者如斯，而未尝往也](https://www.cnblogs.com/zhangxinqi/p/9210211.html#_label8)
* [XPath中的text\(\)和string\(\)区别](https://blog.csdn.net/weixin_39285616/article/details/78463091)
* [lxml模块的用法](https://www.jianshu.com/p/8321702bddc6)





