# Typora快捷键

![img](https://img2018.cnblogs.com/blog/443934/201810/443934-20181012170159282-378811511.png)  ![img](https://img2018.cnblogs.com/blog/443934/201810/443934-20181012170211920-1988294604.png) 



# *args 和 **kwargs

主要用于函数定义的不定数量的参数传递

`*args` 用于传递一个**非键值**对的可变数量的参数列表

`**kwargs `用于传递一个不定长的**键值对**参数

标准参数与`*args`、`**kwargs`使用顺序：`fargs`, `*args`,  `**kwargs`

# 虚拟环境 Virtualenv

在开发过程中，为了解决不同模块对不同 Python 版本的依赖，我们可以创建一个独立（隔离）的 Python 环境，也就是虚拟环境。 目前有两种用于创建 Python 虚拟环境的常用工具

- **`venv`** 在 Python 3.3 和更高版本中默认可用，并在 Python 3.4 和更高版本将 `pip` 和 `setuptools` 安装到创建的虚拟环境中。 
- **`virtualenv`** 需要单独安装(`pip`)，但支持 Python 2.7+ 和 Python 3.3+，并且 `pip`、`setuptools` 和 `wheel` 默认情况下始终安装到创建的虚拟环境中（无论 Python 版本如何）。

### 使用 `venv`

**创建**：`python -m venv <dir>`

```python
python -m venv <dir>
```

**激活**：`source <dir>/bin/activat e` 或 `<dir>\Scripts\activate.bat`

```python
#linux
source <dir>/bin/activate
#windows
<dir>\Scripts\activate.bat
```

**退出**：`deactivate`

### 使用 `virtualenv`

**安装模块**：`pip install virtualenv`

```python
pip install virtualenv
```
**创建**：在指定文件夹创建一个隔离的 `virtualenv` 

```python
virtualenv myproject
```
**激活(启动)**：激活隔离的环境（`virtualenv`）

```python
#linux
source myproject/bin/activate
#windows
myproject\Scripts\activate.bat
```
默认情况下，`virtualenv` 不会使用系统全局模块，通过 `--system-site-packages` 参数可创建使用系统全局模块的 `virtualenv`。或者利用 `--no-site-packages` 创建不包含任何第三方包的纯净`virtualenv`

**虚拟环境相关命令**
`mkvirtualenv envname` 默认创建目录为user目录下（可通过修改 Python 安装目录下的 Scripts 目录中的 `mkvirtualenv.bat` 文件中24行：set "venvwrapper.default_workon_home=指定目录"，并将目录添加到环境变量）

`lsvirtualenv -b` 查看虚拟环境

`workon envname` 进入虚拟环境 envname

`deactivate` 退出虚拟环境

`rmvirtualenv envname` 删除虚拟环境






# 容器 Collections

## 容器 （`Collections`）

Python 附带一个模块，它包含许多容器数据类型，名字叫做 `collections`。我们将讨论它的作用和用法。

我们将讨论的是：

- defaultdict
- counter
- deque
- namedtuple
- enum.Enum（包含在 Python 3.4 以上）

### defaultdict

与 `dict` 类型不同，你不需要检查 **key** 是否存在

```python
from collections import defaultdict

colors = (
	('Yasoob', 'Yellow'),
	('Ali', 'Blue'),
	('Arham', 'Green'),
	('Ali', 'Black'),
	('Yasoob', 'Red'),
	('Ahmed', 'Silver'),
)

favourite_colours = defaultdict(list)

for name,colour in colours:
	favourite_colours[name].append(colour)

print(favourite_colours)
```

运行输出

```python
# defaultdict(<type 'list'>,
#	{'Arham': ['Green'],
	'Yasoob': ['Yellow','Red'],
	'Ahmed': ['Silver'],
	'Ali': ['Blue','Black']
#})
```

当在一个字典中对一个键进行嵌套赋值时，如果键不存在，或出发 `KeyError`异常，`defaultdict` 允许我们用一个聪明的方式绕过这个问题。

```python
some_dict={}
some_dict['colours']['favourite'] = 'yellow'

#异常输出：KeyError: 'colours'
```

利用 `defaultdict`

```python
import collections
tree = lambda: collection.defaultdict(tree)
some_dict=tree()
some_dict['colours']['favourite'] = 'yellow'
#运行正常

import json
print(json.dumps(some_dict))
# Output: {'colours':{'favourite':'yellow'}}
```

### counter

`Counter` 是一个计数器，它可以帮助我们针对某项数据进行计数，取值和字典类似

```python
from collections import Counter

colours=(
	('Yasoob', 'Yellow'),
    ('Ali', 'Blue'),
    ('Arham', 'Green'),
    ('Ali', 'Black'),
    ('Yasoob', 'Red'),
	('Ahmed', 'Silver'),
)

favs = Counter(name for name,colour in colours)
print(favs)

# Output: Counter({'Yasoob':2,'Ali':2,'Arham':1,'Ahmed':1})

#取值
print(favs['Yasoob'])
# Output: 2
```

我们也可以利用它统计一个文件

```python
with open('filename','rb')as f:
	line_count = Counter(f)
print(line_count)
```

### deque

`deque` 提供了一个双端队列，你可以从**头/尾**两端添加或删除元素。

```python
#导入模块
from collections import deque
#创建一个 `deque` 对象
d = deque()
```

用法和 Python 的 `list`一样，并且提供了类似的方法

```python
d = deque()
d.append('1')
d.append('2')
d.append('3')

print(len(d))
## Output: 3

print(d[0])
## Output: '1'

print(d[-1])
## Output: '3'
```
从两端取出（`pop`）数据

```python
d = deque(range(5))
print(len(d))
## Output: 5

d.popleft()
## Output: 0

d.pop()
## Output: 4

print(d)
## Output: deque([1,2,3])
```

限制队列长度，当超出限制时，数据会从队列另一端被挤出去（`pop`）

```python
d = deque(maxlen=4)
d.extend([1,2,3,4])
d.append(5)
print(d)

## Output: deque([2, 3, 4, 5], maxlen=4)

d.appendleft(0)
print(d)

## Output: deque([0, 2, 3, 4], maxlen=4)
```
从任一端扩展队列

```python
d = deque([1,2,3,4,5])
d.extendleft([0])
d.extend([6,7,8])
print(d)

## Output: deque([0,1,2,3,4,5,6,7,8])
```

### namedtuple

命名元组 （ `namedtuple`）将元组变成一个针对简单任务的容器，可以像字典（`dict`）一样访问 `namedtuple`，但 **`namedtuple` 是不可变的**

```python
from collections import namedtuple

Animal = namedtuple('Animal','name age type')
perry = Animal(name='perry',age=31,type='cat')
print(perry)
## Output: Animal(name='perry', age=31, type='cat')

print(perry.name)
## Output: 'perry'
```

`namedtuple` 有两个必须的参数，分别是元组名称和字段名称

上面例子中，我们的元组名称是 `Animal`，字段名称是 `name`，`age` 和 `type`。

`namedtuple` 让你的元组变成 **自文档** ，并且 **`namedtuple` 的每个实例没有对象字典**，所以很轻量，与普通元组相比，并不需要更多的内存

然而，要记住它是一个元组，属性值在 `namedtuple` 是不可变的

```python
from collections import namedtuple

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="perry", age=31, type="cat")
perry.age = 42

## 输出:
## Traceback (most recent call last):
##     File "", line 1, in
## AttributeError: can't set attribute
```

你应该使用 **命名元组** 来让代码 **自文档，他们向后兼容于普通的元组**，这意味着你可以既使用整数索引，也可以使用名称来访问 `namedtuple`：

```python
from collections import namedtuple

Animal = namedtuple('Animal', 'name age type')
perry = Animal(name="perry", age=31, type="cat")
print(perry[0])
print(perry.age)

## 输出: perry
##      31
```

将 **命名元组** 转换为字典

```python
from collections import namedtuple

Animal = namedtuple('Animal','name age type')
perry = Animal(name="Perry", age=31, type="cat")
print(perry._asdict())

## 输出: OrderedDict([('name', 'Perry'), ('age', 31), ...
```

### enum.Enum (Python 3.4+)

在某些情况下，一个类的对象是有限且固定的，例如季节类，它只有四个对象。这种势力有限且固定的类，在 Python 中被称为枚举类，枚举对象 属于 `enum` 模块，存在于 Python3.4 以上的版本中（同时作为一个独立的 PyPI 包 `enum34` 供老版本使用）。

定义枚举类：
- 使用 `Enum()` 函数定义
- 继承 `Enum` 基类来派生枚举类

**利用 `Enum()` 函数**

`Enum()` 函数的第一个参数是枚举类类名，第二个参数是包含所有枚举值的元组或列表

```python
import enum
Season = enum.Enum('Season',('SPRING','SUMMER','FALL','WINTER'))
```

访问枚举对象

```python
#直接访问指定枚举对象              
print(Season.SPRING)  # Season.SPRING
#访问枚举成员变量
print(Season.SPRING.name)  # SPRING
#访问枚举成员值
print(Season.SPRING.value)  # 1
#根据枚举变量名访问枚举对象
print(Season['SUMMER'])  # Season.SUMMER
#根据枚举值访问枚举对象
print(Season(3))  # Season.FALL
```
Python 还为枚举提供了一个 `__members__` 属性，该属性返回一个 dict 字典，字典包含了该枚举的所有枚举实例

```python
for name,member in Season.__members__.items():
	print(member,name,member.value)
	
## Output: Season.SPRING SPRING 1
#		   Season.SUMMER SUMMER 2
#		   Season.FALL FALL 3
#		   Season.WINTER WINTER 4
```

**继承 `Enum` 基类**

通过继承 `Enum` 基类，我们可以构造一个枚举类，枚举成员名称不能重复。默认情况下，不同成员的枚举值可以重复，第二个成员视为第一个成员的**别名**，但通过枚举值获取成员时，只能获取第一个成员。如果要限制枚举值不重复，可以使用装饰器 `@unique`

```python
from enum import Enum

class Species(Enum):
    cat = 1
    dog = 2
    horse = 3
    aardvark = 4
    butterfly = 5
    owl = 6
    platypus = 7
    dragon = 8
    unicorn = 9
    # 依次类推

    # 但我们并不想关心同一物种的年龄，所以我们可以使用一个别名
    kitten = 1  # (译者注：幼小的猫咪)
    puppy = 2   # (译者注：幼小的狗狗)

```
利用枚举值获取重复值，只能获取第一个成员
```python
print(Species(1))

## Output: Species.cat
```
使用 `@unique` 装饰器时，定义重复值枚举（**报错**）

```python
from enum import Enum
from enum import unique

@unique
class Species(Enum):
    cat = 1
    dog = 2
    horse = 3
    # 依次类推

    # 但我们并不想关心同一物种的年龄，所以我们可以使用一个别名
    kitten = 1  # (译者注：幼小的猫咪)
    puppy = 2   # (译者注：幼小的狗狗)

## Output: ValueError: duplicate values found in <enum 'Species'>: kitten -> cat, puppy -> dog
```

# 枚举 Enumerate

`enumerate(iterable, start=0)`允许我们遍历数据并自动计数，可接受参数 初始index

```python
my_list = ['apple','banana','grapes','pear']
for index,value in enumerate(my_list,1):
	print(index,value)
#输出
(1, 'apple')
(2, 'banana')
(3, 'grapes')
(4, 'pear')
```

创建包含索引的元组列表

```python
my_list = ['apple','banana','grapes','pear']
counter_list = list(enumerate(my_list,1))
print(counter_list)
#输出：[(1, 'apple'), (2, 'banana'), (3, 'grapes'), (4, 'pear')]
```

# 对象自省

## dir

`dir` 列出一个对象所拥有的属性和方法

```python
my_list = ['apple','banana','grapes','pear']
dir(my_list)
# Output: ['__add__', '__class__', '__contains__', '__delattr__', '__delitem__',
# '__delslice__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__',
# '__getitem__', '__getslice__', '__gt__', '__hash__', '__iadd__', '__imul__',
# '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__',
# '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__',
# '__setattr__', '__setitem__', '__setslice__', '__sizeof__', '__str__',
# '__subclasshook__', 'append', 'count', 'extend', 'index', 'insert', 'pop',
# 'remove', 'reverse', 'sort']
```

## type 和 id

`type()` 函数返回一个对象的类型

```python
print(type(''))
# Output: <type 'str'>
print(type([]))
# Output: <type 'list'>
print(type({}))
# Output: <type 'dict'>
print(type(dict))
# Output: <type 'type'>
print(type(3))
# Output: <type 'int'>
```

```python
name = "Yasoob"
print(id(name))
# Output: 139972439030304
```



## inspect 模块

inspect 模块也提供了很多有用的函数，来获取活跃对象的信息

```python
import inspect
print(inspect.getmembers(str))
# Output: [('__add__', <slot wrapper '__add__' of ... ...
```

# 推导式 Comprehension

 推导式是可以从一个数据序列构建另一个新的数据序列的结构体。 

##  列表(`list`)推导式 

```python
multiples = [i for i in range(30) if i % 3 is 0]
print(multiples)
# Output: [0, 3, 6, 9, 12, 15, 18, 21, 24, 27]
```

## 字典( `dict` )推导式

```python
mcase = {'a': 10, 'b': 34, 'A': 7, 'Z': 3}
mcase_frequency = {
    k.lower(): mcase.get(k.lower(), 0) + mcase.get(k.upper(), 0)for k in mcase.keys()
}
# mcase_frequency == {'a': 17, 'z': 3, 'b': 34}
```

快速兑换一个字典的键和值

```python
{v: k for k, v in some_dict.items()}
```

##  集合( `set` )推导式 

```python
squared = {x**2 for x in [1, 1, 2]}
print(squared)
# Output: {1, 4}
```

# 异常

 最基本的术语里我们知道了 `try/except` 从句。可能触发异常产生的代码会放到`try`语句块里，而处理异常的代码会在 `except` 语句块里实现。这是一个简单的例子： 

```python
try:
    file = open('test.txt', 'rb')
except IOError as e:
    print('An IOError occurred. {}'.format(e.args[-1]))
# Output: An IOError occurred.No such file or directory
```

## 处理多个异常

1、把所有可能发生的异常放到一个**元组**里

```python
try:
    file = open('test.txt', 'rb')
except (IOError, EOFError) as e:
    print("An error occurred. {}".format(e.args[-1]))
# Output: An IOError occurred.No such file or directory
```

2、把每个单独的异常放到**单独**的 `except` 语句中处理，异常被按照顺序处理，或者根本不会被处理，需要注意的是：**子异常要放到父异常前**，否则子异常会被父异常忽略，而不能正确定位异常代码

```python
try:
    file = open('test.txt', 'rb')
except EOFError as e:
    print("An EOF error occurred.")
    raise e
except IOError as e:
    print("An error occurred.")
    raise e
```

3、捕获**所有**异常

```python
try:
    file = open('test.txt', 'rb')
except Exception:
    # 打印一些异常日志，如果你想要的话
    raise
```

## `finally` 从句

不管 `try` 中的代码是否异常，最后都会执行 `finally` 中的代码

```python 
try:
    file = open('test.txt', 'rb')
except IOError as e:
    print('An IOError occurred. {}'.format(e.args[-1]))
finally:
    print("This would be printed whether or not an exception occurred!")

# Output: An IOError occurred. No such file or directory
# This would be printed whether or not an exception occurred!
```

## `try/else` 从句

`try` 会捕获代码中的所有异常，有时候我们想要 `try` 只捕获某种异常，就需要 `else`， `else` 从句只会在没有异常的情况下执行，并且代码中的异常不会被捕获，在 `finally` 语句之前执行

```python
try:
    print('I am sure no exception is going to occur!')
except Exception:
    print('exception')
else:
    # 这里的代码只会在try语句里没有触发异常时运行,
    # 但是这里的异常将 *不会* 被捕获
    print('This would only run if no exception occurs. And an error here '
          'would NOT be caught.')
finally:
    print('This would be printed in every case.')

# Output: I am sure no exception is going to occur!
# This would only run if no exception occurs.
# This would be printed in every case.
```

# lambda 表达式

`lambda` 表达式是一行函数，也被称为**匿名函数**， 如果你不想在程序中对一个函数使用两次，你也许会想用lambda表达式，它们和普通的函数完全一样。 

**原型**

```python
lambda 参数:操作(参数)
```

**例子**

```python
add = lambda x, y: x + y
print(add(3, 5))
# Output: 8
```

**列表排序**

```python
a = [(1, 2), (4, 1), (9, 10), (13, -3)]
a.sort(key=lambda x: x[1])
print(a)
# Output: [(13, -3), (4, 1), (1, 2), (9, 10)]
```

**列表并行排序**

```python
data = zip(list1, list2)
data = sorted(data)
list1, list2 = map(lambda t: list(t), zip(*data))
```

# 一行式的 Python 命令

## 简易 Web Server - - 网络快速共享文件 

```python
# Python 2
python -m SimpleHTTPServer 8000

# Python 3
python -m http.server 8000
```

若不指定端口，会默认开放 `8000` 端口

## 脚本性能分析

 这可能在定位你的脚本中的性能瓶颈时，会非常奏效： 

```python
python -m cProfile my_script.py
```

 备注：`cProfile ` 是一个比 `profile` 更快的实现，因为它是用c写的 

## CSV 转换为 JSON

命令行执行

```python
python -c "import csv,json;print json.dumps(list(csv.reader(open('csv_file.csv'))))"
```

## 列表碾平（多维变一维）

 您可以通过使用 `itertools` 包中的 `itertools.chain.from_iterable` 轻松快速的辗平一个列表。 

```python
a_list = [[1, 2], [3, 4], [5, 6]]
print(list(itertools.chain.from_iterable(a_list)))
# Output: [1, 2, 3, 4, 5, 6]

# or
print(list(itertools.chain(*a_list)))
# Output: [1, 2, 3, 4, 5, 6]
```

## 一行式的构造器（初始化构造）

 避免类初始化时大量重复的赋值语句 

```python
class A(object):
    def __init__(self, a, b, c, d, e, f):
    	self.__dict__.update({k: v for k, v in locals().items() if k != 'self'})
```

# For/Else 语句

`for` 循环可以接 `else` 语句，这个 `else` 语句会在循环正常结束时执行，这意味着循环没有遇到任何 `break`，一旦程序执行 `break` ，`else` 语句将不会被执行

`for/else` 循环的基本结构

```python
for item in container:
    if search_something(item):
        # Found it!
        process(item)
        break
else:
    # Didn't find anything..
    not_found_in_container()
```

例如：找出2-10之间的数字的因子（质数）

```python
for n in range(2, 10):
    for x in range(2, n):
        if n % x == 0:
            print(n, 'equals', x, '*', n / x)
            break
    else:
        # loop fell through without finding a factor
        print(n, 'is a prime number')
```

# 上下文管理器（context manager）

 上下文管理器就是实现了上下文管理协议的对象，它允许你在需要的时候，精确的分配和释放资源，主要用于保存和恢复各种全局状态，关闭文件等，本身就是一种装饰器

## 上下文管理协议(context management protocol)

上下文管理协议包括两个方法：

-  `contextmanager.__enter__()` 从该方法进入运行时上下文，并返回当前对象或者与运行时上下文相关的其他对象。如果 `with` 语句有 `as` 关键词存在，返回值会绑定在 `as`后的变量上。 

- `contextmanager.__exit__(exc_type, exc_val, exc_tb)` 退出运行时上下文，并返回一个布尔值标示是否有需要处理的异常（如果没有返回 `True` 会抛出异常 ）。如果在执行 `with` 语句体时发生异常，那退出时参数会包括异常类型 `type`、异常值 `value`、异常追踪信息 `traceback`，否则，3个参数都是None。

## with

以 `open` 函数为例，`open` 函数的返回值是一个文件句柄，是操作系统托付给你的 Python 程序，当处理完文件就需要归还这个句柄，否则程序会超出一次能打开的文件句柄的数量上限

```python
f = open('photo.jpg', 'r+')
jpgdata = f.read()
f.close()
```

上述程序显示的调用了 `close` 关闭这个文件句柄，但在 `f=open(...)` 之后如果有异常产生，`f.close()` 将不会被调用，为了确保不管是否触发异常，文件都能关闭，可以将其写成

```python
file = open('some_file', 'w')
try:
    file.write('Hola!')
finally:
    file.close()
```

`with` 语句能够更简单的实现它，并且确保我们的文件会被关闭，而不用关心是否异常

```python
with open('some_file', 'w') as opened_file:
    opened_file.write('Hola!')
```

## 基于类的实现

一个上下文管理器的类，最起码要定义 `__enter__` 和 `__exit__` 方法，`__exit__` 需要接收三个必要参数：`type` 、`value` 、`traceback` ，程序异常时返回异常类型、异常值、异常追踪信息 ，否则全部返回 `None`

```python
class File(object):
    def __init__(self, file_name, method):
        self.file_obj = open(file_name, method)
    def __enter__(self):
        return self.file_obj
    def __exit__(self, type, value, traceback):
        self.file_obj.close()
```

`with` 语句里使用

```python
with File('demo.txt', 'w') as opened_file:
    opened_file.write('Hola!')
```

底层原理

1. `with` 语句先暂存了 `File` 类的 `__exit__` 方法
2. 然后它调用 `File` 类的 `__enter__ `方法
3. `__enter__` 方法打开文件并返回给 `with` 语句
4. 打开的文件句柄被传递给`opened_file`参数
5. 我们使用 `.write()` 来写文件
6. `with` 语句调用之前暂存的 `__exit__` 方法
7.  `__exit__ `方法关闭了文件（不论是否异常）

## 处理异常

`__exit__` 方法的三个参数：`type` 、`value` 、`traceback` 

如果程序发生异常（上述 4-6 步），Python 会将异常的 `type` 、`value` 、`traceback` 传递给 `__exit__` 方法，由它来决定如何关闭文件以及是否需要其他步骤 

我们尝试创造异常，访问文件对象一个不支持的方法

```python
with File('demo.txt', 'w') as opened_file:
    opened_file.undefined_function('Hola!')

# Output：Traceback (most recent call last):
#  File "<stdin>", line 2, in <module>
#AttributeError: 'file' object has no attribute 'undefined_function'
```

异常发生时，`with` 语句采取步骤

1. 把异常的 `type`、`value`、`traceback` 传递给 `__exit__` 方法
2. 让`__exit__` 方法来处理异常
3. 如果 `__exit__` 返回的是 `True` ，异常将被优雅的处理
4. 如果 `__exit__` 返回的是 `True` 之外的任何东西，异常将被 `with` 语句抛出

上述案例中，`__exit__` 方法返回的是 `None` (如果没有 `return` 语句那么方法会返回 `None` )。因此，`with` 语句抛出了那个异常。 

我们尝试在 `__exit__` 方法中处理异常：

```python
class File(object):
    def __init__(self, file_name, method):
        self.file_obj = open(file_name, method)
    def __enter__(self):
        return self.file_obj
    def __exit__(self, type, value, traceback):
        print("Exception has been handled")
        self.file_obj.close()
        return True

with File('demo.txt', 'w') as opened_file:
    opened_file.undefined_function()

# Output: Exception has been handled
```

 `__exit__ `方法返回了 `True`，因此没有异常会被 `with` 语句抛出。 

## 基于生成器的实现

用装饰器（decorators）和生成器（generators）实现上下文管理器

 Python有个`contextlib`模块专门用于这个目的。 我们可以使用一个生成器函数来实现一个上下文管理器，而不是使用一个类

```python
from contextlib import contextmanager

@contextmanager
def open_file(name):
    f = open(name, 'w')
    yield f
    f.close()
```

让我们小小地剖析下这个方法。 

1. Python解释器遇到了`yield`关键字。因为这个缘故它创建了一个生成器而不是一个普通的函数。
2. 因为这个装饰器，`contextmanager`会被调用并传入函数名（`open_file`）作为参数。 
3. `contextmanager`函数返回一个以`GeneratorContextManager`对象封装过的生成器。 
4. 这个`GeneratorContextManager`被赋值给`open_file`函数，我们实际上是在调用`GeneratorContextManager`对象。 

`with`  里使用

```python
with open_file('some_file') as f:
    f.write('hola!')
```

# 函数缓存

函数缓存允许我们将一个函数对于给定参数的返回值缓存起来。 

当一个I/O密集的函数被频繁使用相同的参数调用的时候，函数缓存可以节约时间。 

在Python 3.2版本以前我们只有写一个自定义的实现。在Python 3.2以后版本，有个 `lru_cache `的装饰器，允许我们将一个函数的返回值快速地缓存或取消缓存。 

## Python 3.2+

使用 `lru_cache` 实现一个斐波那契数列，`maxsize` 参数表示缓存最近多少个返回值

```python
from functools import lru_cache

@lru_cache(maxsize=32)
def fib(n):
    if n < 2:
        return n
    return fib(n-1) + fib(n-2)

>>> print([fib(n) for n in range(10)])
# Output: [0, 1, 1, 2, 3, 5, 8, 13, 21, 34]
```

我们也可以轻松对返回值清空缓存

```python
fib.cache_clear()
```

## Python 2+

你可以创建任意种类的缓存机制，有若干种方式来达到相同的效果，这完全取决于你的需要。 这里是一个一般的缓存：

```python
from functools import wraps

def memoize(function):
    memo = {}
    @wraps(function)
    def wrapper(*args):
        if args in memo:
            return memo[args]
        else:
            rv = function(*args)
            memo[args] = rv
            return rv
    return wrapper

@memoize
def fibonacci(n):
    if n < 2: return n
    return fibonacci(n - 1) + fibonacci(n - 2)

fibonacci(25)
```

# 协程

Python 中的协程和生成器很相似但又稍有不同，主要区别在于：

- 生成器是数据的生产者
- 协程是数据的消费者

创建一个生成器

```python
def fib():
	a, b = 0, 1
	while True:
		yield a
		a, b = b, a+b
```

在 `for` 循环中使用

```python
for i in fib():
	print  i
```

这样值都是动态生成的，而不是保存在一个列表中，使用 `yield` 便可获得了一协程，协程会消耗掉发送给它的值，例如 Python 的 `grep`

```python
def grep(pattern):
	print("Searching for", pattern)
	while True:
        line = (yield)
        if pattern in line:
        	print(line)
```

现在，`yield` 变成了一个协程，不再包含任何初始值，只能通过 `send()` 传值

```python
search = grep('coroutine')
next(search)
#output: Searching for coroutine
search.send("I love you")
search.send("Don't you love me?")
search.send("I love coroutine instead!")
#output: I love coroutine instead!
```

发送的值会被 `yield` 接收

`next()` 方法用于启动一个协程， 就像协程中包含的生成器并不是立刻执行，而是通过`next()`方法来响应`send()`方法。因此，你必须通过`next()`方法来执行`yield`表达式。 

也可以调用 `close()` 关闭一个协程

```python
search = grep('coroutine')
search.close()
```

# 使用 C 扩展

CPython还为开发者实现了一个有趣的特性，使用Python可以轻松调用C代码

开发者有三种方法可以在自己的Python代码中来调用C编写的函数-`ctypes`，`SWIG`，`Python/C API`。每种方式也都有各自的利弊。

首先，我们要明确为什么要在Python中调用C？

常见原因如下：

- 你要提升代码的运行速度，而且你知道C要比Python快50倍以上
- C语言中有很多传统类库，而且有些正是你想要的，但你又不想用Python去重写它们
- 想对从内存到文件接口这样的底层资源进行访问
- 不需要理由，就是想这样做

## cTypes

Python 中的 [ctypes模块](https://docs.python.org/2/library/ctypes.html) 可能是 Python 调用 C 方法中最简单的一种。ctypes模块提供了和 C 语言兼容的数据类型和函数来加载 dll 文件，因此在调用时不需对源文件做任何的修改。也正是如此奠定了这种方法的简单性。

示例如下

实现两数求和的 C 代码，保存为 `add.c`

```c
//sample C file to add 2 numbers - int and floats

#include <stdio.h>

int add_int(int, int);
float add_float(float, float);

int add_int(int num1, int num2){
    return num1 + num2;

}

float add_float(float num1, float num2){
    return num1 + num2;

}
```

 接下来将 C 文件编译为 `.so` 文件 (windows下为DLL)。下面操作会生成 adder.so 文件 

```c
#For Linux
$  gcc -shared -Wl,-soname,adder -o adder.so -fPIC add.c

#For Mac
$ gcc -shared -Wl,-install_name,adder.so -o adder.so -fPIC add.c
```

在 Python 代码中来调用它

```python
from ctypes import *

#load the shared object file
adder = CDLL('./adder.so')

#Find sum of integers
res_int = adder.add_int(4,5)
print("Sum of 4 and 5 = " + str(res_int))

#Find sum of floats
a = c_float(5.5)
b = c_float(4.1)

add_float = adder.add_float
add_float.restype = c_float
print("Sum of 5.5 and 4.1 = ", str(add_float(a, b)))
```

输出如下

```python
Sum of 4 and 5 = 9
Sum of 5.5 and 4.1 =  9.60000038147
```

在这个例子中，C 文件是自解释的，它包含两个函数，分别实现了整形求和和浮点型求和。

在 Python 文件中，一开始先导入 ctypes 模块，然后使用 CDLL 函数来加载我们创建的库文件。这样我们就可以通过变量 `adder` 来使用 C 类库中的函数了。当 `adder.add_int()` 被调用时，内部将发起一个对 C 函数 `add_int` 的调用。 ctypes 接口允许我们在调用 C 函数时使用原生 Python 中默认的字符串型和整型。

而对于其他类似布尔型和浮点型这样的类型，必须要使用正确的 ctype 类型才可以。如向 `adder.add_float()` 函数传参时, 我们要先将 Python 中的十进制值转化为 c_float 类型，然后才能传送给 C 函数。这种方法虽然简单，清晰，但是却很受限。例如，并不能在 C 中对对象进行操作。

## SWIG

SWIG 是 Simplified Wrapper and Interface Generator 的缩写。是 Python 中调用 C 代码的另一种方法。在这个方法中，开发人员必须编写一个额外的接口文件来作为 SWIG (终端工具)的入口。

Python 开发者一般不会采用这种方法，因为大多数情况它会带来不必要的复杂。而当你有一个 C/C++ 代码库需要被多种语言调用时，这将是个非常不错的选择。

示例如下(来自[SWIG官网](http://www.swig.org/tutorial.html))

~~~c
```C
#include <time.h>
double My_variable = 3.0;

int fact(int n) {
    if (n <= 1) return 1;
    else return n*fact(n-1);

}

int my_mod(int x, int y) {
    return (x%y);

}

char *get_time()
{
    time_t ltime;
    time(&ltime);
    return ctime(&ltime);

}
~~~

编译它

```c
unix % swig -python example.i
unix % gcc -c example.c example_wrap.c \
    -I/usr/local/include/python2.1
unix % ld -shared example.o example_wrap.o -o _example.so
```

最后，Python 的输出

```python
>>> import example
>>> example.fact(5)
120
>>> example.my_mod(7,3)
1
>>> example.get_time()
'Sun Feb 11 23:01:07 1996'
>>>
```

 我们可以看到，使用SWIG确实达到了同样的效果，虽然下了更多的工夫，但如果你的目标是多语言还是很值得的。 

## Python/C API

[Python/C API](https://docs.python.org/2/c-api/) 可能是被最广泛使用的方法。它不仅简单，而且可以在 C 代码中操作你的 Python 对象。

这种方法需要以特定的方式来编写 C 代码以供 Python 去调用它。所有的 Python 对象都被表示为一种叫做 PyObject 的结构体，并且 `Python.h` 头文件中提供了各种操作它的函数。例如，如果 PyObject 表示为 PyListType (列表类型)时，那么我们便可以使用 `PyList_Size()` 函数来获取该结构的长度，类似Python中的 `len(list)` 函数。大部分对 Python 原生对象的基础函数和操作在 `Python.h` 头文件中都能找到。

示例

编写一个 C 扩展，添加所有元素到一个 Python 列表(所有元素都是数字)

来看一下我们要实现的效果，这里演示了用 Python 调用 C 扩展的代码

```python
#Though it looks like an ordinary python import, the addList module is implemented in C
import addList

l = [1,2,3,4,5]
print "Sum of List - " + str(l) + " = " +  str(addList.add(l))
```

上面的代码和普通的 Python 文件并没有什么分别，导入并使用了另一个叫做 `addList `的 Python 模块。唯一差别就是这个模块并不是用 Python 编写的，而是 C。

接下来我们看看如何用 C 编写 `addList` 模块，这可能看起来有点让人难以接受，但是一旦你了解了这之中的各种组成，你就可以一往无前了。

```c
//Python.h has all the required function definitions to manipulate the Python objects
#include <Python.h>

//This is the function that is called from your python code
static PyObject* addList_add(PyObject* self, PyObject* args){

    PyObject * listObj;

    //The input arguments come as a tuple, we parse the args to get the various variables
    //In this case it's only one list variable, which will now be referenced by listObj
    if (! PyArg_ParseTuple( args, "O", &listObj ))
        return NULL;

    //length of the list
    long length = PyList_Size(listObj);

    //iterate over all the elements
    int i, sum =0;
    for (i = 0; i < length; i++) {
        //get an element out of the list - the element is also a python objects
        PyObject* temp = PyList_GetItem(listObj, i);
        //we know that object represents an integer - so convert it into C long
        long elem = PyInt_AsLong(temp);
        sum += elem;
    }

    //value returned back to python code - another python object
    //build value here converts the C long to a python integer
    return Py_BuildValue("i", sum);

}

//This is the docstring that corresponds to our 'add' function.
static char addList_docs[] =
"add(  ): add all elements of the list\n";

/* This table contains the relavent info mapping -
   <function-name in python module>, <actual-function>,
   <type-of-args the function expects>, <docstring associated with the function>
 */
static PyMethodDef addList_funcs[] = {
    {"add", (PyCFunction)addList_add, METH_VARARGS, addList_docs},
    {NULL, NULL, 0, NULL}

};

/*
   addList is the module name, and this is the initialization block of the module.
   <desired module name>, <the-info-table>, <module's-docstring>
 */
PyMODINIT_FUNC initaddList(void){
    Py_InitModule3("addList", addList_funcs,
            "Add all ze lists");

}
```

逐步解释

- `Python.h `头文件中包含了所有需要的类型( Python 对象类型的表示)和函数定义(对 Python 对象的操作)
- 接下来我们编写将要在 Python 调用的函数, 函数传统的命名方式由{模块名}_{函数名}组成，所以我们将其命名为 `addList_add`   
- 然后填写想在模块内实现函数的相关信息表，每行一个函数，以空行作为结束
- 最后的模块初始化块签名为 `PyMODINIT_FUNC init{模块名}`。

函数 `addList_add` 接受的参数类型为 PyObject 类型结构(同时也表示为元组类型，因为 Python 中万物皆为对象，所以我们先用 PyObject 来定义)。传入的参数则通过 `PyArg_ParseTuple()` 来解析。第一个参数是被解析的参数变量。第二个参数是一个字符串，告诉我们如何去解析元组中每一个元素。字符串的第 n 个字母正是代表着元组中第 n 个参数的类型。例如，"i" 代表整形，"s" 代表字符串类型, "O" 则代表一个 Python 对象。接下来的参数都是你想要通过 `PyArg_ParseTuple()` 函数解析并保存的元素。这样参数的数量和模块中函数期待得到的参数数量就可以保持一致，并保证了位置的完整性。例如，我们想传入一个字符串，一个整数和一个 Python 列表，可以这样去写

```c
int n;
char *s;
PyObject* list;
PyArg_ParseTuple(args, "siO", &n, &s, &list);
```

在这种情况下，我们只需要提取一个列表对象，并将它存储在 `listObj` 变量中。然后用列表对象中的 `PyList_Size()` 函数来获取它的长度。就像 Python 中调用 `len(list)`。

现在我们通过循环列表，使用 `PyList_GetItem(list, index)` 函数来获取每个元素。这将返回一个 `PyObject*` 对象。既然 Python 对象也能表示`PyIntType`，我们只要使用 `PyInt_AsLong(PyObj *)` 函数便可获得我们所需要的值。我们对每个元素都这样处理，最后再得到它们的总和。

总和将被转化为一个 Python 对象并通过 `Py_BuildValue()` 返回给 Python 代码，这里的 i 表示我们要返回一个 Python 整形对象。

现在我们已经编写完 C 模块了。将下列代码保存为 `setup.py`

```python
#build the modules

from distutils.core import setup, Extension

setup(name='addList', version='1.0',  \
      ext_modules=[Extension('addList', ['adder.c'])])
```

并且运行

```python
python setup.py install
```

现在应该已经将我们的 C 文件编译安装到我们的 Python 模块中了    。

在一番辛苦后，让我们来验证下我们的模块是否有效

```python
#module that talks to the C code
import addList

l = [1,2,3,4,5]
print "Sum of List - " + str(l) + " = " +  str(addList.add(l))
```

 输出结果如下 

```python
Sum of List - [1, 2, 3, 4, 5] = 15
```

如你所见，我们已经使用 Python.h API 成功开发出了我们第一个 Python C 扩展。这种方法看似复杂，但你一旦习惯，它将变的非常有效。

Python 调用 C 代码的另一种方式便是使用 [Cython](http://cython.org/) 让Python编译的更快。但是 Cython 和传统的 Python 比起来可以将它理解为另一种语言，所以我们就不在这里过多描述了。

# 目标 Python2+3

通过一些技巧可以使我们的脚本同时兼容 Python2 和 Python3

### Future 模块

通过导入 `__future__` 模块，可以让我们在旧版本体验到新版本的功能

例如：上下文管理器是 Python2.6+ 引入的新特性，如果你想在 Python2.5 中使用它可以这样做

```python
from __future__ import with_statement
```

Python3 中 `print` 已经变为一个函数。如果你想在 Python2 中使用它可以通过 `__future__` 导入：

```python
print
# Output:

from __future__ import print_function
print(print)
# Output: <built-in function print>
```

### 模块重命名

在脚本中导入模块，通常我们会这样做

```python
import foo
#or
from foo import bar
```

其实也可以

```python
import foo as foo
```

这样做可以起到和上面代码同样的功能，但更重要的是它能让你的脚本同时兼容 Python2 和 Python3 

```python
try:
	import urllib.request as urllib_request  #for python3
except ImportError:
	import urllib2 as urllib_request  #for python2
```

我们将模块导入代码包装在 `try/except` 语句中，因为 Python2 中并没有 `urllib.request` 模块。这将引起一个 `ImportError` 异常，而在 Python2 中 `urllib.request` 的功能则是由 `urllib2` 提供的，所以，当我们试图在 Python2 中导入 `urllib.request` 模块的时候，一旦我们捕获到 `ImportError` 我们将通过导入 `urllib2` 模块来替代它。

最后，`as` 关键字的作用是将导入的模块映射到 `urllib.request`，所以我们通过 `urllib_request` 这个别名就可以使用 `urllib2` 中所有类和方法了。

### 过期的 Python2 内置功能

另一个需要了解的事情就是 Python2 中有 12 个内置能供在 Python3 中已经被移除了，要确保在 Python2 代码中不要出现这些功能来保证对 Python3 的兼容，这有一个强制让你放弃 12 内置功能 的方法：

```python
from future.builtins.disabled import *
```

现在，只要你尝试在 Python3 中使用这些被遗弃的模块时，就会抛出一个 `NameError` 异常如下：

```python
from future.builtins.disabled import *

apply()
#Output: NameError: obsolete Python 2 builtin apply is disabled
```

### 标准库向下兼容的外部支持

有一些包在非官方的支持下为 Python2 提供了 Python3 的功能。例如，我们有

- enum `pip install enum34`
- singledispatch `pip install singledispatch`
- pathlib `pip install pathlib`





