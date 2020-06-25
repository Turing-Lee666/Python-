# 面向对象基础及Python类常考问题

## 什么是面向对象编程？

### Object Oriented Programming(OOP)

* 将对象作为基本单元，把对象抽象成类(Class)，包括成员(就是属性)和方法
* 数据封装、继承、多态(子类可以重写父类的方法)
* Python中使用类来实现。过程式编程(函数)，OOP(类)

## Python中如何创建类？

```python
class Person(object):  # py3 直接 class Person 
    def __init__(self, name, age):  # 方法  两个下划线"__"开头的这种叫魔术方法
        self.name = name  # 属性
        self.age = age  # 属性
        self._sex = O  # _表示该属性不希望被外界访问(私有属性)
        
        
    def print_name(self):  # 方法
        print('ma name is {}'.format(self.name))
        
    
    def __len__(self):  # 魔术方法
        pass
```

> 成员|方法|私有属性|魔术方法

## 组合与继承

### 优先使用组合而非继承

* 组合是使用其他的类实例作为自己的一个属性(Has-a关系)
* 子类继承父类的属性和方法(Is a关系)
* 优先使用组合保持代码简单

之前实现stack/queue就使用到了组合

栈：

```python
from collections import deque

class Stack(object):  # 使用组合的例子
    def __init__(self):
        self._deque = deque()  # has a deque()
        
    def push(self, value):
        return self._deque.append(value)
    
    def pop(self):
        return self._deque.pop()
    
    def empty(self):
        return len(self._deque) == 0
```

继承的例子：

Counter  <-- dict subclass for counting hashable objects  dict子类，用于统计可哈希对象数目

OrderedDict  <-- dict subclass that remembers the order entries were added  记住条目添加顺序的字典子类

defaultdict  <-- dict subclass that calls a factory function to supply missing values  调用工厂函数以提供丢失值的dict子类

## 类变量和实例变量的区别

### 区分类变量和实例变量

* 类变量由所有实例共享
* 实例变量由实例单独享有，不同实例之间不影响
* 当我们需要在一个类的不同实例之间共享变量的时候使用类变量

```python
class Person:  # py3
    
    Country = 'China'  # class属性(变量)
    
    def __init__(self, name):
        self.name = name  # 实例属性(变量)
        
    def print_name(self):
        print(self.name)
        
laowang = Person('laowang')
laoli = Person('laoli')
laowang.print_name()
laoli.print_name()
print(laowang.Country)
print(laoli.Country)
        
```

执行结果

```python
laowang
laoli
China
China
```



## classmethod/staticmethod区别

### classmethod vs staticmethod

* 都可以通过Class.method()的方法使用
* classmethod第一个参数是cls(类本身)，可以引用类变量
* staticmethod使用起来和普通函数一样，只不过放在类里去组织。

classmethod是为了使用类变量；staticmethod是代码组织的需要，完全可以放到类之外

```python
class Person:
    
    Country = 'China'  # 类变量
    
    def __init__(self, name):
        self.name = name
        
    @classmethod
    def print_country(cls):
        print(cls.Country)
        
    @staticmethod
    def join_name(first_name, last_name):
        return last_name + first_name  # last_name姓
```

## 什么是元类？使用场景

### 元类(Meta Class)是创建类的类

* **元类允许我们控制类的生成，比如修改类的属性等**
* 使用type来定义元类
* 元类最常见的一个使用场景就是ORM框架

```python
class X:
    a = 1
等价于
# 调用type类创建X类
X = type('X', (object,), dict(a=1))  # type(name, bases, dict) ---> type这个类它接收三个参数：类的名字， 类的基类，dict(类的属性)  
```

```python
class Base:  # 定义了一个基类
    pass


class Child(Base):
    pass


# 等价定义  注意Base后要加上逗号否则就不是tuple了
SameChild = type('Child', (Base,), {})


# 加上方法
class ChildWithMethod(Base):
    bar = True  # 类属性
    
    def hello(self):  # hello属性
        print('hello')
        
        
def hello(self):
    print('hello')
    
    
# 等价定义
ChildWithMethod = type(
    'ChildWithMethod', (Base,), {'bar': True, 'hello': hello}
)
    
    
# 元类继承自 type    元类(Meta Class)是创建类的类
class LowercaseMeta(type):
    """修改类的属性名称为小写的元类"""
    def __new__(mcs, name, bases, attrs):  # 创建对象时先调用它  重写__new__方法
        lower_attrs = {}
        for k, v in attrs.items():  # 遍历这个类他所有的属性
            if not k.startswith('__'):  # 排除magic method  如果属性以两个下划线开头的话，它是魔术方法                   # 不是魔术方法的话
                lower_attrs[k.lower()] = v  # 把方法名变成lower,就是小写
            else:  # 否则的话
                lower_attrs[k] = v  # 保持原来的不变
        return type.__new__(mcs, name, bases, lower_attrs)  # mcs-->当前的这个类；name-->名称；bases-->基类；lower_attrs-->新的属性
 

class LowercaseClass(metaclass=LowercaseMeta):  # py3  
    BAR = True  # 把属性都弄成大写的名字
    
    def HELLO(self):
        print('hello')
        
        
print(dir(LowercaseClass))  # 你会发现"BAR"和"HELLO"都变成了小写
# 用一个类的实例调用hello方法，我们修改了类定义时候的属性名！！！
LowercaseClass().hello()
```
运行结果
```python
['__class__', '__delattr__', '__dict__', '__dir__', '__doc__', '__eq__', '__format__', '__ge__', '__getattribute__', '__gt__', '__hash__', '__init__', '__init_subclass__', '__le__', '__lt__', '__module__', '__ne__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', '__weakref__', 'bar', 'hello']
hello

```

> `__new__`生成实例
>
> `__init__`初始化实例

## 本章总结

### 面向对象与Python类常考题

* 什么是面向对象编程，Python如何使用类
* 组合vs继承；类变量vs实例变量；classmethod vs staticmethod
* 元类的创建和使用