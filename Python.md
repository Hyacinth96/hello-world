[TOC]

# 易错点

## is和==的区别

+ `is`表示的是对象标识符，用来检查对象的标识符是否一致，即两个对象在内存中的地址是否一样

  在使用`a is b`的时候，相当于使用`id(a) = id(b)`

+ `==`表示两个对象是否相等，相当于调用`__eq__()`方法，即`a == b`

+ None在Python中是个单列对象，一个变量如果是None，它一定和None指向同一个内存地址

  None是Python中的一个特殊的常量，表示一个空的对象，空值是python中的一个特殊值。

  **数据为空不代表是空对象，**例如`[]`、`''`等都不是None

+ None和除自身以外的任何对象比较返回值都是False

```python
a = None
b = None
print(id(a) == id(b))  # True
```

+ `is None`判断两个对象在内存中的地址是否一致，`== None`调用`__eq__()`，而`__eq__()`可以被重载

```python
class test():
    def __eq__(self, other):
        return True
t = test()
print(t is None)  # False
print(t == None)  # True
```

## (&, |)和(and, or)的区别

+ 在对逻辑变量进行运算时，两类用法基本一致

```python
(3 > 0) & (3 < 1)  # False
(3 > 0) and (3 < 1)  # False
(3 > 0) | (3 < 1)  # True
(3 > 0) or (3 < 1)  # True
```

+ 对布尔值数组而言，(and, or)并没有用，要使用(&, |)

```python
names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])
mask = (names == 'Bob') | (names == 'Will')
mask  # array([ True, False,  True,  True,  True, False, False])
```

# 数据结构、函数和文件

## 内置函数
### int()
向下取整
```python
int(1.7)  # 1
```
### sum()
参数为可迭代对象，如：列表、元组、集合
```python
sum([1, 2, 3])  # 6
sum([1, 2, 3], 3)  # 9
```
### round()
四舍五入
```python
round(4.3)  # 4
round(4.9)  # 5
round(x, d)  # 解决“不确定尾数”问题，奇进偶不进
0.1 + 0.2 == 0.3  # False
round(0.1+0.2, 3) == 0.3  # True
round(0.5)  # 0
round(1.5)  # 2
round(1.51)  # 2
```
### floor()
向下取整
```python
from math import *
floor(4.7)  # 4
```
### ceil()
向上取整
```python
from math import *
ceil(4.1)  # 5
```
### modf()
分别取小数和整数部分，以元组类型表示
```python
from math import *
modf(1.26)  # (0.26, 1.0)
```
### pow(x, y, z) 
幂运算为x ** y
幂运算取模的运行速度更快，等价于 x ** y % z
### type()
判断变量类型
### strip()
用于移除字符串头尾指定的字符（默认为空格或换行符）或字符序列，不能删除中间部分的字符
```python
string = '123456789abc987654312'
print(string.strip('123'))
```
### divmod(x,y)
返回元组类型（商，余数）
```python
divmod(100, 9)  # (11, 1)
```
### // 求商，% 求余数
### map()
`map()`函数是python内置的高阶函数，对传入的list的每一个元素进行映射，返回一个新的映射之后的list
map函数返回的是一个map对象，需要通过`list(map(function, iterable, ...))`来将映射之后的map对象转换成列表

>语法：`map(function, iterable, ...)`
```python
list(map(lambda x: x ** 2, [1, 2, 3, 4]))  # [1, 4, 9, 16] 
```

提供两个列表，对相同位置的列表数据进行相加

```python
list(map(lambda x, y: x + y, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10]))  # [3, 7, 11, 15, 19]
```

### count()

计数

```python
my_list = [1, 2, 3, 3, 4, 5, 6]
my_list.count(3)  # 2
my_strings = 'hello'
my_strings.count('l')  # 2
```

## 模块

###  itertools

标准库中的`itertools`模块是适用于大多数数据算法的生成器集合

#### groupby()

可以根据任意的序列和一个函数，通过函数的返回值对序列中连续的元素进行分组

```python
first_letter = lambda x: x[0]
name_list = ['Alan', 'Adam', 'Wes', 'Will', 'Albert', 'Steven']
for letter, names in itertools.groupby(name_list, first_letter):
    print(letter, list(names))
#    A ['Alan', 'Adam']
#    W ['Wes', 'Will']
#    A ['Albert']
#    S ['Steven']
```

![](images/Table 3-2. Some useful itertools functions.png)

## 数据结构和序列

### 元组

创建最简单的元组，是用逗号分隔一列值

```python
tup = 4, 5, 6
# 等价于
tup = (4, 5, 6)
tup  # (4, 5, 6)
```

当元组中只有一个元素时，该元素后必须加逗号，否则会被视为int

```python
a = (2,)
type(a)  # tuple
a = (2)
type(a)  # int
```

#### 拆分元组

变量拆分常用来迭代元组或列表序列

```python
seq = [(1, 2, 3), (4, 5, 6)]
for a, b, c in seq:
    print('a={0}, b={1}, c={2}'.format(a, b, c))
# a=1, b=2, c=3
# a=4, b=5, c=6
```

从元组的开头“摘取”元素。rest的部分是想要舍弃的部分，rest的名字不重要，也可用*_

```python
values = 1, 2, 3, 4, 5
# a, b, *_ = values 有时也用下划线(_)表示不要的变量
a, b, *rest = values
a, b  # (1, 2)
rest  # [3, 4, 5]
```

#### count计数

```python
a = (1, 2, 2, 2, 3, 4, 2)
a.count(2)  # 4
```

list函数常用来在数据处理中实体化迭代器或生成器

```python
list(range(10))  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
```

### 列表

#### extend() 

追加列表元素/列表相加

```python
# 比较append()和extend()添加列表元素的区别
ls = [4, None, 'foo']
ls.append([7, 8, (2, 3)])
ls  # [4, None, 'foo', [7, 8, (2, 3)]] 添加单一元素

ls = [4, None, 'foo']
ls.extend([7, 8, (2, 3)])
ls  # [4, None, 'foo', 7, 8, (2, 3)] 添加多个元素
```

如果已经定义了一个列表，用extend方法可以追加多个元素，效率比用加法串联要快

（因为串联法要新建一个列表，并且要复制对象）

> 语法
>
> ```python
> everything = []
> for chunk in list_of_lists:
>  everything.extend(chunk)
> 
> # 上述实现比下述实现更快
> everything = []
> for chunk in list_of_lists:
>  everything.extend + chunk
> ```

```python
# extend()效率更高，推荐
ls = [4, None, 'foo']
ls.extend([7, 8, (2, 3)])
ls  # [4, None, 'foo', 7, 8, (2, 3)]

# 串联法，效率低
[4, None, 'foo'] + [7, 8, (2, 3)]  # [4, None, 'foo', 7, 8, (2, 3)]
```

 例：提取包含两个或两个以上**e**的名字

```python
all_data = [['John', 'Emily', 'Michael', 'Mary', 'Steven'],
            ['Maria', 'Juan', 'Javier', 'Natalia', 'Pilar']]
names_of_interst = []
for names in all_data:
    enough_es = [name for name in names if name.count('e') >= 2]
    names_of_interst.extend(enough_es)
names_of_interst  # ['Steven']
```

#### sort(key= ) 排序

sort函数将一个列表原地排序（不创建新的对象），二级排序key

```python
b = ['saw', 'small', 'He', 'foxes', 'six']
b.sort(key=len)
b  # ['He', 'saw', 'six', 'small', 'foxes']
```

#### 二分搜索和维护已排序的列表

用于已排序的列表。bisect模块不会检查列表是否已排好序，对未排序的列表使用bisect不会报错，但可能导致不正确的结果

`bisect()`用于查找该数值将会插入的位置并返回，而不会插入；`insort()`用于在不影响原有的排序的前提下插入元素

```python
import bisect
c = [1, 2, 2, 2, 3, 4, 7]
bisect.bisect(c, 2)  # 4
bisect.insort(c, 6)
c  # [1, 2, 2, 2, 3, 4, 6, 7]
```

#### enumerate()

返回(index, value)元组序列

```python
some_list = ['foo', 'bar', 'baz']
mapping = {}
for i, v in enumerate(some_list):
    mapping[v] = i
mapping  # {'foo': 0, 'bar': 1, 'baz': 2}

# 创建一个字符串的查找映射表以确定它在列表中的位置
strings = ['a', 'as', 'bat', 'car', 'dove', 'python']
loc_mapping = {value: index for index, value in enumerate(strings)}
loc_mapping  # {'a': 0, 'as': 1, 'bat': 2, 'car': 3, 'dove': 4, 'python': 5}
```

#### sort()与sorted()区别

`sort()`对list排序会修改list本身，不会返回新list；`sort()`不能对dict字典进行排序

`sorted()`会生成一个新的列表或字典对象；对dict排序默认会按照dict的key值进行排序，最后返回的结果是一个对key值排序好的list

```python
# sort()
my_list = [3, 5, 1, 4, 2]
my_list.sort()
my_list  # [1, 2, 3, 4, 5]
my_dict = {"a": "1", "c": "3", "b": "2"}
my_dict.sort()  # 报错 AttributeError: 'dict' object has no attribute 'sort'

# sorted()
sorted('horse race')  # [' ', 'a', 'c', 'e', 'e', 'h', 'o', 'r', 'r', 's'] 
my_list = [3, 5, 1, 4, 2]
sorted(my_list)  # [1, 2, 3, 4, 5]
my_list  # [3, 5, 1, 4, 2]
my_dict = {"a": "1", "c": "3", "b": "2"}
sorted(my_dict)  # ['a', 'b', 'c']
my_dict  # {'a': '1', 'c': '3', 'b': '2'}
```

#### zip()

zip可以将多个列表、元组或其他序列成对组合成一个元组列表

zip可以处理任意多的序列，元素的个数取决于最短的序列

```python
seq1 = ['foo', 'bar', 'baz']
seq2 = ['one', 'two', 'three']
zipped = zip(seq1, seq2)
list(zipped)  # [('foo', 'one'), ('bar', 'two'), ('baz', 'three')]
seq3 = [False, True]
list(zip(seq1, seq2, seq3))  # [('foo', 'one', False), ('bar', 'two', True)]
```

zip与enumerate连用

```python
for i, (a, b) in enumerate(zip(seq1, seq2)):
    print('{0}: {1}, {2}'.format(i, a, b))
# 0: foo, one
# 1: bar, two
# 2: baz, three
```

拆分序列

```python
pitchers = [('Nolan', 'Ryan'), ('Rpger', 'Clemens'), ('Schilling', 'Curt')]
first_name, last_name = zip(*pitchers)
first_name  # ('Nolan', 'Rpger', 'Schilling')
last_name  # ('Ryan', 'Clemens', 'Curt')
```

### 字典

#### 有效的字典键类型

字典的值可以是任何的Python对象，但键必须是不可变的对象，如标量类型（整数、浮点数、字符串）或元组（且元组内对象也必须是不可变对象）

通过hash函数检查一个对象是否可以哈希化（即是否可以用作字典的键）

```python
hash('a string')  # -3912381382333166481
hash((1, 2, (2, 3)))  # 1097636502276347782
hash((1, 2, [2, 3]))  # 报错 TypeError: unhashable type: 'list'
```

检查字典中是否包含某个键

```python
d1 = {'a': 'some value', 'b': [1, 2, 3, 4]}
'b' in d1  # True
```

#### del / pop() 

用del关键字或pop方法（返回值的同时删除键）删除值

```python
d1 = {'a': 'some value', 'b': [1, 2, 3, 4]}
d1[5] = 'some value'
d1  # {'a': 'some value', 'b': [1, 2, 3, 4], 5: 'some value'}

d1['dummy'] = 'another value'
d1  # {'a': 'some value', 'b': [1, 2, 3, 4], 5: 'some value', 'dummy': 'another value'}

del d1[5]  # 删除键值对
d1  # {'a': 'some value', 'b': [1, 2, 3, 4], 'dummy': 'another value'}

ret = d1.pop('dummy')
ret  # 'another value'  删除键值对，并返回值
d1  # {'a': 'some value', 'b': [1, 2, 3, 4]}
```

#### Keys() / values()

`keys()`和`value()`函数分别返回字典对应的键和值

update()函数将两个字典融合

```python
d1 = {'a': 'some value', 'b': [1, 2, 3, 4], '7': 'an integer'}
list(d1.keys())  # ['a', 'b', '7']
list(d1.values())  # ['some value', [1, 2, 3, 4], 'an integer']
```

#### update()

将两个字典合并

对于任何原字典中已经存在的键，如果传给update方法的数据也含有相同的键，则它的值将会被覆盖

```python
d1.update({'b': 'foo', 'c':'hello world'})  # 'b'的值被覆盖
d1  # {'a': 'some value', 'b': 'foo', '7': 'an integer', 'c': 'hello world'} 
```

#### 从序列生成字典

```python
mapping = {}
key_list = ['a', 'b', '7']
value_list = ['some value', [1, 2, 3, 4], 'an integer']
for key, value in zip(key_list, value_list):
    mapping[key] = value
mapping  # {'a': 'some value', 'b': [1, 2, 3, 4], '7': 'an integer'}

# 字典的本质是2-元组的集合，字典是可以接受一个2-元组的列表作为参数的
mapping = dict(zip(range(5), reversed(range(5))))
mapping  # {0: 4, 1: 3, 2: 2, 3: 1, 4: 0}
```

#### get()

> 语法 `dict.setdefault(key, default)`

字典中存在键时返回对应的值，不存在时返回默认值；pop则会抛出一个异常

```python
d1 = {'a': 'some value', 'b': 'foo', '7': 'an integer', 'c': 'hello world'}
d1.get('a', 'NONE')  # 'some value' 返回值
d1.get('d', 'NONE')  # 'NONE' 不存在时返回默认值
```

#### setdefault()

> 基本语法 `dict.setdefault(key, default=None)`

字典中存在键时返回对应的值，不存在时返回默认值

#### defaultdict()

default_factory 接收一个工厂函数作为参数, 例如int, str, list, set等

`defaultdict`在dict的基础上添加了一个missing(key)方法, 在调用一个不存的key的时候，`defaultdict`会调用missing，返回一个根据default_factory参数的默认值，所以不会返回Keyerror

例：按首字母分类

```python
words = ['apple', 'bat', 'bar', 'atom', 'book']
by_letter = {}
for word in words:
    letter = word[0]
    if letter in by_letter:
        by_letter[letter].append(word)
    else:
        by_letter[letter] = [word]  # 此处为列表形式，否则对于字符串用append会报错
by_letter  # {'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}
```

`setdefault()`改进

```python
words = ['apple', 'bat', 'bar', 'atom', 'book']
by_letter = {}
for word in words:
    letter = word[0]
    by_letter.setdefault(letter, []).append(word)
by_letter  #  {'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']}
```

`defaultdict()`进一步改进

```python
from collections import defaultdict
words = ['apple', 'bat', 'bar', 'atom', 'book']
by_letter = defaultdict(list)
for word in words:
    by_letter[word[0]].append(word)
by_letter  # defaultdict(list, {'a': ['apple', 'atom'], 'b': ['bat', 'bar', 'book']})
```

### 集合

 ![](../../Typora Files/images/Table 3-1. Python set operations.png.jpg)

```python
a = {1, 2, 3, 4, 5}
b = {3, 4, 5, 6, 7, 8}
c = a.copy()
c |= b  # 取并集，并替代原集合
c  # {1, 2, 3, 4, 5, 6, 7, 8}
d = a.copy()
d &= b  # 取交集，并替代原集合
d  # {3, 4, 5}

# 检测一个集合是否为另一个集合的子集或父集
a_set = {1, 2, 3, 4, 5}
{1, 2, 3}.issubset(a_set)  # True
a_set.issubset({1, 2, 3})  # False 子集
a_set.issuperset({1, 2, 3})  # True  超集
a_set.isdisjoint({6, 7, 8})  # 没有交集返回True

# 检测两个集合是否相同
{1, 2, 3} == {2, 1, 3}  # True 
```

### 列表、集合和字典的推导式

列表推导式的for循环部分是根据嵌套的顺序排列的

例：提取包含两个或两个以上e的名字

```python
all_data = [['John', 'Emily', 'Michael', 'Mary', 'Steven'],
            ['Maria', 'Juan', 'Javier', 'Natalia', 'Pilar']]
names_of_interst = []
for names in all_data:
    enough_es = [name for name in names if name.count('e') >= 2]
    names_of_interst.extend(enough_es)
names_of_interst  # ['Steven']

# 等价于 用嵌套列表推导式的方法
all_data = [['John', 'Emily', 'Michael', 'Mary', 'Steven'],
            ['Maria', 'Juan', 'Javier', 'Natalia', 'Pilar']]
result = [name for names in all_data for name in names if name.count('e') >= 2]
result  # ['Steven']
```

将一个整数元组的列表扁平化成一个整数列表

```python
some_tuples = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]
flattened = [x for tup in some_tuples for x in tup]
flattened  # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

嵌套推导式的语法要和列表推导式中的列表推导式区分开

```python
some_tuples = [(1, 2, 3), (4, 5, 6), (7, 8, 9)]  # 将列表中的元组换成列表
[[x for x in tup] for tup in some_tuples]  # [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
```

## 函数

### 全局变量和局部变量

任何在函数中赋值的变量都是被分配到局部命名空间中的。在函数执行完毕后，局部命名空间就会被销毁。

1. 在模块层面定义的变量（无需global修饰），如果在函数中没有再定义同名变量，可以在函数中当做全局变量使用

   ```python
   a = 6
   def func():
       print(a)
   func()  # 6
   ```

2. 但如果在函数中有再赋值/定义（因为python是弱类型语言，赋值语句和其定义变量的语句一样），则调用函数时会产生引用了未定义变量的错误

   ```python
   a = 6
   def func():
       print(a)
       a = 2
   print(a)  # 6
   func()  # UnboundLocalError: local variable 'a' referenced before assignment
   ```

而如果在函数中的定义在引用前使用，那么会正常运行，但函数中的变量和模块中定义的全局变量不为同一个

```python
   a = 6
   def func():
       a = 2
       print(a)
   func()  # 2 局部变量
   print(a)  # 6  全局变量
```

3. 在函数内使用全局变量——在函数使用某一变量后又对其进行修改（即再赋值）

   ```python
   a = 6
   def func():
       global a  # 定义全局变量
       print(a)
       a = 2
   func()  # 6
   print(a)  # 2 全局变量的值被修改
   ```

   注意，global语句不允许同时进行赋值，如 global a = 2 是不允许的

### 返回多个值

return返回元组数据类型，也可用于返回字典

```python
def func():
    a = 5
    b = 6
    c = 7
    return {'a': a, 'b': b, 'c': c}
func()  # {'a': 5, 'b': 6, 'c': 7}
```

### 生成器 generator

直接作用于for循环的数据类型有以下几种：

+ 集合数据类型，如list、tuple、dict、set、str 等；
+ generator，包括生成器和带yield的generator function

这些可以直接作用于for循环的对象统称为可迭代对象：iterable

**1. 迭代器**

迭代器是一种用于在上下文中（比如for循环）向Python解释器生成对象的对象

在Python中如果一个对象有`__iter__( )`方法和`__next__( )`方法，则称这个对象是迭代器(Iterator)；其中`__iter__( )`方法是让对象可以用for ... in循环遍历，`__next__( )`方法是让对象可以通过next(实例名)访问下一个元素

注意：这两个方法必须同时具备，才能称之为迭代器。列表List、元组Tuple、字典Dictionary、字符串String等数据类型虽然是可迭代的，但都不是迭代器，因为他们都没有`next( )`方法

**2. 生成器**:

生成器是一个特殊的程序，可以被用作控制循环的迭代行为，python中生成器是迭代器的一种，使用yield返回值函数，每次调用yield会暂停，而可以使用`next()`和`send()`恢复生成器

生成器类似于返回值为数组的一个函数，这个函数可以接受参数，可以被调用。但是，不同于一般的函数会一次性返回包括了所有数值的数组，生成器一次只能产生一个值，这样消耗的内存数量将大大减小，而且允许调用函数可以很快地处理前几个返回值，因此生成器看起来像是一个函数，但是表现得却像是迭代器

**生成器创建方式**：把列表推导式两端的方括号改成圆括号，或将函数中的return替换为yield

```python
# 生成列表
ls = [x for x in range(10)]
ls  # [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]

# 生成器
gen = (x for x in range(10))
gen  # <generator object <genexpr> at 0x00000154F8776CF0>

# 通过next()函数获得generator的下一个返回值
next(gen)  # 0
next(gen)  # 1
next(gen)  # 2
next(gen)  # 3
next(gen)  # 4
next(gen)  # 5

# 将函数中的return替换为yield
def squares(n=10):
    print('Generating squares from 1 to {0}'.format(n ** 2))
    for i in range(1, n+1):
        yield i ** 2
gen = squares()
gen  # <generator object squares at 0x00000154F8776B88>

for x in gen:
    print(x, end=' ')  # 1 4 9 16 25 36 49 64 81 100 
```

在很多情况下，生成器表达式可以作为函数参数用于替代列表推导式

```python
sum(x ** 2 for x in range(100))  # 328350

dict((i, i ** 2) for i in range(4))  # {0: 0, 1: 1, 2: 4, 3: 9}
# 等价于
{i: i ** 2 for i in range(4)}  # {0: 0, 1: 1, 2: 4, 3: 9}
```

1. Python的Iterator对象表示的是一个数据流，可以把这个数据流看做是一个有序序列，但我们却不能提前知道序列的长度，所以Iterator的计算是惰性的，只有在需要返回下一个数据时它才会计算

2. Iterator保存的是算法，Iterator对象可以被`next()`函数调用并不断返回下一个数据，直到没有数据时抛出StopIteration错误

3. 所有的Iterable可迭代对象均可以通过内置函数`iter()`来转变为迭代器Iterator

   除了通过内置函数next调用可以判断是否为迭代器外，还可以通过collection中的Iterator类型判断。如：   `isinstance(’’, Iterator)`可以判断字符串类型是否迭代器。注意： list、dict、str虽然是Iterable，却不是Iterator

   ```python
   from collections.abc import Iterator
   ls = list(range(5))
   isinstance(ls, Iterator)  # False
   ls2 = iter(ls)  # 将列表转为迭代器
   isinstance(ls2, Iterator)  # True
   ```

4. 迭代器优点：节约内存（循环过程中，数据不用一次读入，在处理文件对象时特别有用，如一次读取一行，因为文件也是迭代器对象）、不依赖索引取值、实现惰性计算(需要时再取值计算)

5. 迭代器使用上存在限制：只能向前一个个地访问数据，已访问数据无法再次访问、遍历访问一次后再访问无数据。 如果需要解决这个问题，可以分别定义一个可迭代对象，每次访问前从可迭代对象重新生成迭代器对象

   ```python
   ls = list(range(5))
   iterator1 = iter(ls)
   iterator1  # <list_iterator at 0x204fc0dc4e0>
   ls2 = list(iterator1)
   ls2  # [0, 1, 2, 3, 4]
   ls3 = list(iterator1)
   ls3  # [] 再访问无数据
   
   ls = list(range(5))
   iterator1 = iter(ls)
   iterator1
   for i in iterator1:
       print(i, end=' ')  # 0 1 2 3 4
   for i in iterator1:
       print(i, end=' ')  # 无输出
   ```

### 函数是对象

例：数据清洗

方法一：使用内建的字符串方法，结合标准库中的正则表达式模块re

```python
states = ['   Alabama ', 'Georgial', 'Georgia', 'georgia', 'FlorIda', 
         'south  carolina##', 'West virginaia?']
import re
def clean_strings(strings):
    result = []
    for value in strings:
        value = value.strip()
        value = re.sub('[!#?]', '', value)
        value = value.title()
        result.append(value)
    return result
clean_strings(states)
```

方法二：将特定的列表操作应用到某个字符串的集合上

```python
states = ['   Alabama ', 'Georgial', 'Georgia', 'georgia', 'FlorIda', 
         'south  carolina##', 'West virginaia?']
import re
def remove_punctuation(value):
    return re.sub('[!#?]', '', value)
clean_ops = [str.strip, remove_punctuation, str.title]
def clean_strings(strings, ops):
    result = []
    for value in strings:
        for function in ops:
            value = function(value)
        result.append(value)
    return result
clean_strings(states, clean_ops)

###
Out:	['Alabama',
         'Georgial',
         'Georgia',
         'Georgia',
         'Florida',
         'South  Carolina',
         'West Virginaia']
```

将函数作为一个参数传给其他的函数，比如内建的map函数，可以将一个函数应用到一个序列上

```python
states = ['   Alabama ', 'Georgial', 'Georgia', 'georgia', 'FlorIda', 
         'south  carolina##', 'West virginaia?']
import re
def remove_punctuation(value):
    return re.sub('[!#?]', '', value)
for x in map(remove_punctuation, states):
    print(x)

###
Out:       Alabama 
        Georgial
        Georgia
        georgia
        FlorIda
        south  carolina
        West virginaia
```

### 匿名(lambda)函数

和def关键字申明的函数不同，匿名函数对象自身并没有一个显示的`__name__`属性，这是lambda函数被称为匿名函数的一个原因

```python
def apply_to_list(some_list, f):
    return [f(x) for x in some_list]
ints = [4, 0, 1, 5, 6]
apply_to_list(ints, lambda x: x ** 2)  # [16, 0, 1, 25, 36]

# 等价于
eq_anon = [x ** 2 for x in ints]
eq_anon  # [16, 0, 1, 25, 36]
```

将匿名函数传给列表的sort方法

```python
# 根据字符串中不同字母的数量对一个字符串集合进行升序排序
strings = ['foo', 'card', 'bar', 'aaaa', 'abab']
strings.sort(key=lambda x: len(set(list(x))))
strings  # ['aaaa', 'foo', 'abab', 'bar', 'card']
```

### 柯里化：部分参数应用

通过部分参数应用的方法从已有的函数中衍生出新的函数

```python
def add_numbers(x, y):
    return x + y
add_five = lambda y: add_numbers(5, y)  # 第二个参数对于函数add_numbers就是柯里化
add_five(6)  # 11
```

内建的functools模块可以使用partial函数简化这种处理

```python
from functools import partial
def add_numbers(x, y):
    return x + y
add_six = partial(add_numbers, 6)
add_six(7)  # 13
```

### 错误和异常处理

使用finally关键字，无论try代码块是否报错都要执行

```python
f = open(path, 'w')
try:
    write_to_file(f)
finally:
    f.close()  # 让f再程序结束后总是关闭
```

使用else来执行当try代码块成功执行时才会执行的代码

```python
f = open(path, 'w')
try:
    write_to_file(f)
except:
    print('Failed')
else:
    print('Succeeded')
finally:
    f.close()
```

## 文件处理

### 读取文件

从文件中取出的行都带有完整的行结束符（EOL)，因此常用以下代码得到没有EOL的行

```python
lines = [x.rstrip() for x in f]
```

默认以只读方式（'r'）打开

```python
path = 'F:\\Python Files\\Python for Data Analysis\\The Road Not Taken.txt'
f = open(path)
lines = [x.rstrip() for x in f]
f.close()  # 关闭文件
lines
```

用with语句可以在退出代码块时，自动关闭文件

```python
path = 'F:\\Python Files\\Python for Data Analysis\\The Road Not Taken.txt'
with open(path) as f:
    lines = [x.rstrip() for x in f]
lines
```

![](images/Table 3-3. Python file modes.jpg)

### 写入文件

```python
with open('F:\\Python Files\\Python for Data Analysis\\copy.txt', 'w') as handle:
    handle.writelines(x for x in open(path) if len(x) > 1)
    
with open('F:\\Python Files\\Python for Data Analysis\\copy.txt') as f:
    lines = f.readlines()
lines
```

![](images/Table 3-4(1). Important Python file methods or attributes.jpg)

![](images/Table 3-4(2). Important Python file methods or attributes.jpg)



# 读取数据

从excel读取日期格式的数据

```python
from xlrd import xldate_as_tuple
time_ls2 = [xldate_as_tuple(x,0) for x in time_ls if x]
```



# Numpy

```python
import numpy as np
```

## 原数组视图

**数组索引和转置均为原数据的视图。这意味着数据并不是被复制了，任何对于视图的修改都会反映到原数组上**

如果想要一份拷贝而不是一份视图，需用`.copy()`来复制

## 初始设置

```python
numpy.set_printoptions(suppress=True)
# 设置suppress是不使用科学计数
# 括号里也可以是precision=小数的位数
```

## ndarray

ndarray（N-维数组对象）是一个通用的同构数据多维容器，因此其中的所有元素必须是相同类型的。每个数组都有一个shape（一个表示各维度大小的元组）和一个dtype（一个用于说明数组数据类型的对象）

```python
import numpy as np
data = np.random.randn(2, 3)  # 产生正态分布随机数组
data
# array([[-0.50986823,  1.75861631,  0.51254599],
#        [ 0.43797552, -0.13857349,  1.73018754]])

data * 10  # 数学运算
# array([[-5.0986823 , 17.58616314,  5.12545991],
#        [ 4.37975518, -1.38573489, 17.30187542]])

data.shape  # (2, 3)
data.dtype  # dtype('float64')
```

## 数组运算

大小相等的数组之间的算数运算

```python
arr = np.array([[1, 2, 3, 4], [5, 6, 7, 8]])
arr
# array([[1, 2, 3, 4],
#        [5, 6, 7, 8]])

arr * arr
# array([[ 1,  4,  9, 16],
#        [25, 36, 49, 64]])

arr - arr
# array([[0, 0, 0, 0],
#        [0, 0, 0, 0]])

1 / arr
# array([[1.        , 0.5       , 0.33333333, 0.25      ],
#        [0.2       , 0.16666667, 0.14285714, 0.125     ]])

arr ** 0.5
# array([[1.        , 1.41421356, 1.73205081, 2.        ],
#        [2.23606798, 2.44948974, 2.64575131, 2.82842712]])

arr2 = np.array([[0, 4, 1, 4], [7, 2, 12, 8]])
arr2
# array([[ 0,  4,  1,  4],
#        [ 7,  2, 12,  8]])

# 大小相同的数组之间的比较会生成布尔值数组
arr2 > arr
# array([[False,  True, False, False],
#        [ True, False,  True, False]])
```

不同大小的数组之间的运算叫做广播（broadcasting）

## 线性代数

Numpy的线性代数中`*`是矩阵的**逐元素**乘积

### 点乘

矩阵的**点乘积**用 `np.dot(x, y)`、`x.dot(y)`、`x @ y`

```python
x = np.array([[1, 2, 3], [4, 5, 6]])
y = np.array([[6, 23], [-1, 7], [8, 9]])

np.dot(x, y)
x.dot(y)
x @ y
# array([[ 28,  64],
#        [ 67, 181]])
```

### numpy.linalg

`numpy.linalg`是Linear Algebra的缩写，拥有一个矩阵分解的标准函数集

```python
from numpy.linalg import inv
x = np.random.randn(5, 5)
mat = x.T.dot(x)
inv(mat)
# array([[ 12.84432811,  -3.30542641, -10.9342774 , -13.05961095,
#           2.65029311],
#        [ -3.30542641,   1.0410862 ,   2.33502273,   3.15260509,
#          -0.58283517],
#        [-10.9342774 ,   2.33502273,  11.99335862,  12.72834185,
#          -2.99603428],
#        [-13.05961095,   3.15260509,  12.72834185,  14.70072618,
#          -3.10362607],
#        [  2.65029311,  -0.58283517,  -2.99603428,  -3.10362607,
#           1.00806334]])
mat.dot(inv(mat))
# array([[ 1.00000000e+00, -1.22124533e-15,  1.19904087e-14,
#         -1.11022302e-15, -2.22044605e-16],
#        [-8.88178420e-16,  1.00000000e+00,  6.21724894e-15,
#         -8.88178420e-16,  4.44089210e-16],
#        [-6.21724894e-15,  4.44089210e-16,  1.00000000e+00,
#          6.21724894e-15,  2.22044605e-16],
#        [-2.22044605e-16, -9.99200722e-16,  8.65973959e-15,
#          1.00000000e+00, -2.10942375e-15],
#        [ 0.00000000e+00, -4.44089210e-16,  0.00000000e+00,
#         -1.77635684e-15,  1.00000000e+00]])

np.set_printoptions(suppress=True)  # 不使用科学计数法
mat.dot(inv(mat))
# array([[ 1., -0.,  0., -0., -0.],
#        [-0.,  1.,  0., -0.,  0.],
#        [-0.,  0.,  1.,  0.,  0.],
#        [-0., -0.,  0.,  1., -0.],
#        [ 0., -0.,  0., -0.,  1.]])
```

## 通用函数(ufunc)

对ndarray中的数据执行**逐元素**操纵的函数，可视为简单函数的矢量化包装

```python
# 一元ufunc，接受一个参数
arr = np.arange(5)
np.sqrt(arr)  # array([0.        , 1.        , 1.41421356, 1.73205081, 2.        ])
np.exp(arr)  # array([ 1.        ,  2.71828183,  7.3890561 , 20.08553692, 54.59815003])

# 二元ufunc，接受两个参数
x = np.random.randn(5)
x  # array([ 0.85905731,  0.13474517,  1.15065896, -0.01374638,  0.73128319])
y = np.random.randn(5)
y  # array([ 0.41610658,  1.92600783,  1.19052162, -0.05339175,  1.15140546])
np.maximum(x, y)  # array([ 0.85905731,  1.92600783,  1.19052162, -0.01374638,  1.15140546])

# 返回多个数组
arr = np.random.randn(5) * 5
arr  # array([-3.91217682,  4.15375325, -3.73826065,  7.59348123,  5.90557995])
remainder, whole_part = np.modf(arr)
remainder  # array([-0.91217682,  0.15375325, -0.73826065,  0.59348123,  0.90557995])
whole_part  # array([-3.,  4., -3.,  7.,  5.])
```

![](images/Table 4-3. Unary ufuncs.png)

![](images/Table 4-4. Binary universal functions1.png)

![](images/Table 4-4. Binary universal functions2.png)

## 常用函数

### 数组生成函数

![](images/Table 4-1. Array creation functions.png)

#### array()

生成数组

```python
data1 = [6, 7.5, 8, 0, 1]
arr1 = np.array(data1)
arr1  # array([6. , 7.5, 8. , 0. , 1. ])

# 嵌套序列
data2 = [[1, 2, 3, 4], [5, 6, 7, 8]]
arr2 = np.array(data2)
arr2
# array([[1, 2, 3, 4],
#        [5, 6, 7, 8]])

arr2.dtype  # dtype('int32')
```

#### zeros() / ones()

分别可以创建指定长度或形状的全为0或全为1的数组

```python
np.zeros(10)  # array([0., 0., 0., 0., 0., 0., 0., 0., 0., 0.])

np.ones((3,4))  # 用元组创建维度
# array([[1., 1., 1., 1.],
#        [1., 1., 1., 1.],
#        [1., 1., 1., 1.]])
```

#### zeros_like() / ones_like()

以另一个数组为参数，根据其形状和dtype创建全为0或全为1的数组

```python
data2 = [[1, 2, 3, 4], [5, 6, 7, 8]]
arr2 = np.array(data2)
np.zeros_like(arr2)
# array([[0, 0, 0, 0],
#        [0, 0, 0, 0]])
np.ones_like(arr2)
# array([[1, 1, 1, 1],
#        [1, 1, 1, 1]])
```

#### empty()

创建新数组，只分配内存空间但不填充任何值

用np.empty()来生成一个全0数组并不安全，有时候它可能会返回一些未初始化的垃圾数值

```python
np.empty((2, 3, 3))
# array([[[6.23042070e-307, 1.89146896e-307, 1.37961302e-306],
#         [1.05699242e-307, 8.01097889e-307, 1.78020169e-306],
#         [7.56601165e-307, 1.02359984e-306, 1.11259940e-306]],
# 
#        [[1.11261774e-306, 1.78019625e-306, 1.11261774e-306],
#         [1.78019625e-306, 1.11261774e-306, 1.78019625e-306],
#         [2.22520559e-306, 8.34451928e-308, 2.22507386e-306]]])
```

#### arange()

Python内置函数range的数组版

```python
np.arange(10)  # array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
```

#### eye() / identity()

创建一个N×N的单位矩阵

```python
np.eye(3)
# array([[1., 0., 0.],
#        [0., 1., 0.],
#        [0., 0., 1.]])

np.identity(3)
# array([[1., 0., 0.],
#        [0., 1., 0.],
#        [0., 0., 1.]])
```

#### astype()

将一个数组从一个dtype转换成另一个dtype

```python
# 将整数转换成浮点数
arr = np.array([1, 2, 3, 4])
arr.dtype  # dtype('int32')
float_arr = arr.astype(np.float64)
float_arr.dtype  # dtype('float64')

# 将浮点数转换成整数，小数部分会被截取
arr = np.array([3.7, -1.2, -2.6, 0.5, 12.9, 10.1])
arr  # array([ 3.7, -1.2, -2.6,  0.5, 12.9, 10.1])
arr.astype(np.int32)  # array([ 3, -1, -2,  0, 12, 10])

# 另一种形式
int_arr = np.arange(10)
calibers = np.array([.22, .270, .357, .380, .44, .50], dtype=np.float64)
int_arr.astype(calibers.dtype)

# 将全为数字的字符串数组转为数值形式
numeric_strings = np.array(['1.25', '-9.6', '42'], dtype=np.string_)
numeric_strings.astype(float)  # array([ 1.25, -9.6 , 42.  ])
```

注意：使用numpy.string_类型时要小心，因为numpy的字符串数据是大小固定的，发生截取时不会发出警告

astype()总会创建一个新的数组（一个数据的备份），即使新的dtype与旧的dtype相同

```python
arr = np.array([1, 2, 3, 4])
arr.dtype  # dtype('int32')
arr.astype(np.float64)  # array([1., 2., 3., 4.])
arr.dtype  # dtype('int32')
```

#### reshape()

生成指定维度的数组。**若要创建高维数组，需要为shape传递一个元组**

```python
arr5 = np.arange(36).reshape(2, 18)
arr5
# array([[ 0,  1,  2,  3,  4,  5,  6,  7,  8,  9, 10, 11, 12, 13, 14, 15, 16, 17],
#       [18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30, 31, 32, 33, 34, 35]])
arr6 = arr5.reshape(6, 6)
arr6
# array([[ 0,  1,  2,  3,  4,  5],
#       [ 6,  7,  8,  9, 10, 11],
#       [12, 13, 14, 15, 16, 17],
#       [18, 19, 20, 21, 22, 23],
#       [24, 25, 26, 27, 28, 29],
#       [30, 31, 32, 33, 34, 35]])
```

#### meshgrid()

根据传入的两个一维数组参数生成两个数组元素的列表

如果第一个参数是xarray，维度是xdim；第二个参数是yarray，维度是ydim，那么生成的第一个二维数组是以xarray为行，共ydim行的向量；而第二个二维数组是以yarray的转置为列，共xdim列的向量

```python
x_points = np.arange(1, 4)
y_points = np.arange(5, 10)
xs, ys = np.meshgrid(x_points, y_points)
xs
# array([[1, 2, 3],
#        [1, 2, 3],
#        [1, 2, 3],
#        [1, 2, 3],
#        [1, 2, 3]])
ys
# array([[5, 5, 5],
#        [6, 6, 6],
#        [7, 7, 7],
#        [8, 8, 8],
#        [9, 9, 9]])
```

### 条件逻辑

#### where()

将条件逻辑表述为数组运算`np.where(cond, xarr, yarr)`

三元表达式`x if condition else y`的矢量化版本

传递给`np.where`的数组既可以是同等大小的数组，也可以是标量

列表推导式存在的问题：第一，对大数组的处理速度不是很快；第二，无法用于多维数组

```python
xarr = np.array([1.1, 1.2, 1.3, 1.4, 1.5])
yarr = np.array([2.1, 2.2, 2.3, 2.4, 2.5])
cond = np.array([True, False, True, True, False])

# 当cond中的值为True时，选取xarr的值，否则从yarr中选取
# 列表推导式
result = [(x if c else y) 
          for x, y, c in zip(xarr, yarr, cond)]
result  # [1.1, 2.2, 1.3, 1.4, 2.5]

# 使用np.where()更简洁
result = np.where(cond, xarr, yarr)
result  # array([1.1, 2.2, 1.3, 1.4, 2.5])
```

`np.where()`的第二个和第三个参数可不必是数组

```python
arr = np.random.randn(4, 4)
arr
# array([[-0.42670945, -0.05014591, -0.22880216,  1.58326101],
#        [ 0.02904492,  0.86621441,  0.29244502, -1.5281871 ],
#        [-0.88263296, -0.66185756,  0.59581819, -0.89392237],
#        [ 0.42971019, -0.43300162, -0.99941476, -1.11071411]])

# 将所有正值替换为2，将所有负值替换为-2
np.where(arr > 0, 2, -2)
# array([[-2, -2, -2,  2],
#        [ 2,  2,  2, -2],
#        [-2, -2,  2, -2],
#        [ 2, -2, -2, -2]])
```

使用`np.where()`可将标量和数组结合起来

```python
# 将所有正值替换为2，其余保持不变
np.where(arr > 0, 2, arr)
# array([[-0.42670945, -0.05014591, -0.22880216,  2.        ],
#        [ 2.        ,  2.        ,  2.        , -1.5281871 ],
#        [-0.88263296, -0.66185756,  2.        , -0.89392237],
#        [ 2.        , -0.43300162, -0.99941476, -1.11071411]])
```

### 数学和统计方法

![](images/Table 4-5. Basic array statistical methods.png)

#### mean() / sum()

```python
arr = np.arange(1, 21).reshape((4, 5))
arr
# array([[ 1,  2,  3,  4,  5],
#        [ 6,  7,  8,  9, 10],
#        [11, 12, 13, 14, 15],
#        [16, 17, 18, 19, 20]])

# arr.mean(1)计算行的平均值，arr.mean(0)计算列的平均值
arr.mean()  # 10.5
arr.sum()  # 210
arr.mean(axis=1)  # array([ 3.,  8., 13., 18.])
arr.mean(axis=0)  # array([ 8.5,  9.5, 10.5, 11.5, 12.5])
```

#### cumsum() / cumprod()

累计函数如`cumsum`、`cumprod`等返回相同长度的数组

```python
arr = np.arange(9).reshape((3,3))
arr
# array([[0, 1, 2],
#        [3, 4, 5],
#        [6, 7, 8]])

arr.cumsum(axis=0)  # 累计和
# array([[ 0,  1,  2],
#        [ 3,  5,  7],
#        [ 9, 12, 15]], dtype=int32)

arr.cumprod(axis=1)  # 累计积
# array([[  0,   0,   0],
#        [  3,  12,  60],
#        [  6,  42, 336]], dtype=int32)
```

#### argmax()

返回数组中出现参数值的第一个位置

效率不高，总是完整地扫描整个数组

```python
rng = np.random.RandomState(1234)
x = rng.randint(0, 10, size=100)
(x == 5).argmax()  # 2
```

### 用于布尔型数组的方法

布尔值会被强制为1（True）和0（False）

sum常用于计算布尔型数组中的True的个数

```python
arr = np.random.randn(100)
(arr > 0).sum()  # 41 正值的个数
```

#### any() / all()

`any`检查数组中是否至少有一个True，而`all`检查是否所有值都是True

```python
bools = np.array([False, False, True, False])
bools.any()  # True
bools.all()  # False
```

这两种方法也能用于非布尔型数组，所有非0元素将会被当作True

### 排序

#### sort()

对原数组进行排序

```python
# 一维数组
arr = np.random.randn(6)
arr  # array([ 1.28210295, -1.7790956 , -0.16812119,  0.85530003,  0.42693403,  0.43852188])

arr.sort()
arr  # array([-1.7790956 , -0.16812119,  0.42693403,  0.43852188,  0.85530003,  1.28210295])


# 多维数组可在任何一个轴向上进行排序
arr = np.random.randn(5, 3)
arr
# array([[ 0.14891767, -0.67526345, -0.80804495],
#        [ 0.20008634, -0.91812032,  0.120672  ],
#        [ 0.13938935,  0.3474118 , -0.10446888],
#        [-0.15573932,  0.94353267, -1.58981545],
#        [-0.63501188, -0.75752942, -1.86098617]])

arr.sort(1)
arr
# array([[-0.80804495, -0.67526345,  0.14891767],
#        [-0.91812032,  0.120672  ,  0.20008634],
#        [-0.10446888,  0.13938935,  0.3474118 ],
#        [-1.58981545, -0.15573932,  0.94353267],
#        [-1.86098617, -0.75752942, -0.63501188]])
```

计算数组分位数最简单的办法就是对其进行排序，然后选取特定位置的值

```python
large_arr = np.random.randn(1000)
large_arr.sort()
large_arr[int(0.05 * len(large_arr))]  # -1.7488781198683174，5%分位数对应的值
```

### 唯一值与其他集合逻辑

#### unique()

返回数组中唯一值排序后形成的数组

```python
names = np.array(['Bob', 'Joe', 'Will', 'Bob', 'Will', 'Joe', 'Joe'])
np.unique(names)  # array(['Bob', 'Joe', 'Will'], dtype='<U4')

ints = np.array([3, 3, 3, 2, 2, 1, 1, 4, 4])
np.unique(ints)  # array([1, 2, 3, 4])
```

纯Python实现

```python
ints = np.array([3, 3, 3, 2, 2, 1, 1, 4, 4])
sorted(set(ints))  # [1, 2, 3, 4]
```

#### in1d()

检查一个数组中的值是否在另外一个数组中，并返回一个布尔值数组

```python
values = np.array([6, 0, 0, 3, 2, 5, 6])
np.in1d(values, [2, 3, 6])  # array([ True, False, False,  True,  True, False,  True])
```

![](images/Table 4-6. Array set operations.png)生成随机数

## 生成随机数

### 正态分布

#### random.randn()

从**标准正态分布**中随机取样

> random.randn(d0, d1, …, dn)

参数：d0, d1, …, dn 为正整数，表示维度

返回值：ndarray或float

```python
np.random.randn(2, 4)
# array([[-1.78798942, -0.94030638,  1.30638268,  0.12137303],
#        [-0.23808032,  0.54053839, -0.73770437, -0.89760192]])
```

从**任意正态分布**中随机取样

>sigma * np.random.randn(d0, d1, …, dn) + mu

```python
2.5 * np.random.randn(2, 4) + 3  # N~(3,2.5^2)
# array([[ 5.5991203 ,  2.27532855,  4.86545007,  6.56995944],
#        [ 8.22961912, -0.20157633,  3.9318603 ,  0.69143598]])
```

#### random.normal()

从**任意正态分布**中随机取样

> random.normal(mu, sigma, size)

```python
np.random.normal(2, 3, 4)  # N~(2,3)
# array([-3.37671999,  0.23837364,  2.40469517,  5.8634633 ])

np.random.normal(2, 3, (2, 4))  # N~(2,3)
# array([[ 1.81264882,  5.72965493,  1.71250466, -2.79717846],
#        [ 4.52214488, 12.37280549, -0.50379513,  6.5952671 ]])
```

### 均匀分布

#### random.rand()

从**标准均匀分布U[0, 1)**中随机抽取样本

> random.rand(d0, d1, …, dn)

```python
np.random.rand(2, 3)
# array([[0.71133727, 0.16153688, 0.97007667],
#        [0.15971494, 0.59958725, 0.95484191]])
```

#### random.uniform()

从均匀分布U[a,b)中随机抽取样本

> random.uniform(a, b, size)

```python
np.random.uniform(2, 4)
# 3.952481875440001

np.random.uniform(2, 4, (2, 3))
# array([[2.87279853, 3.62451635, 3.18720247],
#        [3.68450981, 2.04271902, 2.02164645]])
```

## 生成伪随机数

伪随机数：由具有确定性行为的算法根据随机数生成器中的随机数种子生成

```python
samples = np.random.normal(size=(4, 4))
samples
# array([[ 1.42042501,  0.8069417 ,  0.35409867,  1.06071087],
#        [-2.09642534,  0.03279896, -1.27625703, -0.13723072],
#        [ 0.73452639, -0.51983654,  0.51243829, -0.88504057],
#        [-0.40629556, -0.39436763, -2.00054699,  0.30154028]])
```

### random.seed()

更改随机数种子

```python
np.random.seed(1234)
```

### random.RandomState()

`numpy.random`中的数据生成函数公用一个全局的随机数种子。为了避免全局状态，可以使用`numpy.random.RandomState`生成一个随机数生成器，使数据独立于其他的随机数状态

```python
rng = np.random.RandomState(1234)
rng.randn(5)  # array([ 0.47143516, -1.19097569,  1.43270697, -0.3126519 , -0.72058873])
```

![](images/Table 4-8. Partial list of numpy.random functions.png)

### 示例：随机漫步

1. Python内建random模块

   内建模块中`random.randint(0, 1)`产生的随机数为0和1——**包含边界**

```python
position = 0
walk = [position]
steps = 1000
for i in range(steps):
    step = 1 if random.randint(0, 1) else -1  # 步进1和-1发生的概率相等
    position += step
    walk.append(position)
plt.plot(walk[:100])
```

![](images/random_walk_1.png)

2. 使用`np.random.randint()`

`np.random.randint(0, 2)`产生随机数0和1——**左闭右开**

```python
nsteps = 1000
draws = np.random.randint(0, 2, size=nsteps)
steps = np.where(draws > 0, 1, -1)
walk = np.cumsum(steps)
plt.plot(walk)

walk.min()  # 最小值
walk.max()  # 最大值
np.abs(walk)>=10  # 返回一个布尔值数组，用于表明漫步是否连续在同一方向走了十步
(np.abs(walk)>=10).argmax()  # 返回布尔值数组中最大值的第一个位置
```

![](images/random_walk_2.png)

### 示例：一次模拟多次随机漫步

传入元组

```python
nwalks = 5000
nsteps = 1000
draws = np.random.randint(0, 2, size=(nwalks, nsteps))
steps = np.where(draws > 0, 1, -1)
walks = steps.cumsum(1)
walks
# array([[  1,   2,   1, ...,  28,  29,  30],
#        [  1,   0,  -1, ..., -40, -39, -38],
#        [  1,   0,   1, ...,  46,  45,  46],
#        ...,
#        [  1,   2,   3, ...,  12,  13,  14],
#        [ -1,  -2,  -1, ...,  10,   9,   8],
#        [  1,   2,   3, ..., -24, -23, -24]], dtype=int32)

hits30 = (np.abs(walks) >= 30).any(1)  # 并不是所有的5000个都达到了30，先用any()找出绝对步数大于30所在的行
hits30  # array([ True,  True,  True, ...,  True, False, False])
hits30.sum()  # 3303 达到30或-30的数字
crossing_times = (np.abs(walks[hits30]) >= 30).argmax(1)
crossing_times.mean()
```

## 索引和切片

数组切片与列表切片最重要的区别在于，数组切片是原始数组上的视图

这意味着数据不会被复制，视图上的任何修改都会直接反应到原数组上

### 一维数组

与列表切片类似

```python
arr = np.arange(10)
arr  # array([0, 1, 2, 3, 4, 5, 6, 7, 8, 9])
arr[5]  # 5
```

将一个标量赋值给一个切片时，该值会自动传播（“广播”）到整个选区

```python
arr[5:8] = 12
arr  # array([ 0,  1,  2,  3,  4, 12, 12, 12,  8,  9])
```

**数组切片是原始数组上的视图，任何对于视图的修改都会反映到原数组上**

```python
arr_slice = arr[5:8]
arr_slice  # array([12, 12, 12])
arr_slice[1] = 1234
arr  # array([   0,    1,    2,    3,    4,   12, 1234,   12,    8,    9])
	 # 原始数组发生改变

# 切片[:]给切片中的所有值赋值
arr_slice[:] = 64
arr  # array([ 0,  1,  2,  3,  4, 64, 64, 64,  8,  9])
```

如果想要一份**数组切片的拷贝**而不是一份视图，就必须显式地复制这个数组

```python
slice_arr2 = arr[1: 3].copy()
slice_arr2[1] = 555
arr  # array([   0,    1,    2,    3,    4,   12, 1234,   12,    8,    9])
```

### 二维数组

创建2×2×3数组

```python
arr3d = np.array([[[1, 2, 3], [4, 5, 6]], [[7, 8, 9], [10, 11, 12]]])
arr3d
# array([[[ 1,  2,  3],
#         [ 4,  5,  6]],
#        [[ 7,  8,  9],
#         [10, 11, 12]]])
```

#### 索引

返回的数组都是视图

```python
arr3d[0]
# array([[1, 2, 3],
#        [4, 5, 6]])

arr3d[1, 0]
# array([7, 8, 9])

# 等价于（分两步索引）
x = arr3d[1]
x[0]
# array([7, 8, 9])

#赋值
old_values = arr3d[0].copy()
arr3d[0] = 42
arr3d
# array([[[42, 42, 42],
#         [42, 42, 42]],
#        [[ 7,  8,  9],
#         [10, 11, 12]]])

arr3d[0] = old_values
arr3d
# array([[[ 1,  2,  3],
#         [ 4,  5,  6]],
#        [[ 7,  8,  9],
#         [10, 11, 12]]])
```

#### 切片索引

```python
arr2d = np.array([[1, 2, 3], [4, 5, 6], [7, 8, 9]])
arr2d
# array([[1, 2, 3],
#        [4, 5, 6],
#        [7, 8, 9]])

arr2d[:2]  # 索引前两行
# array([[1, 2, 3],
#        [4, 5, 6]])

arr2d[1, :2]  # 索引第二行的前两列
# array([4, 5])

arr2d[:2, 1:]  # 索引前两行第2个元素后的所有元素
# array([[2, 3],
#        [5, 6]])

arr2d[:, :1]  # 索引第一列
# array([[1],
#        [4],
#        [7]])
```

赋值

```python
arr2d[:2, 1:] = 0
arr2d
# array([[1, 0, 0],
#        [4, 0, 0],
#        [7, 8, 9]])
```

### 布尔值索引

通过布尔型索引选取数组中的数据，将创建数据的副本，即使返回一模一样的数组也是如此

布尔型数组的长度必须和数组**轴**索引长度一致。当布尔值数组的长度不正确时，布尔值选择数据的方法并不会报错，因此在使用该特性的时候要小心。

+ `~`操作符表示反转，类似于`!=`，但是在条件表达式前使用。

```python
names = np.array(['Bob', 'Joe', 'will', 'Bob', 'will', 'Joe', 'Joe'])
names
# array(['Bob', 'Joe', 'will', 'Bob', 'will', 'Joe', 'Joe'], dtype='<U4')

data = np.random.randn(7, 4)
data
# array([[ 2.19844335, -2.60641597,  0.64599781, -0.48395999],
#        [ 0.1087546 , -0.23686701,  0.8829822 , -1.61719679],
#        [ 0.83800837,  0.77291693,  0.77200033,  1.09832853],
#        [-2.3724484 , -1.80117463,  0.75493585,  1.28957244],
#        [ 1.04677125,  0.900982  ,  0.76180121,  0.50018476],
#        [ 0.84903786,  0.86724174,  2.46032234,  0.13894068],
#        [-0.07049406,  1.00358251, -0.18983426,  0.44337335]])


# 注意：布尔型数组的长度必须跟被索引的轴长度一致
con = names == 'Bob'
con  # array([ True, False, False,  True, False, False, False])
con.dtype  # dtype('bool')

data[con]  # 选出对应于名字'Bob'的所有行
# array([[ 2.19844335, -2.60641597,  0.64599781, -0.48395999],
#        [-2.3724484 , -1.80117463,  0.75493585,  1.28957244]])

data[con, 2:]  # 选出对应于名字'Bob'的所有行,并索引列
# array([[ 0.64599781, -0.48395999],
#        [ 0.75493585,  1.28957244]])

data[~con]  # 选出除'Bob'以外的其他值  ~操作符表示反转，类似于!=
# array([[ 0.1087546 , -0.23686701,  0.8829822 , -1.61719679],
#        [ 0.83800837,  0.77291693,  0.77200033,  1.09832853],
#        [ 1.04677125,  0.900982  ,  0.76180121,  0.50018476],
#        [ 0.84903786,  0.86724174,  2.46032234,  0.13894068],
#        [-0.07049406,  1.00358251, -0.18983426,  0.44337335]])
```

多个布尔条件的组合，使用&（和）、|（或）等布尔算术运算符。Python的关键字and和or对布尔数值组没用。

```python
mask = (names == 'Bob') | (names == 'Will')
mask  # rray([ True, False, False,  True, False, False, False])
data[mask]
# array([[ 2.19844335, -2.60641597,  0.64599781, -0.48395999],
#        [-2.3724484 , -1.80117463,  0.75493585,  1.28957244]])
```

通过布尔型数组设置值

```python
data[data < 0] = 0
data
# array([[2.19844335, 0.        , 0.64599781, 0.        ],
#        [0.1087546 , 0.        , 0.8829822 , 0.        ],
#        [0.83800837, 0.77291693, 0.77200033, 1.09832853],
#        [0.        , 0.        , 0.75493585, 1.28957244],
#        [1.04677125, 0.900982  , 0.76180121, 0.50018476],
#        [0.84903786, 0.86724174, 2.46032234, 0.13894068],
#        [0.        , 1.00358251, 0.        , 0.44337335]])

data[names != 'Bob'] = 7
data
# array([[2.19844335, 0.        , 0.64599781, 0.        ],
#        [7.        , 7.        , 7.        , 7.        ],
#        [7.        , 7.        , 7.        , 7.        ],
#        [0.        , 0.        , 0.75493585, 1.28957244],
#        [7.        , 7.        , 7.        , 7.        ],
#        [7.        , 7.        , 7.        , 7.        ],
#        [7.        , 7.        , 7.        , 7.        ]])
```

注意：

+ 布尔型数组的长度必须跟被索引的轴长度一致
+ Python关键字and和or在布尔型数组中无效，要使用&和|

### 神奇索引

利用整数数组进行索引

如果不考虑数组的维数，神奇索引的结果总是一维的

**神奇索引与切片不同，它总是将数据复制到一个新的数组中**

1. 通过传递一个包含指明所需顺序的列表或数组来选出一个符合特定顺序的子集

```python
# 创建一个8*4的数组
arr = np.empty((8, 4))
for i in range(8):
    arr[i] = i
arr
# array([[0., 0., 0., 0.],
#        [1., 1., 1., 1.],
#        [2., 2., 2., 2.],
#        [3., 3., 3., 3.],
#        [4., 4., 4., 4.],
#        [5., 5., 5., 5.],
#        [6., 6., 6., 6.],
#        [7., 7., 7., 7.]])

# 传入一个用于指定顺序的整数列表或ndarray，以特定顺序选取子集
arr[[4, 3, 0, 6]]
# array([[4., 4., 4., 4.],
#        [3., 3., 3., 3.],
#        [0., 0., 0., 0.],
#        [6., 6., 6., 6.]])

# 使用负索引，从末尾开始选取
arr[[-3, -5, -7]]
# array([[5., 5., 5., 5.],
#        [3., 3., 3., 3.],
#        [1., 1., 1., 1.]])
```

2. 传递多个索引数组时，会根据每个索引元组对应的元素选出一个一维数组

```python
# 一次传入多个索引数组
arr = np.arange(32).reshape((8, 4))
arr
# array([[ 0,  1,  2,  3],
#        [ 4,  5,  6,  7],
#        [ 8,  9, 10, 11],
#        [12, 13, 14, 15],
#        [16, 17, 18, 19],
#        [20, 21, 22, 23],
#        [24, 25, 26, 27],
#        [28, 29, 30, 31]])
arr[[1, 5, 7, 2],[0, 3, 1, 2]]
# array([ 4, 23, 29, 10])
```

3. 除神奇索引外，上例也可通过选择矩阵中行列的子集所形成的矩形区域

```python
arr[[1, 5, 7, 2]][:, [0, 3, 1, 2]]
# array([[ 4,  7,  5,  6],
#        [20, 23, 21, 22],
#        [28, 31, 29, 30],
#        [ 8, 11,  9, 10]])
```

## 数组转置和轴转换

+ **返回原数组的视图**

数组拥有transpose方法，也有特殊的T属性

```python
arr = np.arange(15).reshape((3, 5))
arr
# array([[ 0,  1,  2,  3,  4],
#        [ 5,  6,  7,  8,  9],
#        [10, 11, 12, 13, 14]])

# 简单的转置可以使用.T
arr.T
# array([[ 0,  5, 10],
#        [ 1,  6, 11],
#        [ 2,  7, 12],
#        [ 3,  8, 13],
#        [ 4,  9, 14]])
```

利用`np.dot()`计算矩阵内积

```python
arr = np.random.randn(6, 3)
arr
np.dot(arr.T, arr)
# array([[ 6.56681811,  4.19306833, -1.52519049],
#        [ 4.19306833,  4.91213373, -2.68154749],
#        [-1.52519049, -2.68154749,  2.5735825 ]])
```

**高维数组的转置**

+ 转置返回底层数据的视图而不需要复制任何内容

transpose方法可以接受包含轴编号的元组，用于置换轴

```python
arr = np.arange(16).reshape((2, 2, 4))
arr
# array([[[ 0,  1,  2,  3],
#         [ 4,  5,  6,  7]],
#        [[ 8,  9, 10, 11],
#         [12, 13, 14, 15]]])

# 原矩阵是一个三维矩阵，索引顺序是x, y, z，角标分别是0、1、2，经（1, 0, 2）调整后就成了y, x, z
arr.transpose((1, 0, 2))
# array([[[ 0,  1,  2,  3],
#         [ 8,  9, 10, 11]],
#        [[ 4,  5,  6,  7],
#         [12, 13, 14, 15]]])
```

`swapaxes()`需要接受一对轴编号，返回原数据的视图（不进行任何复制操作）

```python
arr.swapaxes(1,2)  # 将第二维度和第三维度交换
# array([[[ 0,  4],
#         [ 1,  5],
#         [ 2,  6],
#         [ 3,  7]],
#        [[ 8, 12],
#         [ 9, 13],
#         [10, 14],
#         [11, 15]]])
```

# Pandas

```python
import pandas as pd
```

## 数据结构

### Series()

`Series`是一种**一维**的数据型对象

```python
import pandas as pd
obj = pd.Series([4, 7, -5, 5])
obj
# 0    4
# 1    7
# 2   -5
# 3    5
obj.values  # array([ 4,  7, -5,  5], dtype=int64)
obj.index  # RangeIndex(start=0, stop=4, step=1)，不包含stop的4

# 通过赋值改变索引
obj = pd.Series([4, 7, -5, 5])
obj
# 0    4
# 1    7
# 2   -5
# 3    5
# dtype: int64
obj.index = ['Bob', 'Steve', 'Jeff', 'Ryan']
obj
# Bob      4
# Steve    7
# Jeff    -5
# Ryan     5
# dtype: int64
```

索引序列，用标签标识每个数据点。

```python
obj2 = pd.Series([4, 7, -4, 3], index=['d', 'b', 'a', 'c'])
obj2
# d    4
# b    7
# a   -4
# c    3

# 使用标签进行索引
obj2['a']  # -4
obj2['d'] = 6
obj2[['c', 'a', 'd']]
# c    3
# a   -4
# d    6

obj2[obj2 > 0]
# d    6
# b    7
# c    3
```

可以认为Series是一个长度固定且有序的字典，将索引值和数据值按位置配置。

```python
# 类似字典的索引方式
'b' in obj2  # True
'e' in obj2  # False
```

使用字典生成一个Series

```python
sdata = {'Ohio': 35000, 'Texas': 71000, 'Oregon': 16000, 'Utah': 5000}
obj3 = pd.Series(sdata)
obj3
# Ohio      35000
# Texas     71000
# Oregon    16000
# Utah       5000
# dtype: int64
```

按照构造的顺序排序

```python
states = ['California', 'Ohio', 'Oregon', 'Texas']
obj4 = pd.Series(sdata, states)
obj4
# California        NaN
# Ohio          35000.0
# Oregon        16000.0
# Texas         71000.0
# dtype: float64
```

检查缺失数据

```python
pd.isnull(obj4)
# California     True
# Ohio          False
# Oregon        False
# Texas         False
# dtype: bool

obj4.isnull()
# California     True
# Ohio          False
# Oregon        False
# Texas         False
# dtype: bool

pd.notnull(obj4)
# California    False
# Ohio           True
# Oregon         True
# Texas          True
# dtype: bool
```

name属性

```python
obj4.name = 'population'
obj4.index.name = 'state'
obj4
# state
# California        NaN
# Ohio          35000.0
# Oregon        16000.0
# Texas         71000.0
# Name: population, dtype: float64
```

### DataFrame

矩阵数据表。可利用等长度的字典来创建。

```python
data = {'state': ['Ohio', 'Ohio', 'Ohio', 'Nevada', 'Nevada', 'Nevada'],
        'year': [2000, 2001, 2002, 2001, 2002, 2003],
        'pop': [1.5, 1.7, 3.6, 2.4, 2.9, 3.2]}
frame = pd.DataFrame(data)
frame
#     state  year  pop
# 0    Ohio  2000  1.5
# 1    Ohio  2001  1.7
# 2    Ohio  2002  3.6
# 3  Nevada  2001  2.4
# 4  Nevada  2002  2.9
# 5  Nevada  2003  3.2

frame.head()  # 只展示前5行
#     state  year  pop
# 0    Ohio  2000  1.5
# 1    Ohio  2001  1.7
# 2    Ohio  2002  3.6
# 3  Nevada  2001  2.4
# 4  Nevada  2002  2.9
```

按照指定顺序排列，对不包括的列名做缺失值处理

```python
frame2 = pd.DataFrame(data, columns=['year', 'state', 'pop', 'debt'],
                      index=['one', 'two', 'three', 'four', 'five', 'six'])
frame2
#        year   state  pop debt
# one    2000    Ohio  1.5  NaN
# two    2001    Ohio  1.7  NaN
# three  2002    Ohio  3.6  NaN
# four   2001  Nevada  2.4  NaN
# five   2002  Nevada  2.9  NaN
# six    2003  Nevada  3.2  NaN

frame2.columns
# Index(['year', 'state', 'pop', 'debt'], dtype='object')

frame2.head(2)
#      year state  pop debt
# one  2000  Ohio  1.5  NaN
# two  2001  Ohio  1.7  NaN
```

可按字典型标记或属性那样检索为Series

```python
frame2['state']  # 字典型标记
# one        Ohio
# two        Ohio
# three      Ohio
# four     Nevada
# five     Nevada
# six      Nevada
# Name: state, dtype: object

frame2.year  # 属性
# one      2000
# two      2001
# three    2002
# four     2001
# five     2002
# six      2003
# Name: year, dtype: int64
```

行可以通过位置或特殊的属性loc进行选取

```python
frame2.loc['three']
# year     2002
# state    Ohio
# pop       3.6
# debt      NaN
# Name: three, dtype: object

frame2['debt'] = np.arange(6)
frame2
#        year   state  pop  debt
# one    2000    Ohio  1.5     0
# two    2001    Ohio  1.7     1
# three  2002    Ohio  3.6     2
# four   2001  Nevada  2.4     3
# five   2002  Nevada  2.9     4
# six    2003  Nevada  3.2     5
```

将Series赋值给一列时，Series的索引将会按照DataFrame的索引重新排列，并在空缺的地方填充缺失值

```python
val = pd.Series([-1.2, -1.5, -1.7], index=['two', 'four', 'five'])
frame2['debt'] = val
frame2
#        year   state  pop  debt
# one    2000    Ohio  1.5   NaN
# two    2001    Ohio  1.7  -1.2
# three  2002    Ohio  3.6   NaN
# four   2001  Nevada  2.4  -1.5
# five   2002  Nevada  2.9  -1.7
# six    2003  Nevada  3.2   NaN
```

增加一列

```python
frame2['eastern'] = frame2['state'] == 'Ohio'
frame2       year   state  pop  debt  eastern
# one    2000    Ohio  1.5   NaN     True
# two    2001    Ohio  1.7  -1.2     True
# three  2002    Ohio  3.6   NaN     True
# four   2001  Nevada  2.4  -1.5    False
# five   2002  Nevada  2.9  -1.7    False
# six    2003  Nevada  3.2   NaN    False
```

删除某列

```python
del frame2['eastern']
frame2.columns  # Index(['year', 'state', 'pop', 'debt'], dtype='object')
```

