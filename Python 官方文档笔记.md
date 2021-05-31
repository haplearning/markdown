# 待解决

## 内置函数
### https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__abs__
### breakpoint(\*args,\*\*kws)
### bytearray 深入理解
### bytes 深入

### float() 浮点数精度范围



# 内置函数

## abs(*x*)

返回一个数的绝对值。参数可以是一个整数或浮点数。如果参数是一个复数，则返回它的模
如果 *x* 定义了 `__abs__()`，则 `abs(*x*)` 将返回 `x.__abs__()`
```python
abs(-15)
# Output: 15
abs(3+4j)
# Output: 5
```

## all(*iterable*)

如果 *iterable* 的所有元素为真（或迭代器为空），则返回 `True` 
```python
all('123')
# Output: True
all([1,2,3])
# Output: True
all([1,0,3])
# Output: False
all([])
# Output: True
```
等价于
```python
def all(iterable):
    for element in iterable:
        if not element:
            return False
    return True
```

## any(*iterable*):

如果 *iterable* 的任一元素为真则返回 `True`，如果迭代器为空，则返回 `False`
```python
any('123')
# Output: True
any('000')
# Output: True
any('')
# Output: False
any([1,0,3])
# Output: True
any([0,0,0])
# Output: False
any([])
# Output: False
```
等价于
```python
def any(iterable):
    for element in iterable:
        if element:
            return True
    return False
```

## ascii(*object*)

类似于 `repr()`，该函数也会返回一个用于描述 *object* 的字符串。与 `repr()` 不同的是，`ascii()` 在获取 `__repr__()` 的返回值后，会使用转义序列（`\x`,`\u`,`\U`）来表示其中的非 ASCII 码字符。
```python
class Cls:
    def __repr__(self):
        # ascii与repr都会使用__repr__，
        # 但ascii会转义其中的非ASCII字符
        return "调用__repr__"

    def __str__(self):
        # ascii不使用__str__
        return "调用__str__"

a_cls = Cls()
print("repr 的返回值:{0}".format(repr(a_cls)))
print("ascii的返回值:{0}".format(ascii(a_cls)))

# Output: repr 的返回值:调用__repr__
#         ascii的返回值:\u8c03\u7528__repr__
```
`ascii()` 会将非 ASCII 码字符以 Unicode 转义序列表示：
- `\xhh` 转义序列用于表示码点范围在`U+007F` ~ `U+00FF` 之间的 Unicode 字符。
- `\uxxxx` 转义序列用于表示码点范围在`U+0100` ~ `U+FFFF` 之间的 Unicode 字符。
- `\Uxxxxxxxx` 转义序列用于表示码点范围在 `U+10000` 及以上的字符。
```python
print(ascii('µ')) # U+007F ~ U+00FF
print(ascii('鲸')) # U+0100 ~ U+FFFF 
print(ascii("😊")) # U+10000

# Output: '\xb5'
#         '\u9cb8'
#         '\U0001f60a'

```
对比 `repr()`
```python
print(repr('µ'))
print(repr('鲸'))
print(repr("😊"))

# Output: 'µ'
#         '鲸'
#         '😊'

```

## bin(x)

将一个整数转变为一个前缀为 '0b' 的二进制字符串。结果是一个合法的 Python 表达式。
如果 `x` 不是 Python 的 `int` 对象，那它需要定义 `__index__()` 方法返回一个整数。
```python
bin(3)
# Output: '0b11'
bin(-10)
# Output: '-0b1010'
```
如果不一定需要前缀 '0b' 还可以用如下方法
```python
format(14,'b')
format(14,'#b')
# Output: '1110'
#         '0b1110'

f'{14:b}'
f'{14:#b}'
# Output: '1110'
#         '0b1110'
```

## *class* bool([x])

返回一个布尔值，`True` 或 `False`。如果 *x* 是假或者被遗漏，则返回 `False`；其他情况返回 `True`。
`bool` 是 `int` 的子类，其他类不能继承自它。并且只有 `False` 和 `True` 两个实例

一个 **对象** 在默认情况下均被是为 **真值**，除非当该对象被调用时其所属类定义了 `__bool__()` 方法且返回 `False` 或是定义了 `__len__()` 方法且返回 `0`。以下列出了被毁是为 **假值** 的内置对象
- 被定义为假值的常量：`None` 或 `False`
- 任何数值类型的零：`0`,`0.0`,`0j`,`Decimal(0)`,`Fraction(0,1)`
- 空的序列和多项集：`''`,`()`,`[]`,`{}`,`set()`,`range(0)`

```python

from fractions import Fraction
from decimal import Decimal

bool()    #False
bool(None)    #False
bool(False)    #False
bool(0)    #False
bool(0.0)    #False
bool(0j)    #False
bool(Decimal(0))    #False
bool(Fraction(0,1))    #False
bool('')    #False
bool(())    #False
bool([])    #False
bool({})    #False
bool(set())    #False
bool(range(0))    #False
```

## breakpoint(\*args, \*\*kwargs) 

在函数中设置一个断点便于代码调试，后续补充

## *class* bytearray([*source*[, *encoding*[, *errors*]]])

返回一个新的由字节(8-bits 无符号)构成的可变序列，并拥有大多数可变序列的常见方法[详见：[Mutable Sequence Types](https://docs.python.org/zh-cn/3/library/stdtypes.html#typesseq-mutable)]，并且还包含 `bytes` 类型中的大多数方法[详见：[Bytes and Bytearray Operations](https://docs.python.org/3/library/stdtypes.html#bytes-methods)]

可选形参 *source* 可以用不同的方式来初始化数组
- 如果没有任何参数时，将创建一个空实例（初始化数组为 0 个元素）
```python
# bytearray() -> empty bytes array
>>> bytearray()
bytearray(b'')
```
- 如果 *source* 是一个`integer` 时，将创建一个长度为 `source` 且每个字节均为空的 bytearray 对象
```python
>>> bytearray(5)
# bytearray(int) -> bytearray
bytearray(b'\x00\x00\x00\x00\x00')
```
- *source* 是可迭代对象，且元素必须为 `[0, 255]` 中的整数，该对象会被用做数组的初始内容
```python
# bytearray(iterable_of_ints) -> bytearray
>>> bytearray(range(5))
bytearray(b'\x00\x01\x02\x03\x04')
>>> bytearray([1,2,3,4,5])
bytearray(b'\x01\x02\x03\x04\x05')
```
- 如果 *source* 是一个 `bytes` 对象，将通过缓冲器协议(buffer protocol)复制其中的二进制数据
```python
# bytearray(bytes) -> mutable copy of bytes
>>> bytearray(b'Hi!')
bytearray(b'Hi!')
```
- 如果 *source* 是一个实现了缓冲区(buffer) API 的对象时，则会使用 *source* 的只读缓冲区来初始化 bytearray 对象。
```python
# bytearray(buffer) -> mutable copy of buffer
```
- *source* 是一个字符串时，必须给定 `encoding` 参数。此时，构造函数 `bytearray()` 会通过 `str.encode()` 方法将 *source* 编码(encoding)为字节序列。
    - *encoding* 参数用于设置编码方案，会被传递给 `str.encode()`。在 [Standard Encodings](https://docs.python.org/3.7/library/codecs.html#standard-encodings) 中可查看编码方案列表。
    - *errors* 参数用于设置[错误处理方案](https://docs.python.org/3.7/library/codecs.html#error-handlers)，也会被传递给 `str.encode()`。如果 *errors* 为空，`str.encode()` 会使用默认方案 `'strict'`——该方案在出现编码错误时会抛出 `UnicodeError`。*errors* 可以是 `'ignore'`, `'replace'`, `'xmlcharrefreplace'`, `'backslashreplace'` 或任何已通过 `codecs.register_error()` 注册的名称。
```python
# bytearray(string, encoding[, errors]) -> bytearray
>>> bytearray('abcd','utf-8')
bytearray(b'abcd')
>>> bytearray('鲸','utf-8')
bytearray(b'\xe9\xb2\xb8')

>>> bytearray('鲸','ascii')
Traceback (most recent call last):
  File "<pyshell#11>", line 1, in <module>
    bytearray('鲸','ascii')
UnicodeEncodeError: 'ascii' codec can't encode character '\u9cb8' in position 0: ordinal not in range(128)
>>> bytearray('鲸','ascii','ignore')
bytearray(b'')
```

## *class* bytes([*source*[, *encoding*[, *errors*]]])

返回一个新的由 `bytes` 构成的不可变序列。包含范围为 `0 <= x < 256` 的整数。bytes 是 bytearray 的不可变版本，拥有其中不改变序列的方法和相同的索引、切片操作。
构造函数的实参和 `bytearray()` 相同

## callable(*object*)

`callable()` 函数用于检查一个对象是否是可调用的。如果返回 `True`, `object` 仍然可能调用失败；但如果返回 `False`, 调用对象 `object` 绝不会成功。
需要注意的是，**类** 是可调用的（调用类将返回一个新的实例）; 如果 **实例** 所属的类有`__call__()` 方法，则它就是可调用的，否则就是不可调用

```python
>>> callable(0)
False
>>> callable('runoob')
False

>>> def add(a,b)
...     return a+b
...
>>>callable(add)    #函数返回 True  
True

>>> class A:
...     def method(self):
...         return 0
...
>>> callable(A)    #类可调用返回 True
True
>>> a = A()
>>> callable(a)    #类没有实现 __call__, 实例不可调用，返回 False
False

>>> class B:
...     def __call__(self):
...             return 0
... 
>>> callable(B)
True
>>> b = B()
>>> callable(b)    #类实现 __call__, 实例可调用，返回 True
True
```

## chr(*i*)

返回 Unicode 码位为 *i* 的字符的字符串格式，是 `ord()` 的逆函数。

*i* 可以说 10 进制也可以是 16 进制形式的数字, 数字范围为 0 到 1,114,111 (16 进制为0x10FFFF)。

```python
>>>chr(0x30)
'0'
>>> chr(97) 
'a'
>>> chr(8364)
'€'
```

附：ASCII 码对照表

![img](https://gss3.bdstatic.com/-Po3dSag_xI4khGkpoWK1HF6hhy/baike/c0%3Dbaike116%2C5%2C5%2C116%2C38/sign=a8288ae7fc1fbe090853cb460a096756/e850352ac65c103880a07b53bc119313b17e8941.jpg)



## @classmethod

把一个方法封装成类方法（不用实例化可直接调用。）

`classmethod` 修饰符对应的函数不需要实例化，不需要 `self` 参数，但第一个参数需要是表示自身类的 `cls` 参数（类似实例方法），可以来调用类的属性，类的方法，实例化对象等。

类方法的调用有两种，一种是类直接调用(`A.func2()`)，一种是实例调用(`A().func2()`)
```python
class A(object):
    # 属性默认为类属性（可以给直接被类本身调用）
    bar = 1
    
    # 实例化方法（必须实例化类之后才能被调用）
    def func1(self):  # self : 表示实例化类后的地址id
        print ('foo') 
    
    # 类方法（不需要实例化类就可以被类本身调用）
    @classmethod
    def func2(cls):  # cls: 类本身
        print ('func2')
        print (cls.bar)  # 调用 bar 类属性
        cls().func1()   # 调用 foo 方法
	
    # 不传递传递默认self参数的方法（该方法也是可以直接被类调用的，但是这样做不标准）
    def func3():
        print('func3')
        print(A.bar)  # 属性是可以直接用类本身调用的



A.func2()        # 不需要实例化直接调用
# Output: func2
#         1
#         foo

A().func2()      # 实例调用
# Output: func2
#         1
#         foo

A.func3()        # 不传递self参，类直接调用
```

## compile(*source*, *filename*, *mode*, *flags=0*, *dont_inherit=False*, *optimize=-1* )

将 *source* 编译成代码或 AST 对象。代码对象可以被 `exec()` 或 `eval()` 执行。

参数：



- *source* -- 字符串或者 AST （Abstract Syntax Trees）对象。

- *filename* -- 代码文件名称，如果不是从文件读取代码则传递一些可辨认的值（经常会使用 '<string>'）。

- *mode* -- 指定编译代码的种类。如果 *source* 是语句序列，可以是 `'exec'`；如果是单一表达式，可以是`'eval'`；如果是单个交互式语句，可以使 `'single'`。（在最后一种情况下，如果表达式执行结果不是 `None` 将会被打印出来；如果该参数为 `'exec'`，用 `eval()` 执行代码对象将返回 `None`； 在 `'single'` 或 `'eval'` 模式编译多行代码字符串时，输入必须以至少一个换行符结尾，这使 code 模块更容易检测语句的完整性。）

- *flags*、*dont_inherit* 控制在编译 *source* 时要用到哪个 future 语句。 如果两者都未提供（或都为零）则会使用调用 `compile()` 的代码中有效的 future 语句来编译代码。 如果给出了 *flags* 参数但没有 *dont_inherit* (或是为零) 则 *flags* 参数所指定的 以及那些无论如何都有效的 future 语句会被使用。 如果 *dont_inherit* 为一个非零整数，则只使用 *flags* 参数 -- 在调用外围有效的 future 语句将被忽略

- *flags* -- 还会控制是否允许编译的源码中包含最高层级 `await`, `async for` 和 `async with`。 当设定了比特位 `ast.PyCF_ALLOW_TOP_LEVEL_AWAIT` 时，所返回代码对象在 `co_code` 中设定了 `CO_COROUTINE`，并可通过 `await eval(code_object)` 交互式地执行。

- *optimize* -- 指定编译器的优化级别；默认值 `-1` 选择与解释器的` -O` 选项相同的优化级别。显式级别为 `0` （没有优化；`__debug__` 为真）、`1` （断言被删除， `__debug__` 为假）或` 2` （文档字符串也被删除）。

如果编译的源码不合法，此函数会触发 [`SyntaxError`](https://docs.python.org/zh-cn/3/library/exceptions.html#SyntaxError) 异常；如果源码包含 null 字节，则会触发 [`ValueError`](https://docs.python.org/zh-cn/3/library/exceptions.html#ValueError) 异常。

```python
>>> str = 'for i in range(5): print(i)'
>>> c = compile(str, '', 'exec')  #编译为字节代码对象
>>> c
<code object <module> at 0x000001DEBB858E40, file "", line 1>
>>>exec(c)
0
1
2
3
4
>>> str = '3*4+5'
>>> a = compile(str, '', 'eval')
>>> eval(a)
17
```

## *class* complex([real[,imag]])

创建一个值为 **`real + imag*j`** 的复数，或者将字符串或数字转换为复数。

如果第一个参数为字符串，则不需要指定第二个参数；字符串在 `+` 或 `-` 之间不能有空格，否则触发 `ValueError` 异常。

第二个形参不能是字符串。

每个实参都可以是任意的数值类型（包括复数）。如果省略了 *imag*, 则默认值为零，构造函数会像 `int` 和 `float` 一样进行数值转换。如果两个实参都省略，则返回 `0j`

```python
>>> complex(1,2)  # real,imag实参均为数值
(1+2j)
>>> complex(2+3j,3)
(2+6j)
>>> complex(2+3j,3j)
(-1+3j)
>>> complex(1)   #只有 real 实参
(1+0j)
>>> complex('1')  #real 为字符串
(1+0j)
>>> complex('3+4j') # '+','-' 两边不能有空格
(3+4j)
```

如果 *real* 是一个对象，`complex(real, imag)` 将调用对象的 `__complex__()`方法，即 `real.__complex__()`；如果 `__complex__()` 未定义，则调用 `__float__` 方法。否则报 `TypeError` 异常 （3.8 版本添加了如果 `__float__` 也未定义，则调用`__index__()`，否则报 `TypeError` 异常）

```python
# real 为实现 __complex__ 方法的对象
# complex(real,imag)等效于 real.__complex__()+img*1j
>>> class Sample:    
... 	def __complex___(self):
... 		return 6+6j
... 
>>> complex(Sample(),3)
(6+9j)
>>> complex(Sample(),3j)
(3+6j)

# real 对象没有实现 __complex__() 方法则调用 __float__ 方法
# complex(real,imag)等效于 real.__float__()+img*1j
>>> class Sample:
... 	def __float__(self):
... 		return 3.5
...
>>> complex(Sample(),3)
(3.5+3j)
>>> complex(Sample(),3j)
(0.5+0j)
```

如果 *imag* 是一个实现了 `__float__()` 方法的对象，此时 `complex(real,imag)` 等效于 `real + imag.__float__()*1j`。如果没有实现 `__float__()` 方法则抛出异常

```python
# imag 为实现 __float__() 方法的对象
>>> class Sample:
...     def __complex__(self):
...     	return 3+3j
...     def __float__(self):
... 		return 3.5
>>> complex(3+4j, Sample())
(3+7.5j)    #从结果看 imag为对象时，默认调用 __float__() 方法（只调该方法）
```

complex 类型支持以下操作：

- 支持 `complex` 和 `bool` 转换
- 支持 `real`、`imag`、`+/-/*//`、`abs()`、`conjugate()` 等
- 支持 `==` 和 `!=`

```python
>>> bool(0j)  # 布尔转化
False
>>> (1+3j).real  #抽象方法，检索实部
1.0
>>> (1+3j).imag  #抽象方法，检索虚部
3.0
>>> (1+3j).conjugate()   #抽象方法，返回共轭复数
1-3j
>>> abs(3+4j)  #返回复数的模
5.0
>>> (1+3j).conjugate()==1-3j
True
```



## delattr(*object*, *name*)

与 `setattr()` 相对的函数，用于删除对象属性。参数是一个对象和字符串，这个字符串必须是该对象的属性。如果对象允许，函数将删除指定的属性。`delattr(x, 'foobar)` 等效于 `del x.foobar`

```python
class Sample():
    x=1
    y=2
    z=3
print(Sample.x)
print(Sample.y)
print(Sample.z)
# Output: 1
#		  2
#		  3

delattr(Sample,'z')
print(Sample.z)
# Output: AttributeError: type object 'Sample' has no attribute 'x'

del Sample.y
print(Sample.y)
# Output: AttributeError: type object 'Sample' has no attribute 'y'

```

## dict()

`dict()` 函数用来创建一个字典，dict 对象是一个字典类

语法：

- *class* dict(*\*\*kwarg*)
- *class* dict(**mapping*, *\*\*kwarg*)
- *class* dict(*iterable*, *\*\*kwarg*)

参数说明：

- **kwargs -- 关键字
- mapping -- 元素的容器
- iterable -- 可迭代对象

```python
>>> dict()  # 创建空字典
{}
>>> dict['a']=1   #直接赋值
>>> dict = {'a':1,'b':2}

>>> dict(a='a', b='b')  # 传入关键字
{'a':'a', 'b':'b'}
>>> dict(zip(['a','b','c'],[1,2,3]))  # 传入映射函数
{'a': 1, 'b': 2, 'c': 3}
>>> dict([('a', 1), ('b', 2), ('c', 3)]) #传入可迭代对象
{'a': 1, 'b': 2, 'c': 3}
>>> dict((['a',1],['b',2]))  #传入可迭代对象
{'a': 1, 'b': 2}
```

## dir()

`dir()` 函数不带参数时，返回当前范围内的变量、方法和定义的类型列表；带参数时，返回参数的属性、方法列表。

如果参数包含方法`__dir__()`，该方法将被调用，并且返回一个属性列表。这允许实现自定义 `__getattr__()` 或 `__getattribute__()` 函数的对象能够自定义 `dir()` 来报告它们的属性。

如果参数不包含 `__dir__()`，该方法将最大限度地收集参数信息。结果列表并不总是完整的，如果对象定义 `__getattr__()`，结果可能不准确



```python
>>> dir()  #获取当前模块属性列表
['__annotations__', '__builtins__', '__doc__', '__loader__', '__name__', '__package__', '__spec__']

>>> dir([])  #获取列表对象的方法
['__add__', '__class__', '__contains__', '__delattr__', '__delitem__', '__delslice__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__getitem__', '__getslice__', '__gt__', '__hash__', '__iadd__', '__imul__', '__init__', '__iter__', '__le__', '__len__', '__lt__', '__mul__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__reversed__', '__rmul__', '__setattr__', '__setitem__', '__setslice__', '__sizeof__', '__str__', '__subclasshook__', 'append', 'count', 'extend', 'index', 'insert', 'pop', 'remove', 'reverse', 'sort']
```

默认的 [`dir()`](https://docs.python.org/zh-cn/3/library/functions.html#dir) 机制对不同类型的对象行为不同，它会试图返回最相关而不是最全的信息：

- 如果对象是模块对象，则列表包含模块的属性名称。
- 如果对象是类型或类对象，则列表包含它们的属性名称，并且递归查找所有基类的属性。
- 否则，列表包含对象的属性名称，它的类属性名称，并且递归查找它的类的所有基类的属性。

返回的列表按字母表排序。例如：

```python
>>> import struct
>>> dir()   # show the names in the module namespace  # doctest: +SKIP
['__builtins__', '__name__', 'struct']
>>> dir(struct)   # show the names in the struct module # doctest: +SKIP
['Struct', '__all__', '__builtins__', '__cached__', '__doc__', '__file__',
 '__initializing__', '__loader__', '__name__', '__package__',
 '_clearcache', 'calcsize', 'error', 'pack', 'pack_into',
 'unpack', 'unpack_from']
>>> class Shape:
...     def __dir__(self):
...         return ['area', 'perimeter', 'location']
>>> s = Shape()
>>> dir(s)  # 参数为实例时，调用对象的 __dir__() 方法
['area', 'location', 'perimeter']
>>> dir(Shape)  # 参数为类对象时，递归基类的属性
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__']
>>> dir(object) # 基类属性
['__class__', '__delattr__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__']
```

## divmod(*a*, *b*)

接收两个数字类型（非复数）参数，返回一个包含商和余数的元祖 `(a // b, a % b)`

- 如果 *a* 和 *b* 都是整数，函数返回结果相当于 `(a // b, a % b)`
- 如果其中一个参数为浮点数，函数返回结果相当于 `(q, a % b)`，q 通常是 `math.floor(a/b)`，但也有可能会比 `1` 小，不过 `q * b + a % b` 会非常接近 `a`
- 如果 `a % b` 非零，则 `a % b` 符号同 `b` 一致，并且 `0 <= abs(a % b) < abs(b)`

注：在 Python 中，'//’ 为向下取整（向负无穷方向取整），例如 -9/4 = -2.25 ，-9//4 向下取整为 -3。取余计算也可通过公式：`a % b = a - b(a // b)`

```python
>>> divmod(7, 2)
(3, 1)
>>> divmod(8, 2)
(4, 0)
>>> divmod(8, -2)
(-4, 0)
>>> divmod(3, 1.3)
(2.0, 0.3999999999999999)
>>> divmod(-9,4)
(-3,3)
```

## enumerate(*iterable*, start=0)

返回一个枚举对象，*iterable* 参数必须是一个序列、迭代器或者其他支持迭代的对象，`enumerate()` 返回的迭代器的`__next__()` 方法返回一个包含计数值和迭代值的元组。

```python
>>> seasons = ['Spring', 'Summer', 'Fall', 'Winter']
>>> list(enumerate(seasons))
[(0, 'Spring'), (1, 'Summer'), (2, 'Fall'), (3, 'Winter')]
>>> list(enumerate(seasons, start=1))  #设置初始计数值
[(1, 'Spring'), (2, 'Summer'), (3, 'Fall'), (4, 'Winter')]

>>> for index, element in enumerate(seasons):   #用于for循环
... 	print(index, element)
...
0 Spring
1 Summer
2 Fall
3 Winter
```

等价于

```python
def enumerate(sequence, start=0)
	n = start
	for elem in sequence:
        yield n, elem
        n += 1
```

## eval(*expression*[, *globals*[, *locals*]])

执行一个字符串表达式，并返回表达式的值

参数：

- *expression* -- 表达式，也可以是一个代码对象
- *globals* -- 变量作用域，全局命名空间，如果被提供，则必须是一个字典对象
- *locals* -- 变量作用域，局部命名空间，如果被提供，可以是任何映射对象。

*expression* 参数会作为一个 Python 表达式（从技术上说是一个条件列表）被解析并求值，并使用 *globals* 和 *locals* 字典作为全局和局部命名空间。

 如果 *globals* 字典存在且不包含以 `__builtins__` 为键的值，则会在解析 *expression* 之前插入以此为键的对内置模块 `builtins` 的引用。 这意味着 *expression* 通常具有对标准 `builtins` 模块的完全访问权限且受限的环境会被传播。 

如果省略 *locals* 字典则其默认值为 *globals* 字典。

 如果两个字典同时省略，则表达式执行时会使用 `eval()` 被调用的环境中的 *globals* 和 *locals*。 请注意，`eval() `并没有对外围环境下的 (非局部) 嵌套作用域 的访问权限。

```python
>>> x=7
>>> eval('3*x') 
21
>>> eval('pow(2,2)')
4
>>> eval('2+2')
4
>>> eval("{'a':1,'b':2}") # 字典字符串转化为字典
{'a': 1, 'b': 2}
>>> eval('3+n',{'n':3},{'n':2}) # globals 和 locals 都存在是优先查找 locals
5
>>> n=3
>>> eval('3*n',globals(),{})  # globals() 以字典类型返回当前位置全部全局变量
9
>>> eval('3*n',{},locals())  #locals() 以字典类型返回当前位置全部局部变量
9
```

`eval()` 还用来执行任何代码对象（如 `compile()` 创建的 ），此时参数为代码对象，而不是字符串。如果编译该对象时的 *mode* 实参是 `'exec'` 那么 `eval()`返回值为 `None` 。

`exec()` 函数支持动态执行语句。`globals()` 和 `locals()` 函数各自返回当前的全局和本地字典，因此可以传递给 `eval()` 或 `exec()` 来使用

`eval()` 函数具有一定的安全隐患，推荐使用更为安全的执行包括文字的表达式字符串的 `ast.literal_eval()`

## exec(*object*[, *globals*[, *locals*]])

动态的执行储存在在字符串或文件中的 Python 语句，相比于 eval，exec可以执行更复杂的 Python 代码。

参数：

- *object*：必选参数，表示需要被指定的 Python 代码。它必须是字符串或 code 对象。如果 *object* 是一个字符串，该字符串会先被解析为一组 Python 语句，然后在执行（除非发生语法错误）。如果*object* 是一个code对象，那么它只是被简单的执行。
- *globals*：可选参数，表示全局命名空间（存放全局变量），如果被提供，则必须是一个字典对象。
- *locals*：可选参数，表示当前局部命名空间（存放局部变量），如果被提供，可以是任何映射对象。如果该参数被忽略，那么它将会取与 *globals *相同的值。

`exec()` 的返回值永远是 `None`。在传递给 `exec()` 参数的代码上下文中，`return` 和 `yield` 语句不能再函数定义之外使用

如果 *globals* 和 *locals* 都被省略，代码将在当前作用域内执行。

如果只有 *globals* 字典，该字典将同时被用于全局和局部变量。

如果同时提供了 *globals* 和 *locals*，它们会分别被用于全局和局部变量。

在模块层级上，*globals* 和 *locals* 将是同一个字典。 如果 exec 得到两个单独对象作为 *globals* 和 *locals*，则代码将如同嵌入类定义的情况一样执行。

如果 *globals* 字典不包含 `__builtins__` 键值，则将为该键插入对内建 `builtins` 模块字典的引用。因此，在将执行的代码传递给 `exec()` 之前，可以通过将自己的 `__builtins__` 字典插入到 *globals* 中来控制可以使用哪些内置代码。

```python
>>>exec('print("Hello World")')
Hello World
# 单行语句字符串
>>> exec("print ('runoob.com')")
runoob.com
 
#  多行语句字符串
>>> exec ("""for i in range(5):
...     print ("iter time: %d" % i)
... """)
iter time: 0
iter time: 1
iter time: 2
iter time: 3
iter time: 4
```

```python
x = 10
expr = """
z = 30
sum = x + y + z
print(sum)
"""
def func():
    y = 20
    exec(expr)  # 省略 globals 和 locals
    exec(expr, {'x': 1, 'y': 2})  # 只有 globals
    exec(expr, {'x': 1, 'y': 2}, {'y': 3, 'z': 4})  #globals 和 locals 都存在时
   	# 此时expr类似嵌套函数，locals为非局部变量
    
func()
# Output: 60
#		  33
#		  34
```

## filter(*function*, *iterable*)

过滤序列，过滤掉不符合条件的元素，返回一个迭代器对象，如果要转换为列表，可以使用 **list()** 来转换。

将 `iterable` 的每个元素作为参数传递给 `function`函数，函数返回 True 或 False，并由返回为 True 的元素构成一个新的迭代器。

`filter(function, iterable)` 相当于一个生成器表达式，当 function 不是 `None` 的时候为 `(item for item in iterable if function(item))`；function 是 `None` 的时候为 `(item for item in iterable if item)` 。

```python
#过滤出列表中的所有奇数
tmplist = filter(lambda x:x%2 != 0, [1, 2, 3, 4, 5, 6, 7, 8, 9, 10])
newlist = list(tmplist)
print(newlist)

# Output: [1, 3, 5, 7, 9]
```

**`itertools.filterfalse()`** 为 *function* 返回 False 时才选取 *iterable* 中元素的补充函数

类似于

```python
def filterfalse(predicate, iterable):
    if predicate is None:
        predicate = bool
        print(predicate)
    for i in iterable:
        if not predicate(i):  #bool(1)=True,bool(0)=False
            yield i
```

## float([x])

将整数或字符串转换为浮点数

如果没有实参，将返回 `0.0`

如果实参是字符串，则它必须是包含十进制数字的字符串，字符串前面可以有符号，之前也可以有空格，可选的符号有 `'+'` 和 `'-'` ； `'+'` 对创建的值没有影响，符号和数字之间不能有其他符号。

实参也可以是 NaN（非数字）、正负无穷大的字符串。确切地说，除去首尾的空格后，输入必须遵循以下语法：

```
sign           ::=  "+" | "-"
infinity       ::=  "Infinity" | "inf"
nan            ::=  "nan"
numeric_value  ::=  floatnumber | infinity | nan
numeric_string ::=  [sign] numeric_value
```

这里，`floatnumber` 是 Python 浮点数的字符串形式。字母大小写都可以，例如，“inf”、“Inf”、“INFINITY”、“iNfINity” 都可以表示正无穷大。

如果实参是整数或浮点数，则返回具有相同值（在 Python 浮点精度范围内）的浮点数。如果实参在 Python 浮点精度范围外，则会抛出 `OverflowError`

如果参数为 Python 对象，`float(x)` 会调用 `x.__float__()`，如果 `__float__()` 未定义，则调用对象的 `__index__()` 方法

```python
>>>float(1)
1.0
>>> float(112)
112.0
>>> float(-123.6)
-123.6
>>> float('123')     # 字符串
123.0
>>> float('  -3 \n')
-3.0

>>> class Sample:
...     def __float__(self):
...         return 3.4
... 
>>> float(Sample())
3.4
```

## format(*value*[, *format_spec*])

将值 *value* 按照 *format_spec* 的格式来格式化，*format_spec* 的解释取决于 *value* 实参的类型，但大多数内置类型使用标准格式化语法：[Format Specification Mini-Language](https://docs.python.org/3/library/string.html#formatspec)

默认的 *format_spec* 是一个空字符串，它通常和调用 `str(value)` 的结果相同。

```python
>>> format('test')
'test'
>>>> format(3)
'3'
```



调用 `format(value, format_spec)` 会转换成 `type(value).__format__(value, format_spec)`，所以实例字典中的 `__format__()` 方法将不会被调用。如果在对象中这个方法，但 *format_spec* 不为空，*format_spec* 或返回值不是字符串，会触发 `TypeError` 异常

**标准格式说明符** 如下

```markdown
format_spec     ::=  [[fill]align][sign][#][0][width][grouping_option][.precision][type]
fill            ::=  <any character>
align           ::=  "<" | ">" | "=" | "^"
sign            ::=  "+" | "-" | " "
width           ::=  digit+
grouping_option ::=  "_" | ","
precision       ::=  digit+
type            ::=  "b" | "c" | "d" | "e" | "E" | "f" | "F" | "g" | "G" | "n" | "o" | "s" | "x" | "X" | "%"
```

***align*** 为对齐选项，各对齐选项含义如下：

| 选项  | 意义                                                         |
| :---- | :----------------------------------------------------------- |
| `'<'` | 强制字段在可用空间内左对齐（这是大多数对象的默认值）。       |
| `'>'` | 强制字段在可用空间内右对齐（这是数字的默认值）。             |
| `'='` | 强制将填充放置在符号（如果有）之后但在数字之前。这用于以“+000000120”形式打印字段。此对齐选项仅对数字类型有效。当'0'紧接在字段宽度之前时，它成为默认值。 |
| `'^'` | 强制字段在可用空间内居中。                                   |

如果指定了一个有效的 *align* 值，则可以在该值前面加一个 *fill* 字符作为填充字符，它可以为任意字符，省略则默认位空格符

对齐选项需定义最小字段宽度，否则字段宽度将始终与填充它的数据大小相同（此时对齐没有意义）。

```python
>>> format('test','<') #不指定最小字段宽度，没有意义
'test'
>>> format('test', '<10') #省略fill,默认用‘ ’填充
'test      '
>>> format('test', '#<10') #左对齐
'test######'
>>> format('test', '#^10') #居中
'###test###'
>>> format('test', '#>10') #右对齐
'######test'
>>> format(12345, '0=10') #
'0000012345'
>>> format(12345, '=010') #'0'紧接字段宽度之前，成为默认值
'0000012345'
>>> format(-12345, '=010') #'0'紧接字段宽度之前，成为默认值
'-0000012345'
```

***sign*** 仅对数字类型有效，含义如下

| 选项  | 意义                                           |
| :---- | :--------------------------------------------- |
| `'+'` | 表示标志应该用于正数和负数。                   |
| `'-'` | 表示标志应仅用于负数（这是默认行为）。         |
| space | 表示应在正数上使用前导空格，在负数上使用减号。 |

```python
>>> format(12345, '+')
'+12345'
>>> format(-12345, '+')
'-12345'
>>> format(-12345, '-')
'-12345'
>>> format(12345, ' ')
' 12345'
>>> format(-12345, ' ')
'-12345'
```

***#***  选项可以让“替代形式”被用于转换。 替代形式可针对不同类型分别定义。 此选项仅对整数、浮点、复数和 Decimal 类型有效。 对于整数类型，当使用二进制、八进制或十六进制输出时，此选项会为输出值添加相应的 `'0b'`, `'0o'` 或 `'0x'` 前缀。 对于浮点数、复数和 Decimal 类型，替代形式会使得转换结果总是包含小数点符号，即使其不带小数。 通常只有在带有小数的情况下，此类转换的结果中才会出现小数点符号。 此外，对于 `'g'` 和 `'G'` 转换，末尾的零不会从结果中被移除。

***width*** 为一个定义最小字段宽度的十进制整数。如果未指定，则字段宽度将有内容确定。

当未显示的给出对齐方式时，在 *width* 前字段前加一个零（`'0'`）字段将为数字类型启用感知正负号的零填充。相当于设置 *fill* 填充字符为 `'0'` 且 *alignment* 类型为 `=`。

***grouping_option*** 为分隔选项，`','` 选项表示使用逗号作为千位分隔符。对于感应区域设置的分隔符，请改用`'n'` 整数表示类型。`'_'` 选项表示对浮点类型和整数类型 `'d'` 使用下划线作为千位分隔符。对于整数类型 `'b'`,`'o'`,`'x'` 和 `'X'`，将为每 4 个数位插入一个下划线。对于其他表示类型指定此选项将导致错误

```python
>>> format(123456,',')
'123,456'
>>> format(123456,'_')
'123_456'
>>> format(123, '_b') #二进制整数 
'111_1011'
>>> format(123456,'_o')
'36_1100'
>>> format(123456,'_x')
'1_e240'
>>> format(123456,'_X')
'1_E240'
```



***precision*** 是一个十进制数字，表示对于以 `'f'` 和 `'F'` 格式化的浮点数值要在 **小数点后 **显示多少个数位。或者对于以 `'g'` 或 `'G'` 格式化的浮点数值要在 **小数点前后** 共显示多少个数位。 对于非数字类型，该字段表示最大字段大小 —— 换句话说就是要使用多少个来自字段内容的字符。 对于整数值则不允许使用 *precision*。

```python
>>> format(123.456789, '.3f')  #保留3位小数
'123.457'
>>> format(123.456789, '.6g')  #保留6位有效数字
'123.457'
>>> format(003.456789)
'3.45679'
>>> format(0.0456789,'.6g')
'0.0456789'
>>> format(123, '.6g')
'123'
>>> format(123, '#.6g')
'123.000'
```

***type*** 确定了数据应如何呈现

可用的字符串表示类型

| 类型  | 意义                                         |
| :---- | :------------------------------------------- |
| `'s'` | 字符串格式。这是字符串的默认类型，可以省略。 |
| None  | 和 `'s'` 一样。                              |

可用的整数表示类型

| 类型  | 意义                                                         |
| :---- | :----------------------------------------------------------- |
| `'b'` | 二进制格式。 输出以 2 为基数的数字。                         |
| `'c'` | 字符。在打印之前将整数转换为相应的 unicode 字符。            |
| `'d'` | 十进制整数。 输出以 10 为基数的数字。                        |
| `'o'` | 八进制格式。 输出以 8 为基数的数字。                         |
| `'x'` | 十六进制格式。 输出以 16 为基数的数字，使用小写字母表示 9 以上的数码。 |
| `'X'` | 十六进制格式。 输出以 16 为基数的数字，使用大写字母表示 9 以上的数码。 |
| `'n'` | 数字。 这与 `'d'` 相似，不同之处在于它会使用当前区域设置来插入适当的数字分隔字符。 |
| None  | 和 `'d'` 相同。                                              |

除了上述表示类型外，整数还可以通过浮点表示类型来格式化（除了 `'n'` 和 `None`）。这样做时，会在格式化之前使用 float() 将整数转换为浮点数。

```python
>>> format(123,'.2f')
'123.00'
>>> format(123,'.2e')
'1.23e+02'
```



浮点数和小数值可用的表示类型有：

| 类型  | 意义                                                         |
| :---- | :----------------------------------------------------------- |
| `'e'` | 指数表示。 以使用字母 'e' 来标示指数的科学计数法打印数字。 默认的精度为 `6`。 |
| `'E'` | 指数表示。 与 `'e'` 相似，不同之处在于它使用大写字母 'E' 作为分隔字符。 |
| `'f'` | 定点表示。 将数字显示为一个定点数。 默认的精确度为 `6`。     |
| `'F'` | 定点表示。 与 `'f'` 相似，但会将 `nan` 转为 `NAN` 并将 `inf` 转为 `INF`。 |
| `'g'` | 常规格式。 对于给定的精度 `p >= 1`，这会将数值舍入到 `p` 位有效数字，再将结果以定点格式或科学计数法进行格式化，具体取决于其值的大小。准确的规则如下：假设使用表示类型 `'e'` 和精度 `p-1` 进行格式化的结果具有指数值 `exp`。 那么如果 `m <= exp < p`，其中 `m` 以 -4 表示浮点值而以 -6 表示 [`Decimal`](https://docs.python.org/zh-cn/3/library/decimal.html#decimal.Decimal) 值，该数字将使用类型 `'f'` 和精度 `p-1-exp` 进行格式化。 否则的话，该数字将使用表示类型 `'e'` 和精度 `p-1` 进行格式化。 在两种情况下，都会从有效数字中移除无意义的末尾零，如果小数点之后没有余下数字则小数点也会被移除，除非使用了 `'#'` 选项。正负无穷，正负零和 nan 会分别被格式化为 `inf`, `-inf`, `0`, `-0` 和 `nan`，无论精度如何设定。精度 `0` 会被视为等同于精度 `1`。 默认精度为 `6`。 |
| `'G'` | 常规格式。 类似于 `'g'`，不同之处在于当数值非常大时会切换为 `'E'`。 无穷与 NaN 也会表示为大写形式。 |
| `'n'` | 数字。 这与 `'g'` 相似，不同之处在于它会使用当前区域设置来插入适当的数字分隔字符。 |
| `'%'` | 百分比。 将数字乘以 100 并显示为定点 (`'f'`) 格式，后面带一个百分号。 |
| None  | 类似于 `'g'`，不同之处在于当使用定点表示法时，小数点后将至少显示一位。 默认精度与表示给定值所需的精度一样。 整体效果为与其他格式修饰符所调整的 [`str()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#str) 输出保持一致。 |



## *class* frozenset([iterable])

返回一个新的 `frozenset` 对象，它包含可选参数 *iterable* 中的元素。`frozenset` 是一个内置的集合类型，与 `set` 不同的是，`forzenset` 是不可变的并且为 `hashable`，集合内的各元素也都是` hashable`，其内容在被创建之后不能在改变。因此它被用作字典的键或其他集合的元素。

补充：hashable -- 可哈希

一个对象的哈希值如果在其生命周期内绝不改变，就被称为 *可哈希* （它需要具有 [`__hash__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__hash__) 方法），并可以同其他对象进行比较（它需要具有 [`__eq__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__eq__) 方法）。可哈希对象必须具有相同的哈希值比较结果才会相同。

```python
>>> frozenset('abc')
frozenset({'1', '2', '3'})
```

## getattr(*object*, *name*[, *default*] )

返回对象命名属性的值。*name* 必须是字符串。如果该字符串是对象的属性之一，则返回该属性的值。例如， `getattr(x, 'foobar')` 等同于 `x.foobar`。如果指定的属性不存在，且提供了 *default* 值，则返回它，否则触发 [`AttributeError`](https://docs.python.org/zh-cn/3/library/exceptions.html#AttributeError)。

```python
>>> class Sample:
...     a=1
...     def __init__(self,b):
...             self.b=b
...             self.c=3
...
>>> getattr(Sample,'a')
1
>>> getattr(Sample(2),'b')
2
>>> getattr(Sample(2),'c')
3
>>> getattr(Sample,'d',4) #属性不存在时设置默认值
4
>>> getattr(Sample, 'd')  #属性不存在，default未定义时 报错
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
AttributeError: type object 'Sample' has no attribute 'd'
```

## globals()

返回表示当前全局符号表的字典。这总是当前模块的字典（在函数或方法中，不是调用它的模块，而是定义它的模块）

```python
>>> globals()
{'__name__': '__main__', '__doc__': None, '__package__': None, '__loader__': <class '_frozen_importlib.BuiltinImporter'>, '__spec__': None, '__annotations__': {}, '__builtins__': <module 'builtins' (built-in)>, 'Sample': <class '__main__.Sample'>}
```

`globals()` 还可以当做参数传递给 `eval()` 或 `exec()` 函数

## hasattr(*object*, *name*)

判断对象是否有 对应的属性，有则返回 `True`，否则返回 `False`。

该功能是通过调用 `getattr(object, name)` 看是否有 [`AttributeError`](https://docs.python.org/zh-cn/3/library/exceptions.html#AttributeError) 异常来实现的。

```python
>>> class Sample:
...     a=1
...     def __init__(self,b):
...             self.b=b
...             self.c=3
...
>>> hasattr(Sample, 'a')
True
>>> hasattr(Sample, 'b')
False
>>> hasattr(Sample(2), 'b')
True
```



## hash(*object*)

返回该对象的哈希值（如果它有的话）。 哈希值是整数。它们在字典查找元素时用来快速比较字典的键。相同大小的数字变量有相同的哈希值（即使它们类型不同，如 1 和 1.0）。

```python
>>> x=(1,2)
>>> y=(1,2)
>>> x is y
False
>>> z={x:'sample'}    
>>> z[y]
'sample'
```



对象必须是可哈希的，大多数 Python 中的不可变内置对象都是可哈希的；可变容器（例如列表或字典）都不可哈希；不可变容器（例如元组和  frozenset ）仅当它们的元素均为可哈希时才是可哈希的）。用户定义类的实例对象默认是可哈希的。 它们在比较时一定不相同（除非是与自己比较），它们的哈希值的生成是基于它们的 [`id()`](https://docs.python.org/zh-cn/3/library/functions.html#id)

```python
>>>hash('test')            # 字符串
2314058222102390712
>>> hash(1)                 # 数字
1
>>> hash((1,))        #元祖
3430019387558
>>> hash([1,2,3])     #不可哈希类型 报错
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: unhashable type: 'list'
```

在 **hash()** 对对象使用时，所得的结果不仅和对象的内容有关，还和对象的 **id()**，也就是内存地址有关。

```python
class Test:
    def __init__(self, i):
        self.i = i
for i in range(10):
    t = Test(1)
    print(hash(t), id(t))

# Output: -9223371916071089471 1932538981400
#         120783677160 1932538834560
#         -9223371916071098722 1932538833384
#         120783677160 1932538834560
#         -9223371916071098715 1932538833496
#         120783677160 1932538834560
#         -9223371916071098715 1932538833496
#         120783677160 1932538834560
#         -9223371916071098659 1932538834392
#         120783677160 1932538834560
```

**注解**：如果对象实现了自己的 [`__hash__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__hash__) 方法，请注意，[`hash()`](https://docs.python.org/zh-cn/3/library/functions.html#hash) 根据机器的字长来截断返回值

## help([*object*])

启动内置的帮助系统（此函数主要在交互式中使用）。如果没有实参，解释器控制台里会启动交互式帮助系统。如果实参是一个字符串，则在模块、函数、类、方法、关键字或文档主题中搜索该字符串，并在控制台上打印帮助信息。如果实参是其他任意对象，则会生成该对象的帮助页。

```python
>>>help('sys')             # 查看 sys 模块的帮助
……显示帮助信息……
 
>>>help('str')             # 查看 str 数据类型的帮助
……显示帮助信息……
 
>>>a = [1,2,3]
>>>help(a)                 # 查看列表 list 帮助信息
……显示帮助信息……
 
>>>help(a.append)          # 显示list的append方法的帮助
……显示帮助信息……
```

## hex(x)

将整数转换为以 ’0x‘ 为前缀的小写十六进制字符串。如果 x 不是 Python int 对象，则必须定义返回整数的 __index\_\_() 方法。

```python
>>> hex(255)
'0xff'
>>> hex(-42)
'-0x2a'
>>> class Sample:
...     def __index__(self):
...         return 4
>>> hex(Sample())
'0x4'
```

如果要将整数转换为大写或小写的十六进制字符串，并可选择有无“0x”前缀，则可以使用如下方法：

```python
>>> '%#x' % 255, '%x' % 255, '%X' % 255
('0xff', 'ff', 'FF')
>>> format(255, '#x'), format(255, 'x'), format(255, 'X')
('0xff', 'ff', 'FF')
>>> f'{255:#x}', f'{255:x}', f'{255:X}'
('0xff', 'ff', 'FF')
```

注：如果要获取浮点数的十六进制字符串形式，请使用 [`float.hex()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#float.hex) 方法。

## id(*object*)

返回对象的 “标识值”。该值是一个整数，在此对象的生命周期中保证是唯一且恒定的。两个生命期不重叠的对象可能具有相同的 id() 值

```python
>>> id(123)
140735624409552
>>> a=1
>>> id(a)
140735624405648
>>> b=a
>>> id(b)
140735624405648
```

## input([*prompt*])

该函数接受一个标准输入数据，返回 string 类型，如果参数 *prompt* 存在，则会打印提示信息

```python
>>>a = input("input:")
input:123                  # 输入整数
>>> type(a)
<class 'str'>              # 字符串
>>> a = input("input:")    
input:runoob              # 正确，字符串表达式
>>> type(a)
<class 'str'>             # 字符串
```

## *class* int([*x*]) | *class* int(*x*, *base=10*)

该函数用于将一个字符串或数字转换为 **整型**，未给出参数时返回 `0`

```python
>>> int()
0
```

如果 x 对定义了` __int__()`，`int(x)` 将返回 `x.__int__()`（3.8 版本未定义`__int__()`则回退至 `__index__()`）；如果定义了 `__index__()`，它将返回 `x.__index__()`；如果定义了 `__trunc__()`，它将返回 `x.__trunc__()`。对于浮点数，它将向零舍入。

```python
>>> int(3.6)  #向0取整
3
>>> int(-3.6)  #向0取整
-3
>>> class Sample:    #定义 __int__() 方法
... 	def __init__(self, value):
...         self.value = value
...     def __int__(self):
...         return self.value
>>> int(Sample(6))
6

>>> class Sample:    #定义 __trunc__() 方法
...     def __init__(self,value):
...         self._value = value
...	    def __trunc__(self):
...	        return self._value**2
>>> int(Sample(6)) 
36

>>> class Sample:   #都未定义报异常
...     def __init__(self,value):
...         self._value = value
>>> int(Sample(6))
TypeError: int() argument must be a string, a bytes-like object or a number, not 'Sample'
```

*x* 还可以是以 *base* 为基数的整型字面值([*integer* *literal*](https://docs.python.org/3.7/reference/lexical_analysis.html#integers))的字符串(或 [`bytes`](https://docs.python.org/3.7/library/stdtypes.html#bytes) 和 [`bytearray`](https://docs.python.org/3.7/library/stdtypes.html#bytearray))形式，*base* 的默认值是 `10`。此时，会按照给定进制将 *x* 转换为对应的整数。

可在整型字面值字符串的前方添加 `+` (或 `-`)，字面值和 `+` (或 `-`)之间不允许有空格(*space*)，但在字面值和 `+` (或 `-`)的外围允许存在空白符(*whitespace*)。

```python
int(' \t -10 \n') #> -10
```

一个进制为 n 的数字包含 0 到 n-1 的数，其中 `a` 到 `z` （或 `A` 到 `Z` ）表示 10 到 35。默认的 *base* 为 10 ，允许的进制有 0、2-36。

```python
int('z',base=36) #> 35
```

可以用 `0b`/`0B` 、 `0o`/`0O` 、 `0x`/`0X` 前缀来表示2、8、16 进制的数字。

```python
int('0b10',base=2) #> 2
int('0o10',base=8) #> 8
int('0x10',base=16) #> 16
```

进制为 0 将安照代码的字面量来精确解释，最后的结果会是 2、8、10、16 进制中的一个。所以 `int('010', 0)` 是非法的，但 `int('010')` 和 `int('010', 8)` 是合法的。

```python
int('0b10',0) #> 2
int('0o10',0) #> 8
int('0x10',0) #> 16
int('10',0) #> 10
```

## isinstance(*object*, *classinfo*)

判断一个对象是否是一个已知的类型(*classinfo*)，类似 `type()`。

> isinstance() 与 type() 区别：
>
> - type() 不会认为子类是一种父类类型，不考虑继承关系。
> - isinstance() 会认为子类是一种父类类型，考虑继承关系。
>
> 如果要判断两个类型是否相同推荐使用 isinstance()。

*classinfo* 可以是直接或间接类名、基本类型或者由他们组成的元组。

|  类型   |  说明  | 类型  |  说明  |
| :-----: | :----: | :---: | :----: |
|   int   |  整型  |  str  | 字符串 |
|  float  | 浮点型 | list  |  列表  |
|  bool   | 布尔型 |  set  |  集合  |
| complex |  复数  | tuple |  元组  |
| object  |  基类  | dict  |  字典  |

如果对象的类型与 *classinfo* 一致，或者为其中之一（元组），则返回 True，否则返回 False。

```python
>>>a = 2
>>> isinstance (a,int)
True
>>> isinstance (a,str)
False
>>> isinstance (a,(str,int,list))    # 是元组中的一个返回 True
True
```

`type()` 和 `isinstance()` 的区别

```python
class A:
    pass
 
class B(A):
    pass
 
isinstance(A(), A)    # returns True
type(A()) == A        # returns True
isinstance(B(), A)    # returns True
type(B()) == A        # returns False
```

如果 *classinfo* 既不是类型，也不是类型元组或类型元组的元组，则将引发 [`TypeError`](https://docs.python.org/zh-cn/3/library/exceptions.html#TypeError) 异常。

## issubclass(*class*, *classinfo*)

该函数用于判断参数 *class* 是否为类型参数 *classinfo* 的子类。

*class* 是 *classinfo* 的（直接、间接或虚拟）子类则返回 `True`，否则返回 `False`

*classinfo* 可以是类型元组，此时只要 *class* 为其中一个的子类就返回 `True`

任何类都是基类 object 的子类，也是自身的子类

```python
class A:
    pass
class B(A):
    pass
  
print(issubclass(B, A))    # 返回 True
print(issubclass(B, B))    # 返回 True
print(issubclass(B, object))  # 返回 True
```

## iter(*object*[, *sentinel*])

该函数用于返回一个迭代器对象（iterator）。

*object* 必须是支持迭代协议（有 [`__iter__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__iter__) 方法）的集合对象，或必须支持序列协议（有 [`__getitem__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__getitem__) 方法，且数字参数从 `0` 开始），否则触发 [`TypeError`](https://docs.python.org/zh-cn/3/library/exceptions.html#TypeError) 异常。

```python
>>> sample = '123'
>>> next(sample)
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
TypeError: 'str' object is not an iterator
>>> sample = iter('123')
>>> next(sample)
'1'
>>> next(sample)
'2'
>>> next(sample)
'3'
```

如果有第二个实参 *sentinel*，那么 *object* 必须是 **可调用** 的对象。这种情况下生成的迭代器，每次迭代调用它的 [`__next__()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#iterator.__next__) 方法时都会不带实参地调用 *object*；如果返回的结果是 *sentinel* 则触发 [`StopIteration`](https://docs.python.org/zh-cn/3/library/exceptions.html#StopIteration) （`for` 循环会自动捕获该异常并停止调用`next()`），否则返回调用结果。

```python
class Sample:
    def __init__(self):
        self.l = [1,2,3,4,5]
        self.i = iter(self.l)
    def __call__(self):  # 定义了__call__方法的类的实例是可调用的
        item = next(self.i)
        print('__call__ is called, which would return', item)
        return item
    def __iter__(self): # 支持迭代协议（定义了 __iter__()方法）
        print("__iter__ is called!")
        return iter(self.l)
S = Sample() 
print(callable(S))  
s = iter(S,3)
for i in s:
    print(i)
    
# True
# __call__ is called,which would return 1
# 1
# __call__ is called,which would return 2
# 2
# __call__ is called,which would return 3
```

## len(*s*)

返回对象的长度（元素个数）。实参可以是序列（如 string、bytes、tuple、list 或 range 等）或集合（如 dictionary、set 或 frozen set 等）。

```python
>>>str = "runoob"
>>> len(str)             # 字符串长度
6
>>> l = [1,2,3,4,5]
>>> len(l)               # 列表元素个数
5
```

## list([*iterable*])

构造器将构造一个列表，其中的项与 *iterable* 中的项具有相同的的值与顺序。 *iterable* 可以是序列、支持迭代的容器或其它可迭代对象。 如果 *iterable* 已经是一个列表，将创建并返回其副本，类似于 `iterable[:]`

```python
>>> list()  # 无参生成空列表
[]
>>> list([1,2,3])  # 传入列表序列相当于 [:]
[1,2,3]
>>> list('123')  #传入字符串
['1', '2', '3']
>>> list((1,2,3))  #传入元组
[1,2,3]
>>> list({'a':1,'b':2})  # 字典序列返回键列表
['a', 'b']
```

## locals()

以字典类型返回当前位置的全部局部变量

对于函数, 方法, lambda 函式, 类, 以及实现了 _\_call__ 方法的类实例, 它都返回 True。

```python
>>>def runoob(arg):    # 两个局部变量：arg、z
...     z = 1
...     print (locals())
... 
>>> runoob(4)
{'z': 1, 'arg': 4}      # 返回一个名字/值对的字典
```

## map(*function*, *iterable*, …)

返回一个根据指定的函数 *function* 对指定序列做映射的迭代器。

当存在多个序列时，*function* 必须有对应序列的实参，并被应用于所有序列对象的 **并行项**。当最短迭代对象消耗完毕后，整个迭代结束

```python
>>>def square(x) :            # 计算平方数
...     return x ** 2
... 
>>> map(square, [1,2,3,4,5])   # 计算列表各个元素的平方
[1, 4, 9, 16, 25]
>>> map(lambda x: x ** 2, [1, 2, 3, 4, 5])  # 使用 lambda 匿名函数
[1, 4, 9, 16, 25]
 
# 提供了两个列表，对相同位置的列表数据进行相加
>>> map(lambda x, y: x + y, [1, 3, 5, 7, 9], [2, 4, 6, 8, 10])
[3, 7, 11, 15, 19]
```

## max(*iterable*, *[, *key*, default]) | max(*arg1*, *arg2*, **args*[, *key*])

返回可迭代对象中最大的元素，或者返回两个及以上实参中最大的。

如果只提供了一个位置参数，它必须是非空 [iterable](https://docs.python.org/zh-cn/3/glossary.html#term-iterable)，返回可迭代对象中最大的元素；如果提供了两个及以上的位置参数，则返回最大的位置参数。

有两个可选只能用关键字的实参。*key* 实参指定排序函数用的参数，如传给 [`list.sort()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#list.sort) 的。*default* 实参是当可迭代对象为空时返回的值。如果可迭代对象为空，并且没有给 *default* ，则会触发 [`ValueError`](https://docs.python.org/zh-cn/3/library/exceptions.html#ValueError)。

如果有多个最大元素，则此函数将返回第一个找到的

```python
>>> max('123')  #序列最大值
'3'
>>> max('123','456','789')  #多个位置参数，返回最大的位置参数
'789'
>>> max([],default=3) #指定默认值
3
>>> max([-3,2,1], key=abs)  #指定排序规则
-3
>>> max([-3,2,1,3], key=abs)  #多个最大返回先找到的
-3
```

## memoryview(*obj*)

返回由给定实参创建的“内存视图”对象。该对象允许 Python 代码访问一个对象的内部数据，只要该对象支持 [缓冲区协议](https://docs.python.org/zh-cn/3/c-api/buffer.html#bufferobjects) 而无需进行拷贝。

*obj* 必须支持缓冲区协议。 支持缓冲区协议的内置对象包括 [`bytes`](https://docs.python.org/zh-cn/3/library/stdtypes.html#bytes) 和 [`bytearray`](https://docs.python.org/zh-cn/3/library/stdtypes.html#bytearray)。

[`memoryview`](https://docs.python.org/zh-cn/3/library/stdtypes.html#memoryview) 具有 *元素* 的概念，即由原始对象 *obj* 所处理的基本内存单元。 对于许多简单类型例如 [`bytes`](https://docs.python.org/zh-cn/3/library/stdtypes.html#bytes) 和 [`bytearray`](https://docs.python.org/zh-cn/3/library/stdtypes.html#bytearray) 来说，一个元素就是一个字节，但是其他的类型例如 [`array.array`](https://docs.python.org/zh-cn/3/library/array.html#array.array) 可能有更大的元素。

```python
>>>v = memoryview(bytearray("abcefg", 'utf-8'))
>>> print(v[1])
98
>>> print(v[-1])
103
>>> print(v[1:4])
<memory at 0x10f543a08>
>>> print(v[1:4].tobytes())
b'bce'
```

## min(*iterable*, *[, *key*, default]) | min(*arg1*, *arg2*, **args*[, *key*])

返回可迭代对象中最小的元素，或者返回两个及以上实参中最小的。

如果只提供了一个位置参数，它必须是非空 [iterable](https://docs.python.org/zh-cn/3/glossary.html#term-iterable)，返回可迭代对象中最小的元素；如果提供了两个及以上的位置参数，则返回最小的位置参数。

有两个可选只能用关键字的实参。*key* 实参指定排序函数用的参数，如传给 [`list.sort()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#list.sort) 的。*default* 实参是当可迭代对象为空时返回的值。如果可迭代对象为空，并且没有给 *default* ，则会触发 [`ValueError`](https://docs.python.org/zh-cn/3/library/exceptions.html#ValueError)。

如果有多个最小元素，则此函数将返回第一个找到的

```python
>>> min('123')  #序列最小值
'1'
>>> min('123','456','789')  #多个位置参数，返回最小的位置参数
'123'
>>> min([],default=3) #指定默认值
3
>>> min([-3,2,1], key=abs)  #指定排序规则
1
>>> min([-3,2,1,-1], key=abs)  #多个最小返回先找到的
1
```

## next(*iterator*[, *default*])

通过调用 *iterator* 的 [`__next__()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#iterator.__next__) 方法获取下一个元素。如果迭代器耗尽，则返回给定的 *default*，如果没有默认值则触发 [`StopIteration`](https://docs.python.org/zh-cn/3/library/exceptions.html#StopIteration)。

```python
>>> s = iter('123')
>>> next(s)
'1'
>>> next(s)
'2'
>>> next(s)
'3'
>>> next(s)  #迭代器耗尽且没有默认值，异常
Traceback (most recent call last):
  File "<stdin>", line 1, in <module>
StopIteration
>>> next(s,'0')  #有默认值
'0'
```

## *class* **object**

返回一个没有特征的新对象。[`object`](https://docs.python.org/zh-cn/3/library/functions.html#object) 是所有类的基类。它具有所有 Python 类实例的通用方法。这个函数不接受任何实参。

**注解:** 由于 [`object`](https://docs.python.org/zh-cn/3/library/functions.html#object) 没有 [`__dict__`](https://docs.python.org/zh-cn/3/library/stdtypes.html#object.__dict__)，因此无法将任意属性赋给 [`object`](https://docs.python.org/zh-cn/3/library/functions.html#object) 的实例。

## oct(*x*)

将一个整数转换为一个前缀为 ”0o“ 的八进制字符串。结果是一个合法的 Python 表达式。如果 *x* 不是 Python 的 [`int`](https://docs.python.org/zh-cn/3/library/functions.html#int) 对象，那它需要定义 [`__index__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__index__) 方法返回一个整数。

```python
>>> oct(8)
'0o10'
>>> oct(-56)
'-0o70'
>>> class Sample:
... 	def __init__(self,value):
... 		self.value = value
... 	def __index__(self):
... 		return self.value
...
>>> oct(Sample(12))
'0o14'
```

如果要将整数转换为八进制字符串，并可选择有无“0o”前缀，则可以使用如下方法：

```python
>>> '%#o' % 10, '%o' % 10
('0o12', '12')
>>> format(10, '#o'), format(10, 'o')
('0o12', '12')
>>> f'{10:#o}', f'{10:o}'
('0o12', '12')
```

## open(*file*, *mode='r'*, *buffering=-1*, *encoding=None*, *errors=None*, *newline=None*, *closefd=True*, *opener=None*)

打开 *file* 并返回对应的 [file object](https://docs.python.org/zh-cn/3/glossary.html#term-file-object)。如果该文件不能打开，则触发 [`OSError`](https://docs.python.org/zh-cn/3/library/exceptions.html#OSError)。

*file* 是一个 [path-like object](https://docs.python.org/zh-cn/3/glossary.html#term-path-like-object)，表示将要打开的文件的路径（绝对路径或者当前工作目录的相对路径），也可以是要被封装的整数类型文件描述符。（如果是文件描述符，它会随着返回的 I/O 对象关闭而关闭，除非 *closefd* 被设为 `False` 。）

*mode* 是一个可选字符串，用于指定打开文件的模式。默认值是 `'r'` ，这意味着它以文本模式打开并读取。其他常见模式有：写入 `'w'` （截断已经存在的文件）；排它性创建 `'x'` ；追加写 `'a'` （在 *一些* Unix 系统上，无论当前的文件指针在什么位置，*所有* 写入都会追加到文件末尾）。在文本模式，如果 *encoding* 没有指定，则根据平台来决定使用的编码：使用 `locale.getpreferredencoding(False)` 来获取本地编码。（要读取和写入原始字节，请使用二进制模式并不要指定 *encoding*。）可用的模式有：

| 字符  | 意义                             |
| :---- | :------------------------------- |
| `'r'` | 读取（默认）                     |
| `'w'` | 写入，并先截断文件               |
| `'x'` | 排它性创建，如果文件已存在则失败 |
| `'a'` | 写入，如果文件存在则在末尾追加   |
| `'b'` | 二进制模式                       |
| `'t'` | 文本模式（默认）                 |
| `'+'` | 打开用于更新（读取与写入）       |

默认模式为 `'r'` (打开用于读取文本，与 `'rt'` 同义)。 模式 `'w+'` 与 `'w+b'` 将打开文件并清空内容。 模式 `'r+'` 与 `'r+b'` 将打开文件并不清空内容。

Python区分二进制和文本I/O。以二进制模式打开的文件（包括 *mode* 参数中的 `'b'` ）返回的内容为 `bytes`对象，不进行任何解码。在文本模式下（默认情况下，或者在 *mode* 参数中包含 `'t'` ）时，文件内容返回为 [`str`](https://docs.python.org/zh-cn/3/library/stdtypes.html#str) ，首先使用指定的 *encoding* （如果给定）或者使用平台默认的的字节编码解码。

**Python 不依赖于底层操作系统的文本文件概念；所有处理都有 Python 本身完成，因此与平台无关**

*buffering* 是一个可选的整数，用于设置缓冲策略。传递0以切换缓冲关闭（仅允许在二进制模式下），1选择行缓冲（仅在文本模式下可用），并且>1的整数以指示固定大小的块缓冲区的大小（以字节为单位）。如果没有给出 *buffering* 参数，则默认缓冲策略的工作方式如下:

- 二进制文件以固定大小的块进行缓冲；使用启发式方法选择缓冲区的大小，尝试确定底层设备的“块大小”或使用 [`io.DEFAULT_BUFFER_SIZE`](https://docs.python.org/zh-cn/3/library/io.html#io.DEFAULT_BUFFER_SIZE)。在许多系统上，缓冲区的长度通常为4096或8192字节。
- “交互式”文本文件（ [`isatty()`](https://docs.python.org/zh-cn/3/library/io.html#io.IOBase.isatty) 返回 `True` 的文件）使用行缓冲。其他文本文件使用上述策略用于二进制文件。

*encoding* 是用于解码或编码文件的编码的名称。这应该只在文本模式下使用。默认编码是依赖于平台的（不 管 [`locale.getpreferredencoding()`](https://docs.python.org/zh-cn/3/library/locale.html#locale.getpreferredencoding) 返回何值），但可以使用任何Python支持的 [text encoding](https://docs.python.org/zh-cn/3/glossary.html#term-text-encoding) 。有关支持的编码列表，请参阅 [`codecs`](https://docs.python.org/zh-cn/3/library/codecs.html#module-codecs) 模块。

*errors* 是一个可选的字符串参数，用于指定如何处理编码和解码错误 - 这不能在二进制模式下使用。可以使用各种标准错误处理程序（列在 [错误处理方案](https://docs.python.org/zh-cn/3/library/codecs.html#error-handlers) ），但是使用 [`codecs.register_error()`](https://docs.python.org/zh-cn/3/library/codecs.html#codecs.register_error) 注册的任何错误处理名称也是有效的。标准名称包括:

- 如果存在编码错误，`'strict'` 会引发 [`ValueError`](https://docs.python.org/zh-cn/3/library/exceptions.html#ValueError) 异常。 默认值 `None` 具有相同的效果。
- `'ignore'` 忽略错误。请注意，忽略编码错误可能会导致数据丢失。
- `'replace'` 会将替换标记（例如 `'?'` ）插入有错误数据的地方。
- `'surrogateescape'` 将表示任何不正确的字节作为Unicode专用区中的代码点，范围从U+DC80到U+DCFF。当在写入数据时使用 `surrogateescape` 错误处理程序时，这些私有代码点将被转回到相同的字节中。这对于处理未知编码的文件很有用。
- 只有在写入文件时才支持 `'xmlcharrefreplace'`。编码不支持的字符将替换为相应的XML字符引用 `&#nnn;`。
- `'backslashreplace'` 用Python的反向转义序列替换格式错误的数据。
- `'namereplace'` （也只在编写时支持）用 `\N{...}` 转义序列替换不支持的字符。

*newline* 控制 [universal newlines](https://docs.python.org/zh-cn/3/glossary.html#term-universal-newlines) 模式如何生效（它仅适用于文本模式）。它可以是 `None`，`''`，`'\n'`，`'\r'` 和 `'\r\n'`。它的工作原理:

- 从流中读取输入时，如果 *newline* 为 `None`，则启用通用换行模式。输入中的行可以以 `'\n'`，`'\r'` 或 `'\r\n'` 结尾，这些行被翻译成 `'\n'` 在返回呼叫者之前。如果它是 `''`，则启用通用换行模式，但行结尾将返回给调用者未翻译。如果它具有任何其他合法值，则输入行仅由给定字符串终止，并且行结尾将返回给未调用的调用者。
- 将输出写入流时，如果 *newline* 为 `None`，则写入的任何 `'\n'` 字符都将转换为系统默认行分隔符 [`os.linesep`](https://docs.python.org/zh-cn/3/library/os.html#os.linesep)。如果 *newline* 是 `''` 或 `'\n'`，则不进行翻译。如果 *newline* 是任何其他合法值，则写入的任何 `'\n'` 字符将被转换为给定的字符串。

如果 *closefd* 是 `False` 并且给出了文件描述符而不是文件名，那么当文件关闭时，底层文件描述符将保持打开状态。如果给出文件名则 *closefd* 必须为 `True` （默认值），否则将引发错误。

## ord(*c*)

以一个字符串（Unicode 字符）作为参数，返回对应的 ASCII 数值，或者 Unicode 数值。

是 `chr()` 的逆函数

```python
>>>ord('a')
97
>>> ord('€')
8364
```

## pow(*base*, *exp*[, *mod*])

返回 *base* 的 *exp* 次幂；如果 *mod* 存在，则返回 *base* 的 *exp* 次幂对 *mod* 取余（比 `pow(base, exp) % mod` 更高效）。 两参数形式 `pow(base, exp)` 等价于乘方运算符: `base**exp`。

参数必须具有数值类型。 对于混用的操作数类型，则将应用双目算术运算符的类型强制转换规则。 对于 [`int`](https://docs.python.org/zh-cn/3/library/functions.html#int) 操作数，结果具有与操作数相同的类型（强制转换后），除非第二个参数为负值；在这种情况下，所有参数将被转换为浮点数并输出浮点数结果。 例如，`10**2` 返回 `100`，但 `10**-2` 返回 `0.01`。

对于 [`int`](https://docs.python.org/zh-cn/3/library/functions.html#int) 操作数 *base* 和 *exp*，如果给出 *mod*，则 *mod* 必须为整数类型并且 *mod* 必须不为零。 如果给出 *mod* 并且 *exp* 为负值，则 *base* 必须相对于 *mod* 不可整除。 在这种情况下，将会返回 `pow(inv_base, -exp, mod)`，其中 *inv_base* 为 *base* 的倒数对 *mod* 取余。

下面的例子是 `38` 的倒数对 `97` 取余:

```python
>>> pow(38, -1, mod=97)
23
>>> 23 * 38 % 97 == 1
True
```

## print(**objects*, *sep=' '*, *end='\n'*, *file=sys.stdout*, *flush=False*)

将 *objects* 打印到 *file* 指定的文本流，以 *sep* 分隔并在末尾加上 *end*。 *sep*, *end*, *file* 和 *flush* 如果存在，它们必须以 **关键字参数** 的形式给出。

所有非关键字参数都会被转换为字符串，就像是执行了 [`str()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#str) 一样，并会被写入到流，以 *sep* 且在末尾加上 *end*。 *sep* 和 *end* 都必须为字符串；它们也可以为 `None`，这意味着使用默认值。 如果没有给出 *objects*，则 [`print()`](https://docs.python.org/zh-cn/3/library/functions.html#print) 将只写入 *end*。

*file* 参数必须是一个具有 `write(string)` 方法的对象；如果参数不存在或为 `None`，则将使用 [`sys.stdout`](https://docs.python.org/zh-cn/3/library/sys.html#sys.stdout)。 由于要打印的参数会被转换为文本字符串，因此 [`print()`](https://docs.python.org/zh-cn/3/library/functions.html#print) 不能用于二进制模式的文件对象。 对于这些对象，应改用 `file.write(...)`。

输出是否被缓存通常决定于 *file*，但如果 *flush* 关键字参数为真值，流会被强制刷新。

```python
>>> print()  #换行（end='\n'）

>>> print("Hello World")  
Hello World  
 
>>> a = 1
>>> b = 'runoob'
>>> print(a,b)
1 runoob
 
>>> print("aaa""bbb")
aaabbb
>>> print("aaa","bbb")
aaa bbb
>>> 
 
>>> print("www","runoob","com",sep=".")  # 设置间隔符
www.runoob.com
>>> for i in range(5):   #设置 end 不换行
... 	print(i,end = '')   
...
12345
```

使用 *flush* 参数生成一个 Loading 的效果：

```python
import time

print("---RUNOOB EXAMPLE ： Loading 效果---")

print("Loading",end = "")
for i in range(20):
    print(".",end = '',flush = True)
    time.sleep(0.5)
```

效果如下：

![img](https://www.runoob.com/wp-content/uploads/2017/04/loading-example.gif)

## *class* property(*fget=None*, *fset=None*, *fdel=None*, *doc=None*)

返回新式类属性

*fget* 是获取属性值的函数。 *fset* 是用于设置属性值的函数。 *fdel* 是用于删除属性值的函数。并且 *doc* 为属性描述信息。

定义一个可控属性值 x

```python
class C(object):
    def __init__(self):
        self._x = None
 
    def getx(self):
        return self._x
 
    def setx(self, value):
        self._x = value
 
    def delx(self):
        del self._x
 
    x = property(getx, setx, delx, "I'm the 'x' property.")

c=C()
print(c.x) #空
c.x=3   #设置属性信息
print(c.x) #3
del c.x  #删除属性
print(c.x) #异常，AttributeError: 'C' object has no attribute '_x'
print(C.x.__doc__)  #"I'm the 'x' property."

```

如果 *c* 是 *C* 的实例化, c.x 将触发 getter,c.x = value 将触发 setter ， del c.x 触发 deleter。如果给定 doc 参数，其将成为这个属性值的 docstring，否则 property 函数就会复制 fget 函数的 docstring（如果有的话）。

将 property 函数用作装饰器可以很方便的创建只读属性：

```python
class Parrot(object):
    def __init__(self):
        self._voltage = 100000
 
    @property
    def voltage(self):
        """Get the current voltage."""
        return self._voltage
    
p = Parrot()
print(p._voltage)  #100000  直接访问属性
print(p.vpltage)  #100000  通过getter方法获取属性
```

以上 `@property` 装饰器会将 `voltage()` 方法转化为一个具有相同名称的只读属性的 "getter"，并将 *voltage* 的文档字符串设置为 "Get the current voltage."

特征属性对象具有 `getter`, `setter` 以及 `deleter` 方法，它们可用作装饰器来创建该特征属性的副本，并将相应的访问函数设为所装饰的函数。 这最好是用一个例子来解释:

```python
class C:
    def __init__(self):
        self._x = None

    @property
    def x(self):
        """I'm the 'x' property."""
        return self._x

    @x.setter
    def x(self, value):
        self._x = value

    @x.deleter
    def x(self):
        del self._x
```

上述代码与第一个例子完全等价。 注意一定要给附加函数与原始的特征属性相同的名称 (在本例中为 `x`。)

返回的特征属性对象同样具有与构造器参数相对应的属性 `fget`, `fset` 和 `fdel`。

## range(*stop*) | range(*start*, *stop*[, *step*])

返回一个可迭代对象（类型是对象，不是列表），`list()` 函数是对象迭代器，可以把range()返回的可迭代对象转为一个列表，返回的变量类型为列表。

*start* 初始值，计数从 *start* 开始，默认从 0 开始。例如：range(5) 等价于 range(0, 5)

*stop* 结束值，计数到 *stop* 结束，但不包括 *stop*。例如：range(5) 是 [0, 1, 2, 3, 4] 没有 5

step 步长，默认为 1。例如：range(0, 5) 等价于 range(0, 5, 1)

```python
>>> range(5)  #range对象
range(0, 5)
>>> for i in range(5):
... 	print(i, end=',',flush=True)
...
0,1,2,3,4,
>>> list(range(5))
[0, 1, 2, 3, 4]
>>> list(range(0))
[]
>>>list(range(0, 30, 5))
[0, 5, 10, 15, 20, 25]
>>> list(range(0, 10, 2))
[0, 2, 4, 6, 8]
>>> list(range(0, -10, -1))
[0, -1, -2, -3, -4, -5, -6, -7, -8, -9]
>>> list(range(1, 0))
[]
```

## repr(*object*)

返回对象的 *string* 形式，对于许多类型来说，函数返回的字符串和对象传递给 `eval()` 所产生的对象具有相同的值。其他情况下，返回值是一个括在尖括号中的字符串，其中包括对象类型的名称与通常包括对象名称和地址的附加信息。类可以通过定义 `__repr__()`方法来控制函数返回的内容。

与 `ascii()` 类似，不同的是 `ascii()` 会使用转义序列（`\x`,`\u`,`\U`）来表示其中的非 ASCII 码字符。

```python
>>> repr('µ')
"'µ'"
>>> repr('鲸')
"'鲸'"
>>> repr("😊")
"'😊'"

>>> ascii('µ')
"'\\xb5'"
>>> ascii('鲸')
"'\\u9cb8'"
>>> ascii("😊")
"'\\U0001f60a'"
```

其他情况，返回对象信息

```python
class Sample:
    def __init__(self,value):
        self.value = value
    def __repr__(self):   # 定义 __repr__() 方法控制返回值
        return str(self.value)
    
repr(Sample)    # "<class '__main__.Sample'>"
repr(Sample(3))   # '3'
```

## reversed(*seq*)

返回一个反向的迭代器。*seq* 必须是一个具有 [`__reversed__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__reversed__) 方法的对象或者是支持该序列协议（具有从 `0` 开始的整数类型参数的 [`__len__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__len__) 方法和 [`__getitem__()`](https://docs.python.org/zh-cn/3/reference/datamodel.html#object.__getitem__) 方法）。

```python
>>> reversed('1234')  # 返回的是一个 iterator
<reversed at 0x233b18947b8>
>>> list(reversed('Runoob'))  # 字符串
['b', 'o', 'o', 'n', 'u', 'R']
>>> list(reversed((1,2,3,4)))  # 元组
[4, 3, 2, 1]
>>> list(reversed([1,2,3,4,5]))  # 列表
[5, 4, 3, 2, 1]
>>> list(reversed(range(5)))  # range
[4, 3, 2, 1, 0]
```

## round(*number*[, *ndigits*])

返回 *number* 舍入到小数点后 *ndigits* 位精度的值。

如果 *ndigits* 被省略或为 None，则返回最接近输入值的整数。否则返回值与 *number* 的类型相同。

```python
# ndigits不存在时，返回接近整数
>>> round(3.14) 
3
>>> round(3.54)
4

#ndigits存在时，返回值与number类型相同
>>> round(3,4)
3
>>> round(3.14,0) #浮点数类型，0表示对小数点后第一位取舍
3.0
>>> round(3.14,-1) #负数表示从右到左对整数进行取舍
0.0
```

*ndigits* 可以是任意整数值（正数，0，负数均可），<0 表示对整数位进行取舍

```python
>>> round(1234.1234,1)  #保留1位精度
1234.1
>>> round(1234.1234,0)  #对小数位第一位取舍
1234.0
>>> round(1234.5234,0)  #对小数位第一位取舍
1235.0
>>> round(1234.1234,-1)  #对个位进行取舍
1230.0
>>> round(1234.1234,-2)  #对十位进行取舍
1200.0
>>> round(1234.1234,-3)  #对百位进行取舍
1000.0
>>> round(1234.1234,-4)  #对千位进行取舍
0.0
>>> round(5234.1234, -4)  #对千位进行取舍
10000.0
```

对于支持 [`round()`](https://docs.python.org/zh-cn/3/library/functions.html#round) 的内置类型，值会被舍入到最接近的 10 的负 *ndigits* 次幂的倍数；如果与两个倍数的距离相等，则选择偶数 (因此，`round(0.5)` 和 `round(-0.5)` 均为 `0` 而 `round(1.5)` 为 `2`)。

```python
>>> round(3.14,1)  #值为最接近的10^-1=0.1的倍数(3.1/3.2最接近3.1)
3.1
>>> round(3.145,2)  # 

>>> round(0.5,0) #距离相等的值（0/1），向偶数取（0），类型与0.5相同
0.0
>>> round(1.5,0) #距离相等的值（1/2），向偶数取（2），类型与1.5相同
2.0
```

在进行舍入时，还应考虑到浮点运算存在的限制：十进制数的小数部分大多不能使用浮点数精确表示(可阅读 [Floating Point Arithmetic: Issues and Limitations](https://docs.python.org/3.7/tutorial/floatingpoint.html#tut-fp-issues))。比如在下面这个示例中，实际上浮点数 `2.675` 离 `2.67` 更近，因此舍入结果是 `2.67` 而不是我们预期中的 `2.68`。这是由于大多数十进制都不能精确的表示为二进制小数，这导致在大多数情况下，你输入的十进制浮点数都只能近似地以二进制浮点数形式储存在计算机中。

```python
>>> format(2.675, '.20')
'2.6749999999999998224'
>>> round(2.675, 2)  #值最接近10^-2=0.01的倍数（2.67与2.68取2.67）
2.67

>>> format(0.15,'.20')
'0.14999999999999999445'
>>> round(0.15,1)
0.1
```

字符串格式化可以获取更高的精度（实际是将真正的机器码值进行了舍入操作再显示）

```python
>>> import math
>>> format(math.pi, '.12g')
'3.14159265359'
>>> format(math.pi, '.2f')
'3.14'
>>> repr(math.pi)
3.141592653589793
```

对于需要精确十进制表示的使用场景，请尝试使用 [`decimal`](https://docs.python.org/zh-cn/3/library/decimal.html#module-decimal) 模块，该模块实现了适合会计应用和高精度应用的十进制运算。

另一种形式的精确运算由 [`fractions`](https://docs.python.org/zh-cn/3/library/fractions.html#module-fractions) 模块提供支持，该模块实现了基于有理数的算术运算（因此可以精确表示像 1/3 这样的数值）。

如果你是浮点运算的重度用户，你应该看一下数值运算 Python 包 NumPy 以及由 SciPy 项目所提供的许多其它数学和统计运算包。 参见 < [https://scipy.org](https://scipy.org/) >。

Python 也提供了一些工具，可以在你真的 *想要* 知道一个浮点数精确值的少数情况下提供帮助。 例如 [`float.as_integer_ratio()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#float.as_integer_ratio) 方法会将浮点数表示为一个分数。由于这是一个精确的比值，它可以被用来无损地重建原始值:

```python
>>> x = 3.14159
>>> x.as_integer_ratio()
(3537115888337719, 1125899906842624)
>>> x == 3537115888337719 / 1125899906842624
True
```

[`float.hex()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#float.hex) 方法会以十六进制（以 16 为基数）来表示浮点数，同样能给出保存在你的计算机中的精确值，这种精确的十六进制表示法可被用来精确地重建浮点值

```python
>>> x = 3.14159
>>> x.hex()
'0x1.921f9f01b866ep+1'
>>> float.fromhex('0x1.921f9f01b866ep+1')
3.14159
>>> 
```

另一个有用的工具是 [`math.fsum()`](https://docs.python.org/zh-cn/3/library/math.html#math.fsum) 函数，它有助于减少求和过程中的精度损失。 它会在数值被添加到总计值的时候跟踪“丢失的位”。 这可以很好地保持总计值的精确度， 使得错误不会积累到能影响结果总数的程度:

```python
>>> sum([0.1] * 10) == 1.0
False
>>> math.fsum([0.1] * 10) == 1.0
True
```

## *class* set([*iterable*])

返回一个新的 set 对象（无序不重复元素集），可进行关系测试，删除重复数据，还可以计算交集、差集、并集等。

```python
>>>x = set('runoob')
>>> y = set('google')
>>> x, y
(set(['b', 'r', 'u', 'o', 'n']), set(['e', 'o', 'g', 'l']))   # 重复的被删除
>>> x & y         # 交集
set(['o'])
>>> x | y         # 并集
set(['b', 'e', 'g', 'l', 'o', 'n', 'r', 'u'])
>>> x - y         # 差集
set(['r', 'b', 'u', 'n'])
>>>
```

## setattr(*object*, *name*, *value*)

对应函数 `getattr()`，用于设置属性值，该属性不一定是存在的。字符串指定一个现有属性或者新增属性。 函数会将值赋给该属性，只要对象允许这种操作。例如，`setattr(x, 'foobar', 123)` 等价于 `x.foobar = 123`。

```python
class Sample:
	bar = 1

print(Sample.bar)  #1
print(Sample().bar)  #1
getattr(Sample, 'bar')  #1
setattr(Sample,'bar', 3) 
getattr(Sample, 'bar')  #3
setattr(Sample,'foobar', 123) 
getattr(Sample, 'foobar')  #123
```

## *class* slice(*stop*) | *class* slice(*start*, *stop*[, *step*])

返回一个表示由 `range(start, stop, step)` 所指定 **索引集** 的 [slice](https://docs.python.org/zh-cn/3/glossary.html#term-slice) (切片)对象。 其中 *start* 和 *step* 参数默认为 `None`。 切片对象具有仅会返回对应参数值（或其默认值）的只读数据属性 `start`, `stop` 和 `step`。它们没有其他的显式功能；不过它们会被 NumPy 以及其他第三方扩展所使用。 切片对象也会在使用扩展索引语法时被生成。 例如: `a[start:stop:step]` 或 `a[start:stop, i]`。 请参阅 [`itertools.islice()`](https://docs.python.org/zh-cn/3/library/itertools.html#itertools.islice) 了解返回迭代器的一种替代版本。

```python
>>> slice(0,5)
slice(0, 5, None)
>>> arr = range(10)
[0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
>>> arr[slice(5)]         # 截取 5 个元素
[0, 1, 2, 3, 4]
```

在使用扩展索引语法时，会自动生成切片对象。例如 `a[start:stop:step]` 将被翻译为 `a[slice(start,stop,step)]` ，并使用 `None` 填充切片中缺少的项，然后将结果传递给 `__getitem__` 方法。因此，在使用扩展索引语法时，也可直接使用切片对象。

```python
>>> class Cls:
... 	def __getitem__(self, key):
... 		return key
...
>>> Cls()[0:1] 
slice(0,1,None)
```



## sorted(*iterable*, *\**, *key=None*, *reverse=False*)

根据 *iterable* 中的项返回一个新的已排序 **列表**。

两个可选参数均为关键字参数。

*key* 为排序规则，指定带有单个参数的函数，用于序列中的每个元素提取用于比较的键(例如 `key = str.lower`)。默认位 `None` (直接比较元素)

*reverse* 为一个布尔值，如果为 True，则每个列表元素将按反向顺序比较进行排序

```python
>>>sorted([5, 2, 3, 1, 4])
[1, 2, 3, 4, 5]                      # 默认为升序

>>> sorted("This is a test string from Andrew".split(), key=str.lower)
['a', 'Andrew', 'from', 'is', 'string', 'test', 'This']

>>> sorted([5, 2, 3, 1, 4], reverse=True)   # 倒序排序
[5, 4, 3, 2, 1]
```

使用 [`functools.cmp_to_key()`](https://docs.python.org/zh-cn/3/library/functools.html#functools.cmp_to_key) 可将老式的 *cmp* 函数（比较函数）转换为 *key* 函数（`key function（键函数或整理函数）`）。补充：比较函数意为一个可调用对象，该对象接受两个参数并比较它们，结果为小于则返回一个负数，相等则返回零，大于则返回一个正数。key function则是一个接受一个参数，并返回另一个用以排序的值的可调用对象。

使用对象的一些索引作为键对复杂对象进行排序

```python
# 多个排序规则：按分数排序，分数相同按名称排序
>>> sort_list = [{'name':'alice', 'score':38}, {'name':'bob', 'score':18}, {'name':'darl', 'score':28}, {'name':'christ', 'score':28}]
>>> sorted(sort_list, key=lambda x:(-x['score'], x['name'])) 
[{'name': 'alice', 'score': 38}, {'name': 'christ', 'score': 28}, {'name': 'darl', 'score': 28}, {'name': 'bob', 'score': 18}]

>>> student_tuples = [
...     ('john', 'A', 15),
...     ('jane', 'B', 12),
...     ('dave', 'B', 10),
... ]
>>> sorted(student_tuples, key=lambda student: student[2])   # sort by age
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

同样适用于具有命名属性的对象

```python
# 用于具有命名属性的对象
>>> class Student:
...     def __init__(self, name, grade, age):
...         self.name = name
...         self.grade = grade
...         self.age = age
...     def __repr__(self):
...         return repr((self.name, self.grade, self.age))
>>> student_objects = [
...     Student('john', 'A', 15),
...     Student('jane', 'B', 12),
...     Student('dave', 'B', 10),
... ]
>>> sorted(student_objects, key=lambda student: student.age)   # sort by age
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
```

 [`operator`](https://docs.python.org/zh-cn/3/library/operator.html#module-operator) 模块有 [`itemgetter()`](https://docs.python.org/zh-cn/3/library/operator.html#operator.itemgetter) 、 [`attrgetter()`](https://docs.python.org/zh-cn/3/library/operator.html#operator.attrgetter) 和 [`methodcaller()`](https://docs.python.org/zh-cn/3/library/operator.html#operator.methodcaller) 函数会让排序变得更简单。并且还支持多级排序

```python
>>> from operator import itemgetter, attrgetter
>>> student_tuples = [
...     ('john', 'A', 15),
...     ('jane', 'B', 12),
...     ('dave', 'B', 10),
... ]
>>> sorted(student_tuples, key=itemgetter(2))
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]
>>> sorted(student_objects, key=attrgetter('age'))
[('dave', 'B', 10), ('jane', 'B', 12), ('john', 'A', 15)]

# 多级排序
>>> sorted(student_tuples, key=itemgetter(1,2))
[('john', 'A', 15), ('dave', 'B', 10), ('jane', 'B', 12)]
>>> sorted(student_objects, key=attrgetter('grade', 'age'))
[('john', 'A', 15), ('dave', 'B', 10), ('jane', 'B', 12)]
```

内置的 `sorted()` 是稳定排序（比较结果相同的值的相对顺序不会改变）

```python
>>> data = [('red', 1), ('blue', 1), ('red', 2), ('blue', 2)]
>>> sorted(data, key=itemgetter(0))
[('blue', 1), ('blue', 2), ('red', 1), ('red', 2)]
```

与 `sort()` 区别：

- `sort()` 是列表方法，会直接修改原表，并返回 None 以避免混淆
- `sorted()` 可以接收任何可迭代对象，并返回一个新的列表

## @staticmethod

将方法转换为静态方法。静态方法不会接收隐式的第一个参数。要声明一个静态方法，请使用此语法

```python
class C:
	@staticmethod
    def f(arg1, arg2, ...): ...
```

`@staticmethod` 这样的形式称为函数的 [decorator](https://docs.python.org/zh-cn/3/glossary.html#term-decorator) 

静态方法的调用可以在类上进行 (例如 `C.f()`) 也可以在实例上进行 (例如 `C().f()`)。





## *class* str(*object*='’) | *class* str(*object=b''*, *encoding='utf-8'*, *errors='strict'*)

返回对象的字符串版本。如果 *object* 为空则返回空字符串。

如果 *encoding* 或 *errors* 均为给出，`str(object)` 返回 `object.__str__()`，这是 *object* 的 "非正式” 或格式良好的字符串表示。对于字符串对象，这是该字符串本身。如果 *object* 没有 `__str__()` 方法，则 `str()` 将回退为返回 `repr(object)`

```python
>>> str('RUNOOB')
'RUNOOB'
>>> str({'a':1,'b':2})
"{'a':1, 'b':2}"
>>> str([1,2,3,4])
'[1,2,3,4]'

>>> class Sample:
... 	def __init__(self,value):
... 		self.value = value
... 	def __str__(self):    # 定义了 __str__()方法的对象
... 		return str(self.value)
...
>>> str(Sample(6))
'6'
>>> class Sample:
... 	def __init__(self,value):
... 		self.value = value
... 	def __repr__(self):  # 没有__str__()方法回退到 __repr__()
... 		return repr(self.value)
...
>>> str(Sample(6))
'6'
```

如果 *encoding* 或 *errors* 至少给出其中之一，则 *object* 应该是一个  [bytes-like object](https://docs.python.org/zh-cn/3/glossary.html#term-bytes-like-object) (例如 [`bytes`](https://docs.python.org/zh-cn/3/library/stdtypes.html#bytes) 或 [`bytearray`](https://docs.python.org/zh-cn/3/library/stdtypes.html#bytearray))。 在此情况下，如果 *object* 是一个 [`bytes`](https://docs.python.org/zh-cn/3/library/stdtypes.html#bytes) (或 [`bytearray`](https://docs.python.org/zh-cn/3/library/stdtypes.html#bytearray)) 对象，则 `str(bytes, encoding, errors)` 等价于 [`bytes.decode(encoding, errors)`](https://docs.python.org/zh-cn/3/library/stdtypes.html#bytes.decode)。 否则的话，会在调用 [`bytes.decode()`](https://docs.python.org/zh-cn/3/library/stdtypes.html#bytes.decode) 之前获取缓冲区对象下层的 bytes 对象。

```python
>>> str(b'Zoot!', encoding='utf-8')
'Zoot!'

>>> str(b'Zoot!')  # 只传入 bytes 缺少 encoding或errors参数，
"b'Zoot!'"
```

## sum(*iterable*, */*, *start=0*)

从 *start* （初始值，非下标）开始自左向右对 *iterable* 的项求和并返回总计值。*iterable* 通常为数字，而 *start* 值则不运行为字符串。

```python
>>> sum((2,3,4),1)  #等价于 1+2+3+4
10
>>> sum([0,1,2,3,4], 2)      # 列表计算总和后再加 2
12
```

对某些用例来说，存在 [`sum()`](https://docs.python.org/zh-cn/3/library/functions.html#sum) 的更好替代。 拼接字符串序列的更好更快方式是调用 `''.join(sequence)`。 要以扩展精度对浮点值求和，请参阅 [`math.fsum()`](https://docs.python.org/zh-cn/3/library/math.html#math.fsum)。 要拼接一系列可迭代对象，请考虑使用 [`itertools.chain()`](https://docs.python.org/zh-cn/3/library/itertools.html#itertools.chain)。

```python
>>> '+'.join('1234')
'1+2+3+4'

>>> sum([.1]*10)
0.9999999999999999
>>> import math
>>> math.fsum([.1]*10)
1.0

>>> import itertools
>>> list(itertools.chain('123','456'))
['1', '2', '3', '4', '5', '6']
```

## super([*type*[, *object-or-type*]])

返回一个代理对象，它会将方法调用委托给 *type* 的父类或兄弟类。 这对于访问已在类中被重载的继承方法很有用

*object-or-type* 确定用于搜索的 [method resolution order](https://docs.python.org/zh-cn/3/glossary.html#term-method-resolution-order)。 搜索会从 *type* 之后的类开始。举例来说，如果 *object-or-type* 的 [`__mro__`](https://docs.python.org/zh-cn/3/library/stdtypes.html#class.__mro__) 为 `D -> B -> C -> A -> object` 并且 *type* 的值为 `B`，则 [`super()`](https://docs.python.org/zh-cn/3/library/functions.html#super) 将会搜索 `C -> A -> object`。

*object-or-type* 的 [`__mro__`](https://docs.python.org/zh-cn/3/library/stdtypes.html#class.__mro__) 属性列出了 [`getattr()`](https://docs.python.org/zh-cn/3/library/functions.html#getattr) 和 [`super()`](https://docs.python.org/zh-cn/3/library/functions.html#super) 所共同使用的方法解析搜索顺序。 该属性是动态的，可以在任何继承层级结构发生更新的时候被改变。

 *super* 有两个典型用例。 在具有单继承的类层级结构中，*super* 可用来引用父类而不必显式地指定它们的名称，从而令代码更易维护。 这种用法与其他编程语言中 *super* 的用法非常相似。

第二个用例是在动态执行环境中支持协作多重继承。 此用例为 Python 所独有，在静态编译语言或仅支持单继承的语言中是不存在的。 这使得实现“菱形图”成为可能，在这时会有多个基类实现相同的方法。 好的设计强制要求这种方法在每个情况下具有相同的调用签名（因为调用顺序是在运行时确定的，也因为该顺序要适应类层级结构的更改，还因为该顺序可能包含在运行时之前未知的兄弟类）。

对于以上两个用例，典型的超类调用看起来是这样的:

```python
class C(B):
    def method(self, arg):
        super().method(arg)    # This does the same thing as:
                               # super(C, self).method(arg)
```

对于单继承来讲，`super`获取的类刚好是父类，`super().__init__()`与`Base.__init__()`是一样的。super()避免了基类的显式调用。

```python
class Base(object):
    def __init__(self):
        print 'Create Base'

class A(Base):
    def __init__(self):
        Base.__init__(self)
        print 'Create A'

A()

# 测试结果
Create Base
Create A


class Base(object):
    def __init__(self):
        print 'Create Base'

class A(Base):
    def __init__(self):
        super(A, self).__init__()
        # super().__init()  python3
        print 'Create A'

A()

# 测试结果
Create Base
Create A
```

多继承时，`super`获取的是继承顺序(`MRO`)中的下一个类，并且避免了基类的多次调用，节省系统开销

- 使用 super

```python
class Base(object):
    def __init__(self):
        print("enter Base")
        print("leave Base")

class A(Base):
    def __init__(self):
        print("enter A")
        super(A, self).__init__()
        print("leave A")

class B(Base):
    def __init__(self):
        print("enter B")
        super(B, self).__init__()
        print("leave B")

class C(A, B):
    def __init__(self):
        print("enter C")
        super(C, self).__init__()
        print("leave C")
        
print(C.mro())
C()
# 输出
# [<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class '__main__.Base'>, <class 'object'>]
# enter C
# enter A
# enter B
# enter Base
# leave Base
# leave B
# leave A
# leave C
```

不使用 super

```python
class Base(object):
    def __init__(self):
        print("enter Base")
        print("leave Base")

class A(Base):
    def __init__(self):
        print("enter A")
        Base().__init__()
        print("leave A")

class B(Base):
    def __init__(self):
        print("enter B")
        Base().__init__()
        print("leave B")

class C(A, B):
    def __init__(self):
        print("enter C")
        A().__init__()
        B().__init__()
        print("leave C")
print(C.mro())
C()
# 测试结果
# [<class '__main__.C'>, <class '__main__.A'>, <class '__main__.B'>, <class '__main__.Base'>, <class 'object'>]
# enter C
# enter A
# enter Base
# leave Base
# enter Base
# leave Base
# leave A
# enter A
# enter Base
# leave Base
# enter Base
# leave Base
# leave A
# enter B
# # enter Base
# leave Base
# enter Base
# leave Base
# leave B
# enter B
# enter Base
# leave Base
# enter Base
# leave Base
# leave B
# leave C
```



## tuple([*iterable*])

构造器将构造一个元组，其中的项与 *iterable* 中的项具有相同的值与顺序。 *iterable* 可以是序列、支持迭代的容器或其他可迭代对象。 如果 *iterable* 已经是一个元组，会不加改变地将其返回。如果参数为空，则返回一个空元组

```python
>>> tuple()  #无参
()
>>> tuple('abcd')  #字符串
('a', 'b', 'c', 'd')
>>> tuple([1,2,3,4])  #列表
(1,2,3,4)
>>> tuple({'a':1,'b':2,'c':3})   #字典保留键
('a','b','c')
```

## *calss* type(*object*) | *class* type(*name*, *bases*, *dict*)

传入一个参数时，返回 *object* 的类型。返回值是一个 type 对象，通常与[`object.__class__`](https://docs.python.org/zh-cn/3/library/stdtypes.html#instance.__class__) 所返回的对象相同。

推荐使用 [`isinstance()`](https://docs.python.org/zh-cn/3/library/functions.html#isinstance) 内置函数来检测对象的类型，因为它会考虑子类继承的情况。

```python
print(type('1'))  # <class 'str'>
print('1'.__class__)  # <class 'str'>
print(type(1)==int)  #True

# isinstance()考虑继承问题，而type()不考虑
class A:
    pass
 
class B(A):
    pass
 
isinstance(A(), A)    # returns True
type(A()) == A        # returns True
isinstance(B(), A)    # returns True
type(B()) == A        # returns False
```

传入三个参数时，返回一个新的 type 对象。 这在本质上是 [`class`](https://docs.python.org/zh-cn/3/reference/compound_stmts.html#class) 语句的一种动态形式。 *name* 字符串即类名并且会成为 [`__name__`](https://docs.python.org/zh-cn/3/library/stdtypes.html#definition.__name__) 属性；*bases* 元组列出基类并且会成为 [`__bases__`](https://docs.python.org/zh-cn/3/library/stdtypes.html#class.__bases__) 属性；而 *dict* 字典为包含类主体定义的命名空间并且会被复制到一个标准字典成为 [`__dict__`](https://docs.python.org/zh-cn/3/library/stdtypes.html#object.__dict__) 属性。 例如，下面两条语句会创建相同的 [`type`](https://docs.python.org/zh-cn/3/library/functions.html#type) 对象:

```python
>>> class X:
...     a = 1
...
>>> X = type('X', (object,), dict(a=1))
```

## vars([*object*])

返回模块、类、实例或任何其他具有 [`__dict__`](https://docs.python.org/zh-cn/3/library/stdtypes.html#object.__dict__) 属性的对象的 [`__dict__`](https://docs.python.org/zh-cn/3/library/stdtypes.html#object.__dict__) 属性。 

模块和实例这样的对象具有可更新的 [`__dict__`](https://docs.python.org/zh-cn/3/library/stdtypes.html#object.__dict__) 属性；但是，其它对象的 [`__dict__`](https://docs.python.org/zh-cn/3/library/stdtypes.html#object.__dict__) 属性可能会设为限制写入（例如，类会使用 [`types.MappingProxyType`](https://docs.python.org/zh-cn/3/library/types.html#types.MappingProxyType) 来防止直接更新字典）。

不带参数时，[`vars()`](https://docs.python.org/zh-cn/3/library/functions.html#vars) 的行为类似 [`locals()`](https://docs.python.org/zh-cn/3/library/functions.html#locals)。 请注意，locals 字典仅对于读取起作用，因为对 locals 字典的更新会被忽略。

```python
>>> class C:
...     a=1
...
>>> vars(C)
mappingproxy({'__module__': '__main__', 
              'a': 1, 
              '__dict__': <attribute '__dict__' of 'C' objects>, 
              '__weakref__': <attribute '__weakref__' of 'C' objects>, 
              '__doc__': None})

```

## zip(\**iterable*)

返回一个 **元组** 的迭代器，其中的**第 *i* 个元组**包含来自每个参数序列或可迭代对象的**第 *i* 个元素**。 当所输入可迭代对象中**最短**的一个被耗尽时，迭代器将停止迭代。 当只有一个可迭代对象参数时，它将返回一个单元组的迭代器。 不带参数时，它将返回一个空迭代器。相当于 

```python
def zip(*iterables):
    # zip('ABCE','xy') --> Ax By
    sentinel = object()
    iterators = [iter(it) for it in iterables]
    while iterators:
        result = []
        for itor in iterators:
            elem = next(itor, sentinel)
            if elem is sentinel:
                return
            result.append(elem)
        yield tuple(result)
```

```python
>>>a = [1,2,3]
>>> b = [4,5,6]
>>> c = [4,5,6,7,8]
>>> zipped = zip(a,b)     # 返回一个对象
>>> zipped
<zip object at 0x103abc288>
>>> list(zipped)  # list() 转换为列表
[(1, 4), (2, 5), (3, 6)]
>>> list(zip(a,c))              # 元素个数与最短的列表一致
[(1, 4), (2, 5), (3, 6)]
```

[`zip()`](https://docs.python.org/zh-cn/3/library/functions.html#zip) 与 `*` 运算符相结合可以用来拆解一个列表:

```python
>>> x=[1,2,3]
>>> y=[4,5,6]
>>> list(zip(x,y))
[(1,4),(2,5),(3,6)]
>>> x2, y2 = zip(*zip(x,y))
>>> x2, y2
(1,2,3), (4,5,6)
>>> list(x2)==x and list(y2)==y
True
```

当你不用关心较长可迭代对象末尾不匹配的值时，则 [`zip()`](https://docs.python.org/zh-cn/3/library/functions.html#zip) 只须使用长度不相等的输入即可。 如果那些值很重要，则应改用 [`itertools.zip_longest()`](https://docs.python.org/zh-cn/3/library/itertools.html#itertools.zip_longest)。语法如下：itertools.**zip_longest**(**iterables*, *fillvalue=None*)

`zip_longest()` 会根据最长的可迭代对象来进行迭代，并根据 *fillvalue* 来填充缺失值。相当于

```python
def zip_longest(*args, fillvalue=None):
    #zip_longest('ABCD','xy',fillvalue='-') --> Ax By C- D-
   	iterators = [iter(it) for it in args]
    num_active = len(iterators)
    if not num_active:
        return
    while True:
        values = []
        for i, it in enumerate(iterators):
            try:
                value = next(it)
            except StopIteration:
                num_active -=1
                if not num_active
                	return
                iterators[i] = repeat(fillvalue)
                value = fillvalue
            values.append(value)
        yield tuple(values)  
```



  