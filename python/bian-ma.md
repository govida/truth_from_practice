---
description: python 与 lxml 编码小结
---

# 编码

## 1. 基础

常用编码：

* ASCII：支持英文，1 byte
* GBK：支持中文，2 bytes
* UTF-8：变长编码，中文 3  bytes，英文 1 byte，用于传输，压缩空间，提高带宽
* Unicode：2 bytes，用于内存表示，转换快，空间换时间

> 故在内存中，各类字符固定以unicode表示

编码转化：

* 编码（encode）：unicode（内存）——&gt; UTF-8（硬盘）
* 解码（deocde）：unicode（内存）&lt;—— UTF-8（硬盘）

### Python2

#### str类型

```python
#coding:utf-8
s='林' #在执行时,'林'会被以coding:utf-8的形式保存到新的内存空间中
print repr(s) #'\xe6\x9e\x97' 三个Bytes,证明确实是utf-8
print type(s) #<type 'str'>
s.decode('utf-8')
# s.encode('utf-8') #报错，s为编码后的结果bytes，所以只能decode
```

#### unicode类型

```python
#coding:utf-8
s=u'林'
print repr(s) #u'\u6797'
print type(s) #<type 'unicode'>

# s.decode('utf-8') #报错，s为unicode，所以只能encode
s.encode('utf-8')
```

#### 打印终端

```python
#coding:utf-8
s=u'林' #当程序执行时，'林'会被以unicode形式保存新的内存空间中

#s指向的是unicode，因而可以编码成任意格式，都不会报encode错误
s1=s.encode('utf-8')
s2=s.encode('gbk')
print s1 #打印正常否？是
print s2 #打印正常否？否

print repr(s) #u'\u6797'
print repr(s1) #'\xe6\x9e\x97' 编码一个汉字utf-8用3Bytes
print repr(s2) #'\xc1\xd6' 编码一个汉字gbk用2Bytes

print type(s) #<type 'unicode'>
print type(s1) #<type 'str'>
print type(s2) #<type 'str'>
```

### Python3

```python
#coding:utf-8
s='林' #当程序执行时，无需加u，'林'也会被以unicode形式保存新的内存空间中,

#s可以直接encode成任意编码格式
s1=s.encode('utf-8')
s2=s.encode('gbk')

print(s) #林
print(s1) #b'\xe6\x9e\x97' 在python3中，是什么就打印什么
print(s2) #b'\xc1\xd6' 同上

print(type(s)) #<class 'str'>
print(type(s1)) #<class 'bytes'>
print(type(s2)) #<class 'bytes'>
```

相关链接：

* [python编码问题大终结](https://www.cnblogs.com/vipchenwei/p/6993788.html)
* [python编码及转换（简介明了版）](https://www.cnblogs.com/xyn123/p/8869754.html)

## 2. Requests

首先，需要明确content与text的区别：

* content：以字节码的形式展示response的body部分
* text：通过encoding或apparent\_encoding对content进行encoding后返回的unicode

现在我们来看encoding与apparent\_encoding的区别：

* encoding：根据response头文件Content-Type中的charset来识别编码，如果没有则默认为ISO-8859-1
* apparent\_encoding：通过chardet对body进行识别，推测编码

默认情况下，由于default encoding=ISO-8859-1的存在，apparent\_encoding是不会被系统采用的~

我们在利用text获得encoding的unicode时，可以手动设置encoding，使编码结果正常

相关链接：

* [Request使用指南](https://blog.csdn.net/qq_37616069/article/details/80376776)
* [Python+Requests编码识别Bug](http://liguangming.com/python-requests-ge-encoding-from-headers.html)
* [python requests的content和text方法的区别](https://blog.csdn.net/xie_0723/article/details/51361006)

## 3. lxml

### etree.HTML处理数据乱码

默认情况下，etree.HTML encoding为ISO-8859-1（此为测试结果），这种情况下，其只能处理ISO-8859-1编码的str或unicode的字符串，说明etree.HTML内部是将text先decode为unicode，再进一步处理。

这给我们两个提示：

1. 提供unicode的text供etree处理，这种情况下不会发生乱码
2. 提供str的text供etree处理，但是要声明这些text是通过什么编码得到的（encoding）

> etree.HTML\(text,parser=etree.HTMLParser\(encoding='utf-8'\)\)

注意，这里的是在解析html的第一步就设定了encoding，后续`tostring`等方法上的编码应该保持一致。

