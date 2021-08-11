### Python-basic-tutorial



[**零碎知识点**](0_零碎.md)
[**输入和输出**](1_输入和输出.md)
[**数据类型和运算符**](2_数据类型和运算符.md)
[**列表**](3_列表.md)
[**字典**](4_字典.md)
[**集合和元组**](5_集合和元组.md)
[**函数类型**](6_函数类型.md)
[**类和实例**](7_类和实例.md)
[**进程线程协程**](8_进程线程协程.md)
[**Django**](9_Django.md)
[**Kafka**](10_Kafka.md)
[**消息队列**](11_python中的消息队列使用.md)

### print

```python
print("HelloWorld")
>>>
HelloWorld
```



### print的美化

- Json格式的输出

```python
import json

data = [{'a':1, 'b':2, 'c':3}]

# json.dumps将数组变成JSON
data_json = json.dumps(data)
print(data_json)
print(type(data_json))
>>>
[{"a": 1, "b": 2, "c": 3}]
<class 'str'>

# json.loads将JSON变成数组字段
data_raw = json.loads(data_json)
print(data_raw)
print(type(data_raw))
>>>
[{'a': 1, 'b': 2, 'c': 3}]
<class 'list'>
```

- pprint

```python
import pprint
import json
from urllib.request import urlopen 

with urlopen('https://pypi.org/pypi/sampleproject/json') as resp:
    project_info = json.load(resp)['info']
    pprint.pprint(project_info)
>>>
{'author': 'A. Random Developer',
 'author_email': 'author@example.com',
 'bugtrack_url': None,
 'classifiers': ['Development Status :: 3 - Alpha',
                 'Intended Audience :: Developers',
                 'License :: OSI Approved :: MIT License',
                 'Programming Language :: Python :: 3',
                 'Programming Language :: Python :: 3 :: Only',
                 'Programming Language :: Python :: 3.5',
                 'Programming Language :: Python :: 3.6',
                 'Programming Language :: Python :: 3.7',
                 'Programming Language :: Python :: 3.8',
                 'Topic :: Software Development :: Build Tools'],
 'description': '# A sample Python project\n'
 ...
}
```

### 格式化输出

- 格式化字符串字面值,使用f前缀

```python
year = 2021
month = 5
day = 1

today = f'{year}/{month}/{day}'
print(today)
>>>
2021/5/1
```

- %的常见用法

  - %s 字符串，使用str()显示
  - %r 字符串，使用repr()显示
  - %d 十进制整数
  - %f 浮点数
  - %10s 右对齐，十个占位符
  - %-10s 左对齐，十个占位符
  - %.2s 截断两个字符
  - %10.2s 右对齐十个占位符 截断两个字符
  - %f 保留小数点后六位有效数字
  - %.3f 保留三位有效数字

  ```python
  # %s
  print('%s'%'hello')
  >>>
  hello
  
  # %10s
  print('%10s'%'hello')
  >>>
       hello
    
  # %-10s
  print('%-10s'%'hello')
  >>>
  hello   
  
  # %10.2s
  print('%10.2s'%'hello')
  >>>
          he
    
  # %f
  print('%f'% 1.11)
  >>>
  1.110000
  
  # %.2f
  print('%.2f'% 1.222)
  >>>
  1.22
  ```

  - Format 
    - 不带参数{}
    - 带数字参数 {1}{2}
    - 带关键字 {a} {b}
    - 索引
    - key参数

  ```python
  # 不带参数
  print('{},{}'.format('hello','world'))
  >>>
  hello,world
  # 带数字参数
  print('{0},{1}'.format('hello','world'))
  >>>
  hello,world
  
  # 带关键字参数
  print('{a},{b}'.format(a='hello',b='world'))
  >>>
  hello,world
  
  # 索引
  tup = (1,2)
  print('X:{0[0]}; Y:{0[1]};'.format(tup))
  >>>
  X:1; Y:2;
      
  # key参数
  dic = {'a':11, 'b':22}
  print('X: {0[a]}, Y: {0[b]}'.format(dic))
  >>>
  X: 11, Y: 22
  ```

  

### IO操作

- StringIO:把str写入内存中

```python
from io import StringIO

f = StringIO()
f.write('hello')
f.write(' ')
f.write('world')
print(f.getvalue())
>>>
hello world
```

- BytesIO:把字节流写入内存

```python
from io import BytesIO

f = BytesIO()
f.write('中文'.encode('utf-8'))
print(f.getvalue())
>>>
b'\xe4\xb8\xad\xe6\x96\x87'
```

- with语句

```python
# with语句是一种上下文管理协议
# 通过__enter__()初始化
# 通过__exit__()进行结束和异常处理


# 只读模式
with open('a.txt','r') as f:
    f.readlines()

# 写入模式
with open('a.txt','w') as f:
    f.write()

# 读写模式
with open('a.txt','w+') as f:
    f.write()

# 追加模式
with open('a.txt','a') as f:
    f.readlines()
    f.write()

# 二进制形式
with open('a.txt','ab') as f:
    f.write()
```



- csv模块

```python
import csv

# csv的写入

# 表头创建
headers = ['name','age']

# 每行数据
rows = [
    ['xiaoming',12],
    ['xiaohong',23]
]
# 写入操作
with open('first.csv','w') as f:
    f_csv = csv.writer(f)
    f_csv.writerow(headers)
    f_csv.writerows(rows)
    
# 使用字典写入

rows_dict = [
    {'name':'xiaoming','age':12},
    {'name':'xiaohong','age':23}
]

with open('second.csv','w') as f:
    f_csv = csv.DictWriter(f,headers)
    f_csv.writeheader()
    f_csv.writerows(rows_dict)
    
# csv的读取
with open('first.csv','r') as f:
    f_csv = csv.reader(f)
    for row in f_csv:
        print(row)
>>>
['name', 'age']
['xiaoming', '12']
['xiaohong', '23']
```



### 输入

```python
name = input('What is your name?')
print('Hello ' + name)
```

### 数据类型

- isinstance判断数据类型

```python
# isinstance判断类型
a = 1
print(isinstance(a,int))
>>>
True
```

- string的切片操作

```python
# String的操作

str1 = 'Desktop'

# 打印完整字符串
print(str1)
>>>
Desktop
# 打印第一个字符
print(str1[0])
>>>
D
# 打印第二个到第四个字符
print(str1[1:3])
>>>
es
# 打印倒数第一个字符
print(str1[-1])
>>>
p
# 打印第三个以后的全部字符
print(str1[2:])
>>>
sktop
# 间隔两个打印
print(str1[::2])
>>>
Dstp
# 打印两次
print(str1 * 2)
>>>
DesktopDesktop
# 和String进行拼接
print(str1 + '1')
>>>
Desktop1
```

- \__str\__和\__repr\__

```python
# __repr__和__str__的异同

# 当一个类没有__repr和__str__的时候
class A:
    def __init__(self):
        self.a = 'a'
a = A()
print(a)
>>>
<__main__.A object at 0x7fa42d229d90>


# 当一个类有__str__的时候,可以print出该函数的返回值
class A:
    def __init__(self):
        self.a = 'a'

    def __str__(self):
        return self.a
a = A()
print(a)
>>>
a

# 当一个类有__repr__的时候,可以在交互式状态下输出想要的值
class A:
    def __init__(self):
        self.a = 'a'

    def __str__(self):
        return self.a
    
    def __repr__(self):
        return 'b'
sample = A()
sample
>>>
b

# tips: __str__易读性更好。 __repr__调用方便和精确。 当自己写的class没有__str__方法的时候会自动调用__repr__。所以请写上该方法。
```

### 常见算术运算符

ps:来自菜鸟教程

| 运算符 | 描述                                            | 实例                                               |
| :----- | :---------------------------------------------- | :------------------------------------------------- |
| +      | 加 - 两个对象相加                               | a + b 输出结果 30                                  |
| -      | 减 - 得到负数或是一个数减去另一个数             | a - b 输出结果 -10                                 |
| *      | 乘 - 两个数相乘或是返回一个被重复若干次的字符串 | a * b 输出结果 200                                 |
| /      | 除 - x除以y                                     | b / a 输出结果 2                                   |
| %      | 取模 - 返回除法的余数                           | b % a 输出结果 0                                   |
| **     | 幂 - 返回x的y次幂                               | a**b 为10的20次方， 输出结果 100000000000000000000 |
| //     | 取整除 - 返回商的整数部分（**向下取整**）       | `>>> 9//2 4 >>> -9//2 -5`                          |

```python
# coding:utf-8

a = 21
b = 10
c = 0
 
c = a + b
print("1 - c 的值为：", c)
>>>
1 - c 的值为： 31

c = a - b
print("2 - c 的值为：", c)
>>>
2 - c 的值为： 11

c = a * b
print("3 - c 的值为：", c)
>>>
3 - c 的值为： 210

c = a / b
print("4 - c 的值为：", c)
>>>
4 - c 的值为： 2.1

c = a % b
print("5 - c 的值为：", c)
>>>
5 - c 的值为： 1

# 修改变量 a 、b 、c
a = 2
b = 3
c = a**b 
print("6 - c 的值为：", c)
>>>
6 - c 的值为： 8

a = 10
b = 5
c = a//b 
print("7 - c 的值为：", c)
>>>
7 - c 的值为： 2
```



| 运算符 | 描述                                                         | 实例                                     |
| :----- | :----------------------------------------------------------- | :--------------------------------------- |
| ==     | 等于 - 比较对象是否相等                                      | (a == b) 返回 False。                    |
| !=     | 不等于 - 比较两个对象是否不相等                              | (a != b) 返回 true.                      |
| <>     | 不等于 - 比较两个对象是否不相等。**python3 已废弃。**        | (a <> b) 返回 true。这个运算符类似 != 。 |
| >      | 大于 - 返回x是否大于y                                        | (a > b) 返回 False。                     |
| <      | 小于 - 返回x是否小于y。所有比较运算符返回1表示真，返回0表示假。这分别与特殊的变量True和False等价。 | (a < b) 返回 true。                      |
| >=     | 大于等于 - 返回x是否大于等于y。                              | (a >= b) 返回 False。                    |
| <=     | 小于等于 - 返回x是否小于等于y。                              | (a <= b) 返回 true。                     |

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
a = 21
b = 10
c = 0
 
if  a == b :
   print "1 - a 等于 b"
else:
   print "1 - a 不等于 b"

if  a != b :
   print "2 - a 不等于 b"
else:
   print "2 - a 等于 b"
 
if  a <> b :
   print "3 - a 不等于 b"
else:
   print "3 - a 等于 b"
 
if  a < b :
   print "4 - a 小于 b" 
else:
   print "4 - a 大于等于 b"
 
if  a > b :
   print "5 - a 大于 b"
else:
   print "5 - a 小于等于 b"
>>>
1 - a 不等于 b
2 - a 不等于 b
3 - a 不等于 b
4 - a 大于等于 b
5 - a 大于 b

# 修改变量 a 和 b 的值
a = 5
b = 20
if  a <= b :
   print "6 - a 小于等于 b"
else:
   print "6 - a 大于  b"
 
if  b >= a :
   print "7 - b 大于等于 a"
else:
   print "7 - b 小于 a"
>>>
6 - a 小于等于 b
7 - b 大于等于 a
```

| 运算符 | 描述             | 实例                                  |
| :----- | :--------------- | :------------------------------------ |
| =      | 简单的赋值运算符 | c = a + b 将 a + b 的运算结果赋值为 c |
| +=     | 加法赋值运算符   | c += a 等效于 c = c + a               |
| -=     | 减法赋值运算符   | c -= a 等效于 c = c - a               |
| *=     | 乘法赋值运算符   | c *= a 等效于 c = c * a               |
| /=     | 除法赋值运算符   | c /= a 等效于 c = c / a               |
| %=     | 取模赋值运算符   | c %= a 等效于 c = c % a               |
| **=    | 幂赋值运算符     | c **= a 等效于 c = c ** a             |
| //=    | 取整除赋值运算符 | c //= a 等效于 c = c // a             |

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
a = 21
b = 10
c = 0
 
c = a + b
print "1 - c 的值为：", c
 
c += a
print "2 - c 的值为：", c 
 
c *= a
print "3 - c 的值为：", c 
 
c /= a 
print "4 - c 的值为：", c 
 
c = 2
c %= a
print "5 - c 的值为：", c
 
c **= a
print "6 - c 的值为：", c
 
c //= a
print "7 - c 的值为：", c
>>>
1 - c 的值为： 31
2 - c 的值为： 52
3 - c 的值为： 1092
4 - c 的值为： 52
5 - c 的值为： 2
6 - c 的值为： 2097152
7 - c 的值为： 99864
```

| 运算符 | 描述                                                         | 实例                                                         |
| :----- | :----------------------------------------------------------- | :----------------------------------------------------------- |
| &      | 按位与运算符：参与运算的两个值,如果两个相应位都为1,则该位的结果为1,否则为0 | (a & b) 输出结果 12 ，二进制解释： 0000 1100                 |
| \|     | 按位或运算符：只要对应的二个二进位有一个为1时，结果位就为1。 | (a \| b) 输出结果 61 ，二进制解释： 0011 1101                |
| ^      | 按位异或运算符：当两对应的二进位相异时，结果为1              | (a ^ b) 输出结果 49 ，二进制解释： 0011 0001                 |
| ~      | 按位取反运算符：对数据的每个二进制位取反,即把1变为0,把0变为1 。**~x** 类似于 **-x-1** | (~a ) 输出结果 -61 ，二进制解释： 1100 0011，在一个有符号二进制数的补码形式。 |
| <<     | 左移动运算符：运算数的各二进位全部左移若干位，由 **<<** 右边的数字指定了移动的位数，高位丢弃，低位补0。 | a << 2 输出结果 240 ，二进制解释： 1111 0000                 |
| >>     | 右移动运算符：把">>"左边的运算数的各二进位全部右移若干位，**>>** 右边的数字指定了移动的位数 | a >> 2 输出结果 15 ，二进制解释： 0000 1111                  |

```python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
a = 60            # 60 = 0011 1100 
b = 13            # 13 = 0000 1101 
c = 0
 
c = a & b;        # 12 = 0000 1100
print "1 - c 的值为：", c
 
c = a | b;        # 61 = 0011 1101 
print "2 - c 的值为：", c
 
c = a ^ b;        # 49 = 0011 0001
print "3 - c 的值为：", c
 
c = ~a;           # -61 = 1100 0011
print "4 - c 的值为：", c
 
c = a << 2;       # 240 = 1111 0000
print "5 - c 的值为：", c
 
c = a >> 2;       # 15 = 0000 1111
print "6 - c 的值为：", c
>>>
1 - c 的值为： 12
2 - c 的值为： 61
3 - c 的值为： 49
4 - c 的值为： -61
5 - c 的值为： 240
6 - c 的值为： 15
```

| 运算符 | 逻辑表达式 | 描述                                                         | 实例                    |
| :----- | :--------- | :----------------------------------------------------------- | :---------------------- |
| and    | x and y    | 布尔"与" - 如果 x 为 False，x and y 返回 False，否则它返回 y 的计算值。 | (a and b) 返回 20。     |
| or     | x or y     | 布尔"或" - 如果 x 是非 0，它返回 x 的计算值，否则它返回 y 的计算值。 | (a or b) 返回 10。      |
| not    | not x      | 布尔"非" - 如果 x 为 True，返回 False 。如果 x 为 False，它返回 True。 | not(a and b) 返回 False |

```Python
#!/usr/bin/python
# -*- coding: UTF-8 -*-
 
a = 10
b = 20
 
if  a and b :
   print "1 - 变量 a 和 b 都为 true"
else:
   print "1 - 变量 a 和 b 有一个不为 true"
 
if  a or b :
   print "2 - 变量 a 和 b 都为 true，或其中一个变量为 true"
else:
   print "2 - 变量 a 和 b 都不为 true"
 
# 修改变量 a 的值
a = 0
if  a and b :
   print "3 - 变量 a 和 b 都为 true"
else:
   print "3 - 变量 a 和 b 有一个不为 true"
 
if  a or b :
   print "4 - 变量 a 和 b 都为 true，或其中一个变量为 true"
else:
   print "4 - 变量 a 和 b 都不为 true"
 
if not( a and b ):
   print "5 - 变量 a 和 b 都为 false，或其中一个变量为 false"
else:
   print "5 - 变量 a 和 b 都为 true"
>>>
1 - 变量 a 和 b 都为 true
2 - 变量 a 和 b 都为 true，或其中一个变量为 true
3 - 变量 a 和 b 有一个不为 true
4 - 变量 a 和 b 都为 true，或其中一个变量为 true
5 - 变量 a 和 b 都为 false，或其中一个变量为 false
```

- \__add\__和\__mul\__

```python
# coding:utf-8

# 改变正方形的长和宽
class square_cal:
    def __init__(self,height=10,weight=2):
        self.height = height
        self.weight = weight

    # 定义类相加逻辑
    def __add__(self,others):
        return square_cal(self.height + others.height, \
                          self.weight + others.weight)
    
    # 定义类相乘逻辑
    def __mul__(self,others):
        return square_cal(self.height * others.height,\
                          self.weight * others.weight)

    # 定义展示函数
    def show(self):
        return 'height:{}, weight:{}'.format(self.height,self.weight)  

a = square_cal()
print(a.show())
>>>
height:10, weight:2

b = square_cal(2,2)
print(b.show())
>>>
height:2, weight:2

c = a + b
print(c.show())
>>>
height:12, weight:4

d = a * b
print(d.show())
>>>
height:20, weight:4
```

### List的特性

- 可变容器类型（可以增减，可以放置不同类型数据）
- 可以进行索引和切片操作



### List的底层原理

```c
typedef struct {
    PyObject_VAR_HEAD
    /* Vector of pointers to list elements. list[0] is ob_item[0], etc. */
    PyObject **ob_item;

    /* ob_item contains space for 'allocated' elements. The number
     * currently in use is ob_size.
     * Invariants:
     * 0 <= ob_size <= allocated
     * len(list) == ob_size
     * ob_item == NULL implies ob_size == allocated == 0
     * list.sort() temporarily sets allocated to -1 to detect mutations.
     *
     * Items must normally not be NULL, except during construction when
     * the list is not yet visible outside the function that builds it.
     */
    Py_ssize_t allocated;
} PyListObject;
```

- list本质上是长度可变的连续数组
- ob_item是一个指针列表，每一个指针都指向列表中的元素
- allocated是存储该列表目前已被分配的空间大小
  - allocated和列表的实际空间大小不同
  - 列表的实际大小是指len(list)的返回值 也就是注释中的ob_size, 表示列表中含有多少个元素
  - 实际情况中，为了避免每次增加元素都重新分配内存，列表预分配空间allocated往往比ob_size大
  - 0 <= ob_size <= allocated



### 常用function

- 索引和切片时
  - 以0作为首个下标且不算区间里的最后一个元素
    - my_list[:3]返回三个元素
    - 便于计算区间长度(stop-start)即可
    - my_list[:x]和my_list[x:]不会重复

```python
a_list = list(range(10))
print(a_list)
>>>
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
a_list[2:3] = [33]
print(a_list)
>>>
[0, 1, 33, 3, 4, 5, 6, 7, 8, 9]
```



- 更新列表append

```python
# coding:utf-8

a_list = []
a_list.append(1)
a_list.append(2)
a_list.append(1)
print(a_list)
>>>
[1, 2, 1]
```

- 删除元素del

```python
del a_list[1]
print(a_list)
>>>
[1, 1]
```

- 统计元素数量count

```python
print(a_list.count(1))
>>>
2
```

- 元素索引index

```python
print(a_list.index(1))
>>>
0
```

- 插入元素insert

```python
a_list.insert(1,'1')
print(a_list)
>>>
[1, '1', 1]
```

- 弹出元素pop

```python
a_list.pop(-2)
print(a_list)
>>>
[1, 1]
```

- 移除元素remove

```python
a_list.remove(1)
print(a_list)
>>>
[1]
```

- 逆转列表reverse

```python
a_list = [1,4,2,5,3,7,4]
a_list.reverse()
print(a_list)
>>>
[4, 7, 3, 5, 2, 4, 1]
```

- 合并列表extend

```python
aList = [123, 'xyz', 'zara', 'abc', 123]
bList = [2009, 'manni']
aList.extend(bList)
print(aList)
>>>
[123, 'xyz', 'zara', 'abc', 123, 2009, 'manni']
```

- 列表推导～

```python
# coding:utf-8

sample = 'test'
a_list = [i for i in sample]
print(a_list)

# mapping
def square(x):
    return x**2

print(list(map(square,[1,2,3])))

# filter
def is_odd(n):
    return n % 2 == 1

tmplist = filter(is_odd,[1,2,3,4,5,6,7])
newlist = list(tmplist)
print(newlist)
```

- 列表使用+和*

```python
# coding:utf-8

a = [1,2,3]
b = [4,5,6]
print(a+b)
print(a*2)
>>>
[1, 2, 3, 4, 5, 6]
[1, 2, 3, 1, 2, 3]
```

- list.sort() 就地排序，并不会复制一份，所以返回值也就是None
- sorted() 则不一样，他会在排序后构建新的列表并返回

```python
# coding:utf-8

l = ['bb','ccc','a','dddd']
l_sorted = sorted(l)
print(l_sorted)
>>>
['a', 'bb', 'ccc', 'dddd']

l_sorted_len = sorted(l,key=len)
print(l_sorted_len)
>>>
['a', 'bb', 'ccc', 'dddd']

l_sorted_reverse = sorted(l,reverse=True)
print(l_sorted_reverse)
>>>
['dddd', 'ccc', 'bb', 'a']

l.sort()
print(l)
>>>
['a', 'bb', 'ccc', 'dddd']
```

### 什么是字典

- 字典是一系列由键和值配对组成的元素集合。
- 是可变容器模型
- 搜索速度很快，数据量增加10000倍，搜索速度也就2倍



### 底层实现

- 通过散列表实现的
- 字典也可以叫做哈希数组或者关联数组
- 包括两个部分，一个是键对象的引用，一个是值对象的引用
- Dic = {} 初始化一个字典，其哈希数组的长度就是8

```python
# coding:utf-8
dic = {}
dic['a'] = 1
print(bin(hash('a')))
>>>
0b111111000001101110000011110110000100101100111000110101111001100
```

- 最后三位是100，十进制的4，则查看数组索引4中是否为空。空则放入，否则左移三位001，查看索引1处是否为空直到放入数据为止。
- 接近填入2/3的时候就会进行扩容，比如扩充到16的时候，偏置值就会到4
- 数字，字符串，元组等都是可hash的
  - 支持使用\__eq\__()
  - a==b,则hash(a)==hash(b)
- python中不可变类型都是可hash的
- 字典开销大，用空间换时间
- 查询很快，而列表则是一个一个遍历
- 添加新键的时候可能会扩容，导致哈希数组的键的顺序发生变化，所以不要在遍历的过程中对字典进行修改。



### 常用function

- 几种生成字典的方式

```python
a = dict(one=1, two=2, three=3)
b = {'one':1, 'two':2, 'three':3}
c = dict(zip(['one','two','three'],[1,2,3]))
d = dict([('one',1),('two',2),('three',3)])
print(a == b == c == d)
>>>
True
```

- 访问和修改键值

```python
# coding:utf-8

# 访问键值
profile = {'Name':'kkk','age':18}
print(profile['Name'],profile['age'])
>>>
kkk 18

# 修改键值
profile['age'] = 17
print(profile['age'])
>>>
17

dic = {'Name': 'kkk', 'Age': 18}

# update 增加元素
dic2 = {'sex':'female'}
dic.update(dic2)
print(dic)

dic = {'Name': 'kkk', 'Age': 18}

# setdefault 增加元素
# 如果不存在字典中，添加键并且设置为默认值
print(dic.setdefault('Name',None))
print(dic.setdefault('sex','male'))
>>>
kkk
male
```

- 删除字典元素

```python
dic = {'Name': 'kkk', 'Age': 18}
# 删除键值Name
del dic['Name']
print(dic)
>>>
{'Age': 18}

# 清空字典所有条目
dic = {'Name': 'kkk', 'Age': 18}
dic.clear()
print(dic)
>>>
{}

# 删除整个字典
dic = {'Name': 'kkk', 'Age': 18}
del dic

dic = {'Name': 'kkk', 'Age': 18}
# pop删除
print(dic.pop('Name')) 
>>>
kkk

# popitem 弹出最后一个键值对
print(dic.popitem())
>>>
('Age', 18)
```

- 获取键值

```python
dic = {'Name': 'kkk', 'Age': 18}

# get 获取键值
print(dic.get('Name'))
print(dic.get('Age'))
>>>
kkk
18
# keys 获取键
print(dic.keys())
>>>
dict_keys(['Name', 'Age'])

# values 获取值
print(dic.values())
>>>
dict_values(['kkk', 18])

# items 获取键值
print(dic.items())
>>>
dict_items([('Name', 'kkk'), ('Age', 18)])
```

- defaultdict和orderedDict

```python
from collections import OrderedDict, defaultdict
# defaultdict 是带有默认值的字典
# 作用在于 当字典中没有内容的时候 不是raise一个error 而是有默认值
sample_1 = defaultdict(int)
print(sample_1[1])
>>>
0

# 相较于普通的dict， OrderedDict是一个有序的dict
sample_1 = OrderedDict()
sample_1['Name'] = 'kkk'
sample_1['Age'] = 18
sample_1['Sex'] = 'female'

for k,v in sample_1.items():
    print(k,v)
>>>
Name kkk
Age 18
Sex female
```

### 什么是集合set

- 是无序且不重复的

### set和frozenset的异同

- set是可变集合
- frozenset是不可变集合->可哈希

### 构建set的方式

```python
# 构建set的常见方式
# 方法一
sample_1 = set('google')
print(sample_1)

type_sample_1 = type(sample_1)
print(type_sample_1)
>>>
set(['e', 'o', 'g', 'l'])
<type 'set'>

# 方法二
sample_2 = {'g','o','o','g','l','e'}
print(sample_2)

type_sample_2 = type(sample_2)
print(type_sample_2)
>>>
set(['e', 'o', 'g', 'l'])
<type 'set'>
```



### 常用function

- add添加

```python
# add添加方式
sample_1 = set('google')
print(sample_1)

sample_1.add('g')
print(sample_1)

sample_1.add('a')
print(sample_1)
>>>
set(['e', 'o', 'g', 'l'])
set(['e', 'o', 'g', 'l'])
set(['a', 'e', 'o', 'g', 'l'])
```

- update更新方式

```python
# update
sample_1 = set('google')
print(sample_1)

sample_1.update('ao')
print(sample_1)

sample_1.update('cd')
print(sample_1)
>>>
set(['e', 'o', 'g', 'l'])
set(['a', 'e', 'o', 'g', 'l'])
set(['a', 'c', 'e', 'd', 'g', 'l', 'o'])
```

- remove

```python
# remove
sample_1 = set('google')
print(sample_1)

sample_1.remove('g')
print(sample_1)
>>>
set(['e', 'o', 'g', 'l'])
set(['e', 'o', 'l'])
```

- frozenset因为是不可变集合 自然也就无法进行增加和删除



### 元组tuple

- 和列表类似 但不可以修改
- 使用（）



### 常用function

- 构建方式

```python
sample_1 = ('a','b','c')
print(sample_1)

type_sample_1 = type(sample_1)
print(type_sample_1)
>>>
('a', 'b', 'c')
<type 'tuple'>
```

- 索引访问

```python
# 访问元组
sample_1 = ('a','b','c')
print(sample_1)

print(sample_1[0])
print(sample_1[1:])
>>>
('a', 'b', 'c')
a
('b', 'c')
```

- 添加元素

```python
# 元组无法在原来基础上进行修改，因为每一个元组都是不可变的
# 但是可以通过生成新元组的方式进行
sample_1 = ('a','b','c')
sample_2 = ('e','f','g')

sample_3 = sample_1 + sample_2
print(sample_3)
```

- 删除元组

```python
sample_1 = ('a','b','c')
print(sample_1)
del sample_1
print(sample_1)
>>>
('a', 'b', 'c')
Traceback (most recent call last):
  File "/Users/xxx.py", line 7, in <module>
    print(sample_1)
NameError: name 'sample_1' is not defined
```

### 什么是函数

- 具有相关联功能的一个代码块
- 以def关键字开头，内容进行缩进



### 参数

- 传递参数类型
  - 不可变对象
    - 相当于C++中的值传递
    - string tuple numbers
    - 实际上就是进行了复制 不影响原来的对象
  - 可变对象
    - 相当于C++中的引用传递
    - list dict等
    - 真的把对象内的东西进行传递



### 不同的参数说明

- 必备参数：必须按照正确的顺序传入参数才能正常调用

``` python
def print_special_output(str, int):
    print('Right params')

print_special_output('a')
>>>
TypeError: print_special_output() takes exactly 2 arguments (1 given)
  
print_special_output('a','b')
>>>
Right params
```

- 关键字参数：可以在传入必备参数的情况下，选择是否传入关键字参数进行信息的补充

```python
# 关键字参数kw可以选填, 必备参数name, age必填
def profile(name, age, **kw):
    print('Name:' + name, 'Age:' + str(age), 'Other_info:' + str(kw))

# 只填写必备参数
profile('Andy',18)
    
# 使用关键字参数进行信息补充
other_info = {'language':'Chinese', 'City':'GZ'}
profile('Andy',18,**other_info)
```

- 默认参数：如果在函数调用时，没有进行参数传递，就使用默认值的方式

```python
# 默认参数 当没有传入参数的时候会给一个指定的值
def profile(name,age=18):
    print('Name:' + name,'Age:' + str(age))

profile('Andy')

# 修改默认参数的值 在调用函数的时候传入
profile('Andy',20)
```

- 不定长参数：用*，可以在调用函数的时候传入不同个数的参数

```python
# 使用*作为不定长参数，会以元组的形式
def profile(name, *args):
    print('Name:' + name)
    print('Other:' + str(args))
    print('---------------')

profile('Andy')
profile('Andy',18)
profile('Andy',18,'Chinese')
>>>
Name:Andy
Other:()
---------------
Name:Andy
Other:(18,)
---------------
Name:Andy
Other:(18, 'Chinese')
```



### 变量

- 区别：作用域
- 局部变量：仅作用在所在的函数内
- 全局变量：作用在整个py文件中



### local/global/nonlocal

- local：局部变量
- global关键字：在函数内部声明global变量，可以取代函数外部的同名变量

```python
# global关键字的使用
name = 'Andy'
def profile():
    global name
    name = 'Alan'
    print(name)
profile()
>>>
Alan
```

- Nonlocal关键字

```python
# nonlocal使用在必包函数中
x = 0
def outter():
    x = 1
    def inner():
        x = 2
        print('inner:', x)
    inner()
    print('outter:', x)

outter()
print('global:', x)
>>>
('inner:', 2)
('outter:', 1)
('global:', 0)

# 使用nonlocal
x = 0
def outter():
    x = 1
    def inner():
        nonlocal x
        x = 2
        print('inner:', x)
    inner()
    print('outter:', x)

outter()
print('global:', x)
>>>
('inner:', 2)
('outter:', 2)
('global:', 0)
```



### 回调函数

- 定义一个函数，把这个函数的函数名作为参数传递到另一个函数中

```python
def my_callbcak(args):
    print(args)

def caller(args, func):
    func(args)

caller((1,2), my_callbcak)
>>>
(1, 2)
```

- 带有额外信息的callback

```python
def apply_callback(func, args, callback):
    result = func(*args)
    callback(result)

def add(x,y):
    return x + y

def print_result(result):
    print(result)

apply_callback(add,(2,3),callback=print_result)
```



### 闭包

- 如果一个内部函数，对外部作用域的变量进行引用，那么内部函数就称为必包
- 满足以下几点
  - 必须在一个函数的内部
  - 闭包函数必须返回嵌套函数

```python
def addx(x):
    def adder(y):
        return x + y
    return adder

sample_1 = addx(8)
print(type(sample_1))
print(sample_1.__name__)
print(sample_1(10))
>>>
<type 'function'>
adder
18
```



### 匿名函数

- lambda表达式

```python
# 简易的匿名函数
add = lambda x, y : x + y
output = add(2,3)
print(output)
>>>
5
```



### 装饰器

- 在一个函数上添加上装饰器，相当于给函数赋能，增加其能力
- 装饰器就是一个函数，接受另一个函数作为参数，返回其对应的结果

```Python
# 装饰器
import time
from functools import wraps

def timeit(func):
    @wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        end = time.time()
        print(func.__name__, end - start)
        return result
    return wrapper

@timeit
def countdown(n):
    while n > 0:
        n -= 1
    return 'done'

sample_1 = countdown(1000000)
print(sample_1)
>>>
('countdown', 0.03739595413208008)
done
```

### 类 Class

Q : 什么是类？什么是对象？
A : 类定义了它每个对象共有的属性和方法。对象是类定义的数据结构实例。

Q : 如何简单实现一个类

```python
# 简单定义一个类
class student:
  def __init__(self):
    self.name = 'xxx'

a = student()
a.name
>>> xxx
```



Q :  \__init__()函数和self分别是什么

A : 

- 此函数相当于C++中的构造函数，创建实例时自动调用。其中第一个参数必须是self，self参数是指向实例的引用，用于访问类的属性和方法。
- 在派生类中不会自动调用基类的init，如果派生类要调用基类的则是super().\__init__()



Q : Python中的继承的基本概念

A : 

- 当我们有一个class，继承它的类是子类，他是其子类的父类或基类。

```python
# 定义一个person类 然后让孩子类继承它
class Person:
    def __self__(self,name,age):
        self.name = name
        self.age = age
    
    def print_sth(self):
        print(self.name)
        
class Child(Person):
    pass
```

- 对于子类最大的好处就是可使用父类的属性和方法，同时因为继承了构造函数，所以实例化过程中要传入对应的参数。
- 如果没继承类，那继承自object类。
- 继承让一些代码得到复用，只需要在子类中添加个性化的功能即可。

```python
# 新增属性在构造函数的话，使用super
class Child(Person):
    def __init__(self,name,age,sex):
        super().__init__(name,age)
        self.sex = sex
```



Q : 多态的基本概念

A : 

```python
# 在child类中重写Person类的方法
class Child(Person):
    def __init__(self,name,age,sex):
    		super().__init__(name,age)
        self.sex = sex
    def print_sth(self):
    		print(self.sex)
```

- 多态让子类的实例可以直接用父类方法的同时，也可以进行重写。其中的“开闭原则”，对扩展开放即允许子类重写方法，对修改封闭即不重写的情况下，直接继承父类函数。



Q : 什么是多重继承？

A : 从不同的继承树中分别选择并继承出子类，便于组合。比如D继承了B和C，B和C又继承了A。那么D就会有ABC的所有功能。

- 经典例子：现在有狗狗、蝙蝠、鹦鹉、鸵鸟四种动物
- 父类是动物可以延伸出哺乳类和鸟类，又有会飞的和会跑的。那么要继续继承出会飞的鸟和会跑的鸟吗？
- 更好的思路应该是另外开启runnable类和flyable类让狗狗等动物继承
- 在设计类的继承关系时，主线都是单一继承下来的。当加入其他功能的时候，就可以通过多重继承实现。我们把这种设计称为MinIn。

```python
class Animal(object):
  	pass
class Mannal(Animal):
  	pass
class Bird(Animal):
  	pass
class RunnableMixIn(object):
  	pass
class FlyableMixIn(object):
  	pass
  
# dog类就可以进行多重继承了
class Dog(Mannal,RunnalbeMixIn):
  	pass
```

Q : Python中的GIL是什么？

A : 首先GIL并不是Python的特性，而是解释器CPython引入的概念。中文名是全局解释器锁，类似于操作系统的Mutex。GIL的功能是在CPython解释器中执行的每一个Python线程，都先锁住自己，不让别的线程执行。所以实际上这些线程是伪并行。



Q : 为什么要引入GIL呢？

A : 虽然性能并没有提高，但是实现了模拟多线程。而GIL存在的原因是CPython使用了引用计数器来管理内存。

- 假设有两个 Python 线程同时引用 a，那么双方就都会尝试操作该数据，很有可能造成引用计数的条件竞争，导致引用计数只增加 1（实际应增加 2），这造成的后果是，当第一个线程结束时，会把引用计数减少 1，此时可能已经达到释放内存的条件（引用计数为 0），当第 2 个线程再次视图访问 a 时，就无法找到有效的内存了。
- 所以，CPython 引入 GIL，可以最大程度上规避类似内存管理这样复杂的竞争风险问题。

```python
# 引用计数器例子
import sys

a = [1]
b = a
print(sys.getrefcount(a))
```

- 这个例子中，因为a、b、c、getrefcount都有引用[1]这个列表，所以是四次。



Q : 那么有GIL锁了是否线程就是一定安全了？

A : 虽然已经有了GIL，但还是要注意线程安全。因为check_interval机制，也是会导致出现线程安全问题的。

```python
import threading

num = 0

def change_num(n):
    global num
    num += n
    num -= n

def run(n):
    for _ in range(1000):
        change_num(n)

def main():
    thread1 = threading.Thread(target=run,args=[5])
    thread2 = threading.Thread(target=run,args=[6])
    thread1.start()
    thread2.start()
    thread1.join()
    thread2.join()

main()
print(num)
```

- 发现每次的结果都不一样，是因为num+=1实际上是由4个bytecode组成的，GIL并没有注重这个问题。所以在使用python的多线程时依然需要lock()进行加锁。



Q : Python中如何使用多线程的基本操作？

A : 

- 使用threading模块的一些常用方法
- run()线程活动的方法
- Start()启动线程
- join()等待到线程中止
- isAlive/getName/setName

```python
import threading
import time

def aaa(word):
    for _ in range(3):
        print('aaa and %s'%word)
        time.sleep(1)


def bbb(word):
    for _ in range(3):
        print('bbb and %s'%word)
        time.sleep(1)

def main():
    threads = []
    thread1 = threading.Thread(target=aaa,args=['aaa'])
    thread2 = threading.Thread(target=bbb,args=['bbb'])
    threads.append(thread1)
    threads.append(thread2)
    for t in threads:
        t.start()
    for t in threads:
        t.join()

main()
```





Q : 多个线程一起对某个数据修改，可能会出现不可预料的结果，为了确保数据的正确性，需要多个进程同步的。如何使用threading中的Lock进行进程同步。

A : 

```python
# threading.Lock()
# acquire和release获取和释放锁
# coding:utf-8
import threading
import time

class MyThread(threading.Thread):
    def __init__(self,threadID,name,counter):
        threading.Thread.__init__(self)
        self.threadID = threadID
        self.name = name
        self.counter = counter

    def run(self):
        print("开启进程：" + self.name)
        # 获取
        threadLock.acquire()
        print_time(self.name, self.counter, 3)
        threadLock.release()

def print_time(threadName, delay, counter):
    while counter:
        time.sleep(delay)
        print ("%s: %s" % (threadName, time.ctime(time.time())))
        counter -= 1


threadLock = threading.Lock()
threads = []


# 创建进程实例
thread1 = MyThread(1, 'Thread-1', 1)
thread2 = MyThread(2, 'Thread-2', 2)

thread1.start()
thread2.start()

threads.append(thread1)
threads.append(thread2)

for t in threads:
    t.join()
print('done')
>>>
开启进程：Thread-1
开启进程：Thread-2
Thread-1: Sat Jul 31 22:15:26 2021
Thread-1: Sat Jul 31 22:15:27 2021
Thread-1: Sat Jul 31 22:15:28 2021
Thread-2: Sat Jul 31 22:15:30 2021
Thread-2: Sat Jul 31 22:15:32 2021
Thread-2: Sat Jul 31 22:15:34 2021
done
```



Q : 如何使用线程优先级队列来实现进程间的同步？

A : Python的Queue模块中就提供了同步且线程安全的队列类。

一些常用方法：

- Queue.empty():队列是否空
- Queue.full():队列是否满
- Queue.get():获取队列
- Queue.put():写入队列
- Queue.task_done():完成一个task后，向队列发出完成信号
- Queue.join():等待队列为空

```python
# coding:utf-8
import threading
import queue
import time

exitFlag = 0

class myThread(threading.Thread):
    def __init__(self, threadID, name, q):
        threading.Thread.__init__(self)
        self.threadID = threadID
        self.name = name
        self.q = q
    
    def run(self):
        print('开启线程：' + self.name)
        process_data(self.name, self.q)
        print ("退出线程：" + self.name)


def process_data(threadName, q):
    while not exitFlag:
        queueLock.acquire()
        if not workQueue.empty():
            data = q.get()
            queueLock.release()
            print ("%s processing %s" % (threadName, data))
        else:
            queueLock.release()
        time.sleep(1)

threadList = ["Thread-1", "Thread-2", "Thread-3"]
nameList = ["One", "Two", "Three", "Four", "Five"]
queueLock = threading.Lock()
workQueue = queue.Queue(10)
threads = []
threadID = 1

# 创建线程
for tName in threadList:
    thread = myThread(threadID, tName, workQueue)
    thread.start()
    threads.append(thread)
    threadID += 1

# 填充队列
queueLock.acquire()
for word in nameList:
    workQueue.put(word)
queueLock.release()

# 等待队列任务被消化掉
while not workQueue.empty():
    pass

exitFlag = 1
for t in threads:
    t.join()
print('exit main thread')
>>>
开启线程：Thread-1
开启线程：Thread-2
开启线程：Thread-3
Thread-3 processing One
Thread-1 processing Two
Thread-2 processing Three
Thread-3 processing Four
Thread-1 processing Five
退出线程：Thread-3
退出线程：Thread-1
退出线程：Thread-2
exit main thread
```





Q : Python的多进程和多线程有什么区别？

A : python中的多线程并不是真正意义上的多线程，想要充分利用cpu的多核资源，大部分时候情况使用多进程。python中的multiprocessing可以轻松的完成单进程到并发执行的转换。提供了Process、Queue、Pipe、Lock等组件。

```python
from multiprocessing import Process
import os

def info(title):
    print(title)
    print('module name:',__name__)
    print('parent process:', os.getppid())
    print('process id:', os.getpid())

def f(name):
    info('function f')
    print('hello',name)

if __name__ == '__main__':
    info('main line')
    p = Process(target=f, args=('bob',))
    p.start()
    p.join()

>>>
main line
('module name:', '__main__')
('parent process:', 12121)
('process id:', 13848)
function f
('module name:', '__main__')
('parent process:', 13848)
('process id:', 13849)
('hello', 'bob')
```



Q : multiprocessing有多种启动进程的方法，分别有什么不同？

A : 有spawn、fork、forkserver三种启动方法

- spawn：父进程会启动一个全新的python解释器进程，子进程只继承了运行对象的run方法所需的资源。比较慢。
- fork：父进程使用os.fork()来产生python解释器分叉。
- forkserver：启动服务器进程，每当需要一个新进程时，父进程就会链接到服务器并请求它分叉一个新进程。



Q : 多进程中的start和join分别有什么作用？

A : start会给进程进行启动。而join会阻塞主进程直到当前子进程完成为止。



Q : 在使用Process时候使用函数和类的区别？

A : 

```python
# 函数方式
import multiprocessing
import time

def work_1(interval):
    print("work-1")
    time.sleep(interval)
    print("End work-1")

def work_2(interval):
    print("work-2")
    time.sleep(interval)
    print("End work-2")

def work_3(interval):
    print("work-3")
    time.sleep(interval)
    print("End work-3")

if __name__ == '__main__':
    p1 = multiprocessing.Process(target = work_1, args = (2,))
    p2 = multiprocessing.Process(target = work_2, args = (3,))
    p3 = multiprocessing.Process(target = work_3, args = (4,))

    p1.start()
    p2.start()
    p3.start()
    

    print("The number of CPU is:" + str(multiprocessing.cpu_count()))
    for p in multiprocessing.active_children():
        print("child p.name:" + p.name + "\tpid:" + str(p.pid))
    
    print("END")

    p1.join()
    p2.join()
    p2.join()
>>>
work-1
work-2
work-3
The number of CPU is:4
child p.name:Process-3	pid:14503
child p.name:Process-1	pid:14501
child p.name:Process-2	pid:14502
END
End work-1
End work-2
End work-3
```

```python
# Process使用类
import multiprocessing
import time

class ClockProcess(multiprocessing.Process):
    def __init__(self,interval):
        multiprocessing.Process.__init__(self)
        self.interval = interval

    
    def run(self):
        n = 5
        while n > 0:
            print("The time is {0}".format(time.ctime()))
            time.sleep(self.interval)
            n -= 1
    
if __name__ == '__main__':
    p = ClockProcess(3)
    p.start()
>>>
The time is Mon Aug  2 17:38:06 2021
The time is Mon Aug  2 17:38:09 2021
The time is Mon Aug  2 17:38:12 2021
The time is Mon Aug  2 17:38:15 2021
The time is Mon Aug  2 17:38:18 2021
```



Q : 如何在多进程中使用Lock进行同步或者不使用锁的情况下进行同步操作？

A : 

```python
# 在不使用锁的情况下进行同步
import multiprocessing
import time

def job(v, num):
    for _ in range(5):
        time.sleep(0.1)
        v.value += num
        print(v.value, end=",")

def multicore():
    # 定义共享变量
    v = multiprocessing.Value('i', 0)
    p1 = multiprocessing.Process(target=job,args=(v, 1))
    p2 = multiprocessing.Process(target=job,args=(v, 3))

    p1.start()
    p2.start()


if __name__ == '__main__':
    multicore()
# 发现会争夺共享资源
>>>
1,5,9,13,17,4,8,12,16,20
```

```python
import multiprocessing
import time
# 使用锁进行同步 使得资源不会抢占
lock = multiprocessing.Lock()

def job(v, num, lock):
    lock.acquire()
    for _ in range(5):
        time.sleep(0.1)
        v.value += num
        print(v.value, end=",")
    lock.release()

def multicore():
    # 定义共享变量
    v = multiprocessing.Value('i', 0)
    p1 = multiprocessing.Process(target=job,args=(v, 1, lock))
    p2 = multiprocessing.Process(target=job,args=(v, 3, lock))

    p1.start()
    p2.start()


if __name__ == '__main__':
    multicore()
>>>
3,6,9,12,15,16,17,18,19,20
```



Q : Python的进程池如何使用，是同步还是异步的？要如何实现异步？

A : Multiprocessing.Pool可以提供指定数量的进程供用户调用，当新的请求提交到pool中，如果池子没满，那么就会创建一个新的进程来执行请求。如果池子的请求数已经达到最大值，那么请求就会等待，直到池中有进程结束，才会创建新进程来执行他。

```python
import multiprocessing
import time

def test(interval):
    print(interval)
    time.sleep(3)


if __name__ == '__main__':
    pool = multiprocessing.Pool(processes=10)
    for i in range(10):
        pool.apply(test, args=(i,))
    
    print('test')
    pool.close()
    pool.join()
>>>
0
1
2
3
....
# 发现这样进程会阻塞 一个个子进程执行 最后回到被join阻塞的主进程。
```

```python
# 改用apply_async实现异步进程池
import multiprocessing
import time

def test(interval):
    print(interval)
    time.sleep(3)


if __name__ == '__main__':
    pool = multiprocessing.Pool(processes=10)
    for i in range(10):
        pool.apply_async(test, args=(i,))
    
    print('test')
    pool.close()
    pool.join()
>>>
test
0
1
2
...
# 程序在异步执行，没有阻塞。
```

```python
	# 同样还有map_async可以进行map的异步操作
  import multiprocessing
import time

def test(interval):
    print(interval)
    time.sleep(3)


if __name__ == '__main__':
    pool = multiprocessing.Pool(processes=2)
    pool.map_async(test,range(500))
    print('test')
    pool.close()
    pool.join()
>>>
test
0
64
63
234
---
```



Q : 多进程中如何进行不同进程的沟通交流？

A : 主要有Pipe和Queue两种方式。其中Pipe就是管道模式，只适用于两个进程一读一写的单双工，也就是说信息只向一个方向流动。Pipe的读写效率比Queue要高。

当主进程创建Pipe的时候，Pipe的两个Connections连接的的都是主进程。
当主进程创建子进程后，Connections也被拷贝了一份。此时有了4个Connections。
此后，关闭主进程的一个Out Connection，关闭一个子进程的一个In Connection。那么就建立好了一个输入在主进程，输出在子进程的管道。

```python
# coding:utf-8
from multiprocessing import Process, Pipe

def son_process(x, pipe):
    _out_pipe, _in_pipe = pipe

    # 关闭fork过来的输入端
    _in_pipe.close()
    while True:
        try:
            msg = _out_pipe.recv()
            print(msg)
        except EOFError:
            # 当out_pipe接受不到输出的时候且输入被关闭的时候，会抛出EORFError，可以捕获并且退出子进程
            break


if __name__ == '__main__':
    out_pipe, in_pipe = Pipe(True)
    son_p = Process(target=son_process, args=(100, (out_pipe, in_pipe)))
    son_p.start()

    # 等 pipe 被 fork 后，关闭主进程的输出端
    # 这样，创建的Pipe一端连接着主进程的输入，一端连接着子进程的输出口
    # 当pipe的输入端被关闭，且无法接收到输入的值，那么就会抛出EOFError。
    out_pipe.close()
    for i in range(1000):
        in_pipe.send(i)
    in_pipe.close()
    son_p.join()
    print('Main Done')
```

Queue则是使用get和put 方法，可以在多个process中进行。

```python
# coding:utf-8

from multiprocessing import Queue, Process
from queue import Empty as QueueEmpty
import random

def getter(name, queue):
    print('Son process %s') % name
    while True:
        try:
            value = queue.get(True, 10)
            # block为True,就是如果队列中无数据了。
            #   |—————— 若timeout默认是None，那么会一直等待下去。
            #   |—————— 若timeout设置了时间，那么会等待timeout秒后才会抛出Queue.Empty异常
            # block 为False，如果队列中无数据，就抛出Queue.Empty异常
            print("Process getter get: %f") % value
        except QueueEmpty:
            break


def putter(name, queue):
    print("Son process %s") % name
    for i in range(0, 1000):
        value = random.random()
        queue.put(value)
        # 放入数据 put(obj[, block[, timeout]])
        # 若block为True，如队列是满的：
        #  |—————— 若timeout是默认None，那么就会一直等下去
        #  |—————— 若timeout设置了等待时间，那么会等待timeout秒后，如果还是满的，那么就抛出Queue.Full.
        # 若block是False，如果队列满了，直接抛出Queue.Full
        print("Process putter put: %f") % value

if __name__ == '__main__':
    queue = Queue()
    getter_process = Process(target=getter, args=("Getter", queue))
    putter_process = Process(target=putter, args=("Putter", queue))
    getter_process.start()
    putter_process.start()
```



### 协程

- 通常认为线程是轻量级的进程，协程可以理解成微线程
- 通常python中进行并发变成都是使用多进程或者多线程，计算型任务由于GIL的存在所以我们多使用多进程，IO密集的任务，可以通过在IO时让出GIL，使用多线程，达到表面上的并发。其实对于IO型任务我们还有一种选择就是协程，协程是运行在单线程当中的"并发"，协程相比多线程一大优势就是省去了多线程之间的切换开销，获得了更大的运行效率。
- 协程的作用：
  - 在执行函数A时可以随时中断去执行函数B，然后中断函数B继续执行函数A。但这个过程不是函数调用，这个过程像多线程，其实协程只有一个线程执行。
- 协程的优势
  - 执行效率极高，不是线程切换，而是程序自身控制，没有线程开销。
  - 不需要多线程的锁机制，因为只有一个线程，也不存在同时写变量冲突，在控制共享资源时也不需要加锁，因此执行效率高很多。



### asyncio + yield from

- asyncio在Python3.4引入的标准库，直接内置了对异步IO的支持。

```python
import asyncio
@asyncio.coroutine
def test(i):
    print('test_1', i)
    r = yield from asyncio.sleep(1)
    print('test_2', i)

if __name__ == '__main__':
    loop = asyncio.get_event_loop()
    tasks = [test(i) for i in range(3)]
    loop.run_until_complete(asyncio.wait(tasks))
    loop.close()
```

- @asyncio.coroutine把一个generator标记为coroutine类型，然后就把这个coroutine扔到EventLoop中执行。test()会先打印test_1,然后yield from语法可以让我们方便地调用另一个generator。线程不会等待asyncio.sleep，而是直接中断并执行下一个消息循环。当其返回时，线程就可以从yield from 拿到返回值，然后接着执行下一行。把asyncio.sleep看成一个耗时1秒的IO操作，祝线程未等待，而是去执行EventLoop中其他可以执行的coroutine了，因此可以实现并发。



### asyncio + async/await

- Python3.5后引入async/await,让coroutine更加简洁易读。

  - @asyncio.coroutine换成async
  - 把yield from换成await

  ```python
  import asyncio
  async def test(i):
      print('test_1',i)
      await asyncio.sleep(1)
      print('test_2')
  
  if __name__ == '__main__':
      loop = asyncio.get_event_loop()
      tasks = [test(i) for i in range(3)]
      loop.run_until_complete(asyncio.wait(tasks))
      loop.close()
  ```



### Gevent

- 是一个基于Greenlet实现的网络库，通过greenlet实现协程。基于一个greenlet就认为是一个协程，当一个greenlet遇到IO操作，就会自动切换到其他greenlet，等到IO操作完成，再切回来。而gevent帮我们自动切换，保证总有greenlet在运行，不去等待IO。

```python
# coding:utf-8
import gevent

def test(n):
    for i in range(n):
        print(gevent.getcurrent(), i)
        # 模拟交出控制权
        gevent.sleep(1)

if __name__ == '__main__':
    gevent.joinall([
        gevent.spawn(test,3),
        gevent.spawn(test,3),
        gevent.spawn(test,3)
    ])
```

## Django

### 组件介绍

- 基本配置文件
- 路由系统
- 模型层：连通数据库
- 模板层：渲染页面
- 视图层
- Cookies和Session
- 分页和发邮件
- 管理后台

### 安装Django

```shell
pip install django==2.2.12
# 检查是否安装完成
pip freeze|grep -i 'Django'
```



### 项目结构

- 创建项目

```shell
django-admin startproject proName
```

- 启动服务

```shell
python manage.py runserver
python manage.py runserver port_num
```

- 结束服务

```shell
# 查询id
sudo lsof -i:8000
kill -9 id 
```

创建后的文件树

- HelloWorld

  - db.sqlite3

  - Manage.py

  - HelloWorld

    - \__init__.py:python包的初始化文件
    - settings.py：项目的配置文件
    - urls.py：项目的路由配置
    - wigs.py：web服务网关的配置文件

    

- Manage.py 子命令入口

```shell
python manage.py
python manage.py runserver
# 创建应用
python manage.py startapp
# 数据库迁移
python manage.py migrate
```

- Settings.py
  - 命名全部需要大写
  - 引入方式 from django.conf import settings
  - BASE_DIR:当前根目录
  - DEBUG：True调试模式
  - ALLOWED_HOST:过滤HOST头，最好配置具体域名。过滤垃圾请求。也可以放局域网内的内网地址。
  - ROOT_URLCONF:主路由配置
  - TEMPLATES：html相关配置
  - DATABASES：配置数据库
  - TIME_ZONE：Asia/Shanghai

### URL和视图函数

URL：统一资源定位符，用来标识互联网上某个资源的地址

- Protocol://hostname[:port]/path\[?query]\[#fragment] 
- 端口默认80
- path路由地址：决定服务器如何处理请求
- query查询：动态网页传递参数
- fragment相当于页面中的锚点



处理URL请求的过程：

- 浏览器输入http://127.0.0.1:8000/page/2003/
- 在配置文件中ROOT URLCONF找到主路由文件。默认情况下在xxx/xxx/urls.py
- Django加载主路由文件中urlpatterns变量
- 依次匹配urlpatterns中的path，匹配到第一个合适的中断
- 匹配成功，调用对应的视图函数，返回响应
- 匹配失败，404

视图函数：用于接受http请求并通过httpresponse对象返回响应的函数。

```python
# 从浏览器接受到http对象
def xxx_view(request,[,xxx]):
    return HttpResponse对象
```

```python
# sample_1  HelloWorld/views.py
from django.http import HttpResponse
def page1_view(request):
    html = "<h1>aaa</h1>"
    return HttpResponse(html)
```

```python
# urls.py
from django.contrib import admin
from django.urls import path
from . import views 
urlpatterns = [
    path('admin/', admin.site.urls),
    path('page/xxx/', views.page1_view)
]
```



### 路由配置

- path转换器：<转换器类型：自定义名称>
  - 作用：如果转换器类型匹配到对应类型的数据，则将数据按照关键字传参的方式传递给视图函数
  - path('page/\<int:page>',views.xxx)
  - 转换器类型int str slug path
- re_path转换器：
  - 在url中可以使用正则表达式进行精准匹配
  - re_path(reg, view,name='xxx')
  - 采用命名分组(?P\<name>Pattern)
    - re_path(r'^(P\<x>\d{1,2})/(?P\<op>\w+)/(?P\<y>\d{1,2})$',views.cal_view)
    - from django.urls import re_path



### 请求和响应

- 请求是浏览器通过http协议发送给服务器端的数据
- 响应是服务器端接收到请求后做出相应的处理后，再回复浏览器端的数据
- Django接收到http请求后，会根据请求数据报文创建HttpResponse对象
- httpresponse对象通过属性描述了请求的所有相关信息



- Django中的请求request的属性
  - path_info:URL字符串
  - Method:http的请求方式
  - GET：QueryDict查询字典的对象，包含get请求方式的数据
  - POST：QueryDict查询字典的对象
  - FILES：类似于字典，包含所有上传信息
  - COOKIES：字典
  - session
  - body
  - get_full_path()
  - META:请求的元数据



- 响应
  - 状态码 HTTP Status Code
  - 常见状态码
    - 200成功
    - 301永久重定向
    - 302临时重定向
    - 404不存在
    - 500内部服务错误
  - 响应对象httpResponse
    - HttpResponse(content=xxx,content_type=text/html,status=200)
    - Content_type
      - Text/html
      - Text/javascript
      - Text/css
      - text/json
    - 子类httpResponseRedirect...



### GET和POST

- 无论什么请求都会在视图函数接受

```python
if request.method == 'GET':
    xxx
elif request.method == 'POST':
    xxx
```

- GET

  - 一般用于向服务器获取数据
  - 场景
    - 在地址栏输入URL
    - 网页超链接
    - form表单中的method是get
  - 获取get参数
    - request.GET['xxx']
    - request.GET.get()
    - request.GET.getlist('xxx')

- POST

  - 一般向服务器提交大量/隐私数据
  - 在form表单中指明路由

  ```html
  <form method='post' action='/login'>
    xxx
  </form>
  ```

  - 服务器接收参数
  - 取消csrf验证，否则POST的时候django会让其403
    - setting.py中的MIDDLEWARE中关于csrf的注释掉



### 设计模式

- MVC架构和MTV架构
  - MVC
    - model：模型层对数据库层进行封装
    - view：用于向用户展示结果，展示什么，怎么展示
    - controller：用于处理请求 获取数据 返回结果
    - 降低耦合度
  - MTV模式
    - model：负责和数据库进行交互
    - template：负责呈现内容到浏览器，怎么呈现数据html
    - view：是核心，负责接收请求，获取数据，返回结果

### 模版配置

- 创建文件夹xxx/templates
- Settings.py中的templates配置项
  - BACKEND：模版引擎
  - DIRS：模板的搜索目录
    - Os.path.join(BASE_DIR,'templates')
  - APP_DIRS：是否要在应用中查找

### 模板加载方式

- loader方法

```python
from django.template import loader
t = loader.get_template('xxx')
html = t.render(字典数据)
return HttpResponse(html)
```

- render直接加载和响应模板

```python
from django.shortcuts import render
return render(request,'模板文件名',dict_data)
```



### 视图和模板的交互

- 视图函数中把字典传递到模板

```python
def xxx_view(request):
    dic = {'xxx':'xx'}
    return render(requset,'xxx.html',dic)
```

- 模板中用{{ 变量名 }}调用视图函数传递过来的参数



### 模板层的标签和变量

- 能传递到模板的数据类型
  - Str/int/list/tuple/dict/func/obj
  - {{ 变量名 }}/{{ 变量名.index }}/{{ 对象.方法 }}/{{ 函数名 }}

```python
def test_html_param(request):
    dic = {}
    dic['int'] = 123
    dic['str'] = 'asd'
    dic['lst'] = ['a','b','c']
    dic['dict'] = {'a':1,'b':2}
    dic['func'] = say_hi
    dic['obj'] = Dog()
    return render(request,'test_html_param.html',dic)
    # 封装局部变量成一个字典  locals
    # return render(request,'xxx.html',locals())
```

- 模板标签

  - {% 标签 %} {% 结束标签 %}

    - {% if %}{% elif %}{% endif %}{% for %}

    ```html
    {% for xx in 可迭代对象 %}
        。。。。
    {% empty %}
        可迭代对象无数据时填充的语句
    {% endfor %}
    ```

  - 内置变量
    - forloop.counter
    - forloop.counter0
    - forloop.recounter
    - forloop.first
    - forloop.last
    - forloop.parentloop

### 模板层的过滤器和继承

过滤器

- 定义：在变量输出时对变量的值进行处理
- {{ 变量｜过滤器1:'xxx'｜过滤器2:'xxx' }}
- 高频使用过滤器
  - lower
  - upper
  - safe：不进行html转义
  - Add:'n'
  - Truncatechars:'n'



继承

- 模板继承可以使父模板的内容重用，子模板直接继承父模板的全部内容，并且可以覆盖父模板中的块
- 父模板中
  - 定义父模板中的块block标签
  - 标识出那些在子模板中是允许被修改的
  - block标签：在父模板中定义，可以在子模板覆盖
- 子模板：
  - 继承模板extends标签
    - {% extends 'xxx.html' %}
    - 重写父模板中的内容块,在父模板中
      - {% block block_name %}
      - {% endblock %}



### URL反向解析

- 绝对地址：http://127.0.0.1:8000/page
- 相对地址：page/1或者/page/1
  - 通常用/page/1



- 反向解析是指在视图或者模板中，用path定义的名称来动态查找相应的路由
  - path(route, views, name='xxx')
  - 根据path的name给url确定了一个唯一的名字，在模板或者视图中，可以通过name来反向推断url信息。
- 模板中
  - {% url 'xxx' %}
  - {% url 'xxx' age='18' %} 其他参数
- 视图中

```python
from django.urls import reverse

print(reverse('xxx', args=[300]))
print(reverse('xxx', kwarys = {}))
>>>获得反向解析出来的地址
```



### 静态文件

- 图片 css js等
- 配置静态文件的访问路径
  - 通过哪个url地址来找静态文件
  - STATIC_UTR = ‘/static/’
- 配置静态文件的存储路径
  - STATICFILES_DIRS = (xxx)
- 模板中通过标签访问静态文件
  - 加载static - {% load static %}
  - {% static '路径' %}

### 应用和分布式路由

- 应用就是单独的业务模块，有自己的路由 视图 模板 模型

```shell
python manage.py start app music
```

- 在setting.py的INSTALLED_APPS进行应用添加



- 分布式路由
  - 主路由可以不处理用户具体的路由，做请求的分发。
  - 主路由中include函数
  - include('app名字.url模块名')

```python
# 主路由
from django.urls import path , include
from . import views

urlpatterns = [
    path('music/',include('music.urls'))
]
```



- 应用下的模板
  - 手动创建templates
  - setting.py开启模板功能
    - APP_DIRS=TRUE
  - 应用的templates和外层的都存在
    - 优先查找外层templates下的模板
    - 然后按INSTALLED_APP顺序查找
  - 解决这种查找方式
    - 可以在app下的templates/app_name 这样注册文件夹

### 模型层

- 负责和数据库进行通信

```python
# 在setting.py的同级__init__里面
import pymysql
pymysql.install_as_MySQLdb()
```

- 创建数据库

  - 进入mysql数据库执行
    - create database 数据库名 default charset=utf8;
    - 通常数据库名和项目名保持一致
  - setting.py进行数据库的配置
    - 修改DATABASES，sqlite3改成mysql

  ```python
  DATABASES = {
      'default': {
          'ENGINE': 'django.db.backends.mysql',
          'NAME': 'helloworld',
          'USER': 'root',
          'PASSWORD': 'xxxxxxxx',
          'HOST': '127.0.0.1',
          'PORT': '3306'
      }
  }
  # 支持mysql/sqlite3/oracle/postgresql
  ```



- 什么是模型
  - 模型是python类，django.db.models.Model派生出的子类
  - 一个模型类代表数据库的一张数据表
  - 模型类中每一个类属性都代表数据库中的一个字段
  - 模型是数据交互的接口，是标识和操作数据库的方法和方式



### ORM框架

- 定义：Object Relational Mapping 即对象关系映射，它是一种程序技术，允许你使用类和对象对数据库进行操作，从而避免通过sql语句操作数据库
- 作用
  - 建立模型类和表之间的对应关系，允许我们通过面向对象的方法来操作数据库
  - 根据设计的模型类生成数据库中的表格
  - 通过简单的配置就可以进行数据库切换
- 优点
  - 只需要面向对象编程，不用面向数据库写代码
    - 对数据库的操作都转化成对类属性和方法的操作
    - 不用编写各种数据库sql语句
  - 实现了数据模型和数据库的解耦，屏蔽了不同数据库操作上的差异
- 缺点
  - 复杂业务使用成本高
  - 映射过程中有性能损失

```python
# 模型类
# bookstore/model.py
from django.db import models
class Book(models.Model):
    title = models.CharField("书名", max_length=50, default='')
    price = models.DecimalField('定价', max_digits=7, decimal_places=2, default=0.0)
```

- 数据库迁移
  - 迁移时Django同步我对模型所做更改到数据库模式的方式
    - 生成迁移文件：python manage.py makemigrations
      - 将应用下的models.py文件生成一个中间文件，并保存在migrations文件夹
    - 执行迁移：python manage.py migrate
      - 把每个应用下的migrations目录中的中间文件同步会数据库



### 模型类的基础操作

- 表结构修改
  - 添加info字段varchar(100)
  - 解决方案
    - 模型类加类属性
    - 执行数据库迁移
  
- 字段类型
  - BooleanField
    - 数据库类型：tinyint(1)
    - 编程中用True/False
    - 数据库中使用1/0
  - CharField
    - 数据库类型：varchar
    - 必须指定max_length参数
  - DateField
    - 数据库类型：date
    - 表示日期
    - 参数 三选一
      - auto_now:每次保存，自动设置字段成当前时间True
      - Auto_now_add：对象第一次被创建时自动设置当前时间True
      - default：设置当前时间
  - DateTimeField
    - 数据库类型：datetime
    - 表示日期时间
    - 参数和DateField相同
  - FloatField
    - double
    - 小数表示
  - DecimalField
    - 数据库类型：decimal
    - 使用小数，和钱相关
    - 参数
      - Max_digits：总的位数
      - Decimal_places:小数点后的数量
  - EmailField
    - 数据库类型：varchar
  - IntegerField
    - 数据库类型：int
    - 编程语言和数据库都用整数
  - ImageField
    - 数据库类型：varchar
    - 作用
      - 储存图片的路径
      - 使用字符串
  - TextField
    - 数据库类型：longtext
    - 作用：表示不定长的字符数据
  
- 字段选项
  - 指定创建的列的额外信息
  - 允许出现多个字段选项
  - primary_key
    - 表示为主键，则数据库不会创建id字段
  - black
    - True，字段可以为空，否则必须填写
  - null
    - True，列的值可以空
  - default
    - 设置所在列的默认值，如果字段null=False建议添加此项
  - db_index
    - 如果true，该列增加索引
  - unique
    - True，字段在数据库中的值必须时唯一
  - Db_column
    - 指定列名
  - Verbose_name
    - 设置字段在admin界面上的名称
  
  ```python
  name = models.CharField(max_length=30, unique=True, null=False, db_index=True)
  ```



### 模型类-Meta类

- 使用Meta类来给模型赋予属性，meta类下有很多内建的类属性，可以对模型类做一些控制。

```python
from django.db import models

class Book(models.Model):
    title = models.CharField("书名", max_length=50, default='')
    price = models.DecimalField('定价', max_digits=7, decimal_places=2, default=0.0)
    info = models.CharField('描述', max_length=100,default='')

    class Meta:
        # 模型类的表名
        db_table = 'book'
        # 给模型对象的一个容易理解的名称，在admin显示
        verbose_name = 'xxx'
        verbose_name_plural = 'xxx'
```



### ORM创建数据

- 数据库中Django_migrations表记录了migrate的全过程，项目各应用中migrate文件应该与之对应，否则migrate会报错
  - 删除迁移文件
  - 删除数据库
  - 重建数据库和迁移文件



- CRUD操作->模型类的管理器对象
- 管理器对象
  - 每一个继承models.Model的模型类，都会有一个objects对象被继承下来

```python
class MyModel(models.Model):
    xxx
MyModel.objects.create()
```



- 创建数据
  - MyModel.objects.create(属性1=xxx)
    - 成功就返回创建好的实体对象
  - 创建实例对象，调用save保存
    - obj.属性=xxx
    - obj.save()

- Django Shell
  - 可以替代view进行直接操作
  - python manage.py shell



### ORM查询数据

- 通过MyModel.objects管理器方法进行查询
  - all
    - MyModel.objects.all()
    - select * from table 
    - 返回QuerySet容器对象，内部存放MyModel实例
  - values
    - MyModel.objects.values()
    - Select 列1，列2 from xxx
    - 返回QuerySet，内部存放字典
      - {列1:xxx, 列2:xxx}
  - value_list
    - MyModel.objects.value_list()
    - 和values区别在返回内部是元组，得用索引去拿了
  - order_by
    - MyModel.objects.order_by('列名','-列名')
      - 默认是升序，降序加-
    - 会用sql的ORDER BY子句进行查询
    - 返回内部是实例
    - Book.objects.values('title').order_by('-price')
      - 内部存放就会变成字典了
  - obj.query
    - 查询原始sql语句
  - filter
    - MyModel.objects.filter()
    - 返回包含此条件的数据集合
    - 返回值
      - QuerySet容器对象，内部存放实例
    - 多个属性是and关系
  - exclude
    - 返回不包含此条件的数据集合
  - get
    - 返回满足条件的一条数据
    - 查询结果大于一条会报错，没有也rasie
- 查询谓词
  - __exact:等值匹配
    - author.objects.filter(id__exact=1)
    - select * from author where id = 1
  - __contains:包含指定值
    - Author.objects.filter(name__contains='w')
    - select * from author where name like '%w%'
  - __startwith
  - __endwith
  - __gt:大于指定值
  - __gte:大于等于
  - __lt:小于
  - __lte：小于等于
  - __in：指定范围
    - Author.objects.filter(country__in=['xxx','xxx])
  - __range
    - author.objects.filter(age__range=(10,20))
    - select * from author between 10 and 20;



### ORM更新操作

- 修改单个实体的某些字段
  - 查：通过get获取实体对象
  - 改：对象.属性修改
  - 保存：save方法
- 批量更新数据
  - 调用Queryset的update(属性=值)

```python
books = Book.objects.filter(id__gt=3)
books.update(price=0)
```



### ORM删除操作

- 单个数据
  - 查找查询结果对应的一个数据对象
  - 调用delete方法删除

```python
try:
    auth = author.objects.get(id=1)
    auth.delete()
except:
    print('fail')
```

- 批量数据
  - 获得QuerySet对象
  - 也用delete()
- 伪删除
  - 通常不会真的吧数据删除
    - 在表中添加布尔字段is_active,默认为true
    - 显示数据的时候，要过滤查询



### F对象和Q对象

- 一个F对象代表数据库中某条记录的字段信息
  - 作用
    - 通常是对数据库中的字段值在不获取的情况下进行操作
    - 用于类属性之间的比较

```python
from django.db.models import F
F('列名')

# 给价格+10
Books.objects.all().update(market_price=F('market_price')+10)

# 不用F
books = Book.objects.all()
for book in books:
    book.market_price = book.market_price+10
    book.save()
```

- 在不用F对象的时候存在一个资源竞争问题 并发问题

```python
def add_like(request, topic_id):
    topic = Topic.objects.get(id=topic_id)
    # 更新 量大时 没有进行锁操作 会导致产生脏数据
    new_like = topic.like+1
    topic.like = new_like
    topic.save()
    # 相当于
    # update topic set like = 1 where id = xxx
    
    # 使用F对象
    topic.like = F('like')+1
    topic.save()
    # 相当于
    # update topic set like = like + 1 where id = xxx
```



- Q对象
  - 当获取查询结果使用复杂的逻辑或/逻辑非等可以使用

```python
from django.db.models import Q
Book.objects.filter(Q(price__lt=20)|Q(pub-'xxx'))
Book.objects.filter(Q(price__lt=20)&Q(pub-'xxx'))
Book.objects.filter(Q(price__lt=20)|~Q(pub-'xxx'))
```



### 聚合查询和直接使用数据库查询

- 聚合查询
  - 针对一个表的某个字段进行统计
  - 整表聚合
    - from django.db.models import *
    - sum，avg，count，max，min
    - MyModel.objects.aggregate(聚合后列名=聚合函数('列名'))
      - 返回字典。变量名：值
  - 分组聚合
    - 基于某个字段分组，对每个组的数据聚合
    - QuerySet.annotate(聚合后列名=聚合函数('列名'))
      - 返回QuerySet
    - 用MyModel.objects.values()进行分组
      - 返回了一组QuerySet
      - 然后进行聚合



- 原生数据库操作
  - 返回RawQuerySet对象

```python
# 用来进行查询的方法raw
books = models.Book.objects.raw('select * from bookstore_book')
for book in book:
    xxxx
```

- 避免sql注入，尽量别用

```python
# 防止注入方式，让django帮忙转义
Book.objects.raw('select * from bookstore_book where id=%s',['1 or 1 = 1'])
```



- 跨过模型类，使用游标链接数据库
  - from django.db import connection

```python
with connection.cursor() as cur:
    cur.execute('sql','params')
```



### Admin管理后台

- 提供了比较完善的后台管理数据库的接口，可以提供开发过程中调用和测试使用

- django会收集已经注册的模型类，为这些模型提供数据库管理界面

- 创建后台管理账号

  - 这个账号是管理后台最高权限账号
  - python manage.py createsuperuser

- 注册自定义模型类

  - 在app的admin.py中导入注册要管理的模型models类

  - from .models import Book

  - Admin.site.register(models类)

  - 后台对象的样式

    - 可以通过修改模型类的\__str__修改

  - 使用模型管理器类改变admin里的对象样式

    - 在<应用app>/admin.py定义模型管理器类

      - class xxxManager(admin.ModelAdmin)

    - 绑定注册模型管理器和模型类

    - ```python
      from django.contrib import admin
      from .models import *
      admin.site.register(model,modelmanager)
      ```

    - ```python
      from django.contrib import admin
      from .models import Book
      
      class BookManager(admin.ModelAdmin):
          list_display = ['id','title','price']
          # 链接到修改页面
          list_display_links = ['title'] 
          # 添加过滤器
          list_filter = ['pub']
          # 添加搜索框
          search_fields = ['title'] 
          # 添加可在列表页编辑的字段
          list_editable = ['price']
      ```

      



### 关系映射

- 一对一映射/一对多映射/多对多映射

- 一对一映射

  ```python
  # on_delete级联删除
  OneToOneField(类名, on_delete=xxx)
  ```

  - 级联删除模式

    - models.CASADE ：模拟SQL约束ON DELETE CASEADE行为，并且删除包含ForeignKey的对象
    - Modes.PROTECT：抛出error阻止被引用对象的删除
    - SET_NULL设置ForeignKey null，需要指定null=True
    - SET_DEFAULT设置ForeignKey为默认值

  - ```python
    # 无外键的模型类
    author1 = Author.objects.create(name='xxx')
    # 有外键的模型类
    # 关联对象
    wife1 = Wife.objects.create(name='xxx',author=author1)
    # 关联对应的主键
    wife1 = Wife.objects.create(name='xxx',author_id=1)
    ```

  - 正向查询：通过外键属性查询

    - ```python
      from .models import Wife
      wife = Wife.objects.get(name='xxx')
      print(wife.author.name)
      ```

  - 反向查询：没有外键的一方，调用反向关联属性(类的小写)

    - ```python
      from .models import Author
      author = Author.objects.get(name='xxx')
      print(author.wife.name)
      ```

- 一对多：一个班级多个学生

  - 一对多要明确角色，在多表上设置外键

  - ```python
    from django.db import models
    
    class Publisher(models.Model):
        name = models.charField('name',max_length=50,unique=True)
    
    class Book(models.Model):
        title = models.charField('bookname',max_length=50)
        publisher = Foreighkey(Publisher, on_delete=models.CASCADE)
    ```

  - 创建数据，先创建一，再创建多

    - ```python
      from .model import *
      
      pub1 = Publisher.objects.create(name='xxx')
      Book.objects.create(title='c++',publisher=pub1)
      Book.objects.create(title='java',publisher_id=1)
      ```

    - 正向查询

      - ```python
        abook = Book.objects.get(id=1)
        print(abook.publisher.name)
        ```

    - 反向查询

      - ```python
        pub1 = Publisher.objects.get(name='xxx')
        # 小写的类名_set
        books = pub1.book_set.all()
        ```

  - 多对多

    - mysql需要第三张表实现，进行解耦

    - ```python
      from django.db import models
      class Author(models.Model):
          name = models.CharField('name',max_length=11)
      class Book(models.Model):
          title = models.CharField('bookname',max_length=11)
          authors = models.ManyToManyField(Author)
      ```

    - 创建数据

      - ```python
        # 先创建author
        author1 = Author.objects.create(name='111')
        author2 = Author.objects.create(name='222')
        book11 = author1.book_set.create(title='python')
        author2.book_set.add(book11)
        
        # 先创建book
        book = Book.objects.create(title='python1')
        author3 = book.authors.create(name='333')
        book.authors.add(author1)
        ```

      - 正向查询

        - ```python
          book.author.all()
          book.authors.filter(age__gt=80)
          ```

      - 反向查询

        - ```python
          author.book_set.all()
          author.book_set.filter()
          ```

          

### Cookies/Session

- 会话

  - 从打开浏览器访问一个网站，到关闭浏览器结束访问，为一次会话
  - HTTP协议是无状态的
  - cookies和session是为了保存状态而产生的存储技术

- Cookies是保存在客户端浏览器上的存储空间

  - 在浏览器上以键值对进行存储-ASCII码

  - 存储数据有生命周期

  - cookie数据是按域存储隔离的

  - cookies的内部数据会每次访问网址的时候携带到服务器端，如果cookies过大会降低响应速度

  - ```python
    # max_age:最大存活描述
    # expires:具体过期时间
    HttpResponse.set_cookie(key, value='', max_age=None, expires=None)
    ```

  - ```python
    # 添加cookie
    responds = HttpResponse('xxx')
    responds.set_cookie('my_val',123,3600)
    return responds
    ```

  - 删除cookies

    - ```python
      HttpResponse.delete_cookie(key)
      ```

  - 获取cookies

    - ```python
      value = request.COOKIES.get()
      ```

- Session把数据存在服务器

  - 使用session需要在浏览器客户端启动cookie，在cookie中储存sessionid

  - 每个客户端都可以在服务器有独立的session

  - session设置

    - INSTALLED_APPS = ['django.contrib.sessions']
    - MIDDLEWARE = ['django.contrib.sessions.middleware.Sessionmiddleware']

  - 使用

    - ```python
      request.session['KEY']
      request.session.get('key','default')
      
      del request.session['KEY']
      ```

    - setting配置

      - SESSION_COOKIE_AGE= 60* 60 *24 * 7 * 2
      - SESSION_EXPORE_ART_BROWSER_CLOSER = TRUE：浏览器关闭就失效

    - django_session是单表设计 ,并且不会删除过期数据

      - python manage.py clearsessions 删除过期数据