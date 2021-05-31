# 2、单例模式

## 2.1、理解单例设计模式

单例设计模式意图如下：

- 确保类有且只有一个对象被创建
- 为对象提供一个访问的，以使得程序可以全局访问该对象
- 控制共享资源的并行访问

### python 实现经典的单例模式

- 1、只允许 Singleton 类生成一个实例
- 2、如果已经有一个实例了，则重复提供同一个对象

代码如下：

```python
class Singleton(object):
    def __new__(cls):
        if not hasattr(cls, 'instance'):
            cls.instance = super(Singleton, cls).__new__(cls)
        return cls.instance
    
s = Singleton()
print('Object created', s)

s1 = Singleton()
print('Object created', s1)
```

运行结果：

```python
Object created <__main__.Singleton object at 0x000002558D7857B8>
Object created <__main__.Singleton object at 0x000002558D7857B8>
```

上述代码中，通过覆盖 `__new__` 方法来控制对象的创建，对象创建之前先利用 `hasattr()` 方法查看对象是否具有 `instance` (实例)，如果没有，则通过 `__new__` 方法创建对象，否则直接返回 `instance`

## 2.2、单例模式中的 懒汉式实例化

单例模式的用例之一就是懒汉式实例化。例如，导入模块时可能会无意中创建一个对象，但当时根本用不到

- 确保实际需要时才创建对象
- 节约资源并仅在需要的时候创建

代码如下：

```python
class Singleton:
    _instance = None
    def __init__(self):
        if not Singleton._instance:
            print('__init__ method called..')
        else:
            print('Instance already created', self.getInstance())
    @classmethod
    def getInstance(cls):
        if not cls._instance:
            cls._instance = Singleton()
        return cls._instance
s = Singleton()
print('Object created', Singleton.getInstance())
s1 = Singleton()
    
```

运行结果：

```python
__init__ method called..
__init__ method called..
Object created <__main__.Singleton object at 0x000001876CE9DE10>
Instance already created: <__main__.Singleton object at 0x000001876CE9DE10>
```

上述代码中，`s = Singleton()` 调用了 `__init__` 方法，但没有对象被创建。实际的对象创建发生在调用 `Singleton.getInstance()` 的时候。通过这种方式实现懒汉式实例化。

## 2.3、模块级别的单例模式

默认情况下，所有的模块都是单例，这是由 Python 的导入行为所决定的

Python 通过下列方式来工作：

- 检查一个 Python 模块是否已经导入
- 如果已经导入，则返回该模块的对象。如果还没有导入，则导入该模块并实例化。
- 当模块导入时，它就会被初始化。然而，当同一个模块被再次导入的时候，它不会再次初始化，因为单例模式只能有一个对象，所以，它会返回同一个对象

## 2.4、Monostate 单例模式

根据 **Alex Martelli** 的说法，通常程序员需要的是让实例共享相同的状态。

- 开发人员应该关注状态和行为，而不是同一性。
- 由于该概念基于所有对象共享相同状态，因此它也被称为 Monestate (单态) 模式。

代码如下：

```python
class Borg():
    __shared_state = {'1':'2'}
    def __init__(self):
        self.x = 1
        self.__dict__ = self.__shared_state

b = Borg()
b1 = Borg()
b.x = 4
print('Borg Object "b": ', b)
print('Borg Object "b1": ', b1)
print('Object State "b": ', b.__dict__)
print('Object State "b1": ', b1.__dict__)
```

Python 使用 `__dice__` 存储一个类所有对象的状态。上述代码中，故意将类变量 `_shared_state` 赋值给了 `__dict__`，这样，我们将得到两个不同的对象，而对象的状态（`b.__dict__` 和 `b1.__dict__`）却是相同的

运行结果：

```python
Borg Object "b":  <__main__.Borg object at 0x000001DBDC895748>
Borg Object "b1":  <__main__.Borg object at 0x000001DBDC8957F0>
Object State "b":  {'1': '2', 'x': 4}
Object State "b1":  {'1': '2', 'x': 4}
```

除此之外，我们还可以通过修改 `__new__` 方法本身来实现 Borg 模式。

```python
class Borg:
    _shared_state = {}
    def __new__(cls, *args, **kwargs):
        obj = super(Borg, cls).__new__(cls, *args, **kwargs)
        obj.__dict__ = cls._shared_state
        return obj
```





