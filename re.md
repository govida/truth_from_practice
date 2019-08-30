# re

## 基本规则

### []

[ab12] 代表a、b、1、2都可以

[^ab12] 代表除了a、b、1、2都可以

[a-z] 代表a到z都可以

### |

[a-zA-Z]|[0-9] 代表字母或数字

[a-z|0-9] 代表字母、下划线、数字，即|在[]里无效，代表常规字符

dog|cat 代表 dog 和 cat，|是对左右两条规则（dog和cat）起效，并非字符（g|c）

a (?:dog|cat) 代表 a dog 或 a cat，而 a dog|cat 代表 a dog 或 cat，(?:)称为无捕获组

### .

默认匹配除\n外的所有字符，当在re.S下，会匹配包括\n在内的所有字符

### ^、$

- ^：匹配开头（在re.M下，会匹配每行行首部）

- $：匹配末尾（在re.M下，会匹配每行行尾部）

### \d、\D

- \d：匹配一个数字，等价[0-9]
- \D：匹配一个非数字，等价\[^0-9\]

### \w、\W

- \w：匹配一个英文字母和数字，等价[a-zA-Z0-9]
- \W：匹配一个非英文字母和数字，等价\[^a-zA-Z0-9\]

### \s、\S

- \s：匹配一个空格、制表、回车等分割有意义的字符，等价[ \t\r\n\f\v] （注意前面有个空格）
- \S：匹配一个非空格、制表、回车等分割有意义的字符，等价\[^ \t\r\n\f\v\] （注意前面有个空格）

### \A、\Z

- \A：匹配字符串的开头，与^的区别是，在re.M下，不会匹配每行行首，仍是字符串的开头

- \Z：匹配字符串的结尾，与$的区别是，在re.M下，不会匹配每行行尾，仍是字符串的结尾

### \b、\B

- \b：匹配单词边界，例如空格，与\s不同的是，在最终匹配结果中，\b匹配的内容不会在最终结果中出现，即无空格等字符
- \B：匹配非单词边界，在最终匹配结果中，\B匹配的内容不会在最终结果中出现

```python
s =  'abc abcde bc bcd'
re.findall( r'\bbc\b' , s ) 
# ['bc']   # \b，单词边界不算在最终结果，无空格字符

re.findall( r'\sbc\s' , s ) 
# [' bc '] # \s，单词边界算在最终结果，有空格字符
```

### (?:)

将一段规则作为一个整体使用，不等价于()，()代表组的概念

### (?#)

代表注释，内部内容会被忽略

### **(?iLmsux)** 

编译选项，只要出现在正则中，对整条表达式都有效

- i：代表ignorecase，忽略大小写
- L：代表local，本地化，如对于英语\w代表数字和字母，对于法语则是数字和法文，然对于中文没用。
- m：代表mutiline，多行，如^、$会在每行都可匹配（默认只有开始和结束）

- s：代表dotall，. 匹配所有符号（默认情况下 . 不匹配换行符）
- u：代表unicode
- x：代表verbose，详细模式，不常用

### *、+、？、{m}、{m,n}、{m,}

- *：0次或多次
- +：代表至少1次
- ?：代表0次或1次
- {m}：精确匹配m次
- {m,n}：至少匹配m次，至多匹配n次
- {m,}：至少匹配m次

### .*?、.+?、.??

?结尾，代表非贪婪匹配，即最小匹配

```python
s =r'/* part 1 */ code /* part 2 */'
re.findall( r'/\*.*\*/' , s )
# ['/* part 1 */ code /* part 2 */'] # 贪婪匹配
  
re.findall( r'/\*.*?\*/' , s ) 
# ['/* part 1 */', '/* part 2 */'] # 非贪婪匹配
```

### (?<=…)、(?=…)、(?<!…)、(?!…)

界定：一定要是什么才行，界定不算在最终结果内

- (?<=…)：前向界定，只能用常量，即不能用特殊的正则语法
- (?=…)：后向界定

- (?<!…)：前向非界定

- (?!…)：后向非界定

```python
s=r'/* comment 1 */  code  /* comment 2 */'
re.findall( r'(?<=/\*).+?(?=\*/)' , s )
# [' comment 1 ', ' comment 2 ']

s = 'aaa111aaa , bbb222 , 333ccc '
re.findall( r'(?<=[a-z]+)\d+(?=[a-z]+)' , s ) 
# error: look-behind requires fixed-width pattern # 前向界定，非常量，错误案例

re.findall( r'\d+(?=[a-z]+)', s )
# ['111', '333'] # 后向界定，非常量，可行

re.findall( r'\d+(?!\w+)' , s ) # 后向非界定，非常量
# ['222']
```

## 组

### （）

无命名组，只返回()包裹的部分，其他匹配部分不返回

```python
s = 'aaa111aaa , bbb222 , 333ccc '
re.findall (r'[a-z]+(\d+)[a-z]+' , s )
# ['111']
```

### (?P\<name\>…)

命名组

### (?P=name)

通过名称，调用已匹配到的命名组

```python
s='aaa111aaa,bbb222,333ccc,444ddd444,555eee666,fff777ggg'
re.findall( r'([a-z]+)\d+([a-z]+)' , s ) # 找出中间夹有数字的字母
# [('aaa', 'aaa'), ('fff', 'ggg')]

re.findall( r'(?P<g1>[a-z]+)\d+(?P=g1)' , s ) # 找出中间夹有数字,且前后两边相同的字母
# ['aaa']
```

### \number

通过序号，调用已匹配到的组（命名、未命名都可以），序号从1开始

```python
re.findall( r'(?P<g1>[a-z]+)\d+\1' , s ) # 命名组
# ['aaa']
re.findall( r'([a-z]+)\d+\1' , s ) # 未命名组
# ['aaa']
```

###**(?(** id/name)yes-pattern|no-pattern)

若前面使用指定组（利用序号id，或者名称name）的模式，则这里一定需要满足yes-pattern的模式，否则可以使用no-pattern。

```python
s='<usr1@mail1>  usr2@maill2'
re.findall( r'(<)?\s*(\w+@\w+)\s*(?(1)>)' , s ) # (?(1)>) 等价 (?(1)>|) 
# [('<', 'usr1@mail1'), ('', 'usr2@maill2')]

s='<usr1@mail1>  usr2@maill2 <usr3@mail3   usr4@mail4>  < usr5@mail5 '
# [('<', 'usr1@mail1'), ('', 'usr2@maill2'), ('', 'usr3@mail3'), ('', 'usr4@mail4'), ('', 'usr5@mail5')]
```

## 函数

### 搜索

`re.match`：从开始位置匹配，如果第一个位置失败，就放弃寻找。返回的是 match object，失败为None。

`re.search`：搜索整个字符串，找到一个就返回。成功返回的是 match object，失败为None。

`re.findall`：搜索整个字符串，返回所有找到的字符串。成功返回的是str list，失败为[]

`re.finditer`：搜索整个字符串，所有找到的 match object的迭代器。区别于`re.search`，只找到一个。

### 替换

`re.sub`：返回被替换后的字符串。

`re.subn`：返回被替换后的字符串，被替换次数。

### 切片

`re.split`：返回字符串切片。

```python
s=' I have a dog   ,   you have a dog  ,  he have a dog '
re.split( '\s*,\s*' , s )
# [' I have a dog', 'you have a dog', 'he have a dog ']
```

### 取消正则转义

`re.escape`：取消正则字符的转义，直接匹配字符，多用于匹配字符中含很多正则转义字符的情况。

```python
s= '111 222 (*+?) 333'
rule= re.escape( r'(*+?)' )
re.findall( rule , s )
['(*+?)']
```

注意与 `r'…'` 的区别，`r`是取消字符串的转义，比如`r'\n'`就是匹配两个`\`、`n`字符，而非换行符。

`r'\n'`虽然代表两个`\`、`n`字符，但是在正则情况下，还会被正则转义一次变成换行符

参考 [若爱隔山隔海，那就平山平海](https://www.cnblogs.com/mzc1997/p/7689235.html)

```python
s = '3\8' 
re.findall('\d+\\\\\d+', s)  # \\\\ 先字符串转义，转为\\，再正则转义为\
re.findall(r'\d+\\\d+', s) # \\ 直接正则转义为\
```

## 组与match对象

```python
p=re.compile( r'(?P<name>[a-z]+)\s+(?P<age>\d+)\s+(?P<tel>\d+)' , re.I )
p.groupindex
# {'age': 2, 'tel': 3, 'name': 1}
s='Tom 24 88888888 <='
m=p.search(s)
m.groups() # 匹配的各组
# ('Tom', '24', '88888888')
m.group('name') # 通过名称返回匹配组
# 'Tom'
m.group(1) # 通过序号返回匹配组
# 'Tom'
m.group(0) # 返回整体的匹配串
# 'Tom 24 88888888'
m.group()  # m.group(0) 等价 m.group()
# 'Tom 24 88888888'
```

## 参考资料

[python | 史上最全的正则表达式](https://blog.csdn.net/weixin_40907382/article/details/79654372)

