# Python语言特性

## 1. Python的函数参数传递

   看两个如下的例子，分析运行结果：

   代码一：

   ```python
   a = 1
   def fun(a):
       a = 2  
   fun(a)
   print(a)  # 1  
    

   ```

   代码二：

   ```python
   a = []
   def fun(a):
       a.append(1)
   fun(a)
   print(a)  # [1]

   ```

   ==所有的变量都可以理解是内存中一个对象的“引用”==，或者，也可以看似c中void*的感觉。

   这里记住的是类型是属于对象的，而不是变量。而对象有两种，“可更改”（mutable）与“不可更改”（immutable）对象。在python中，strings, tuples 和numbers是不可更改的对象，而list, dict等则是可以修改的对象。（这就是这个问题的重点）

   <u>当一个引用传递给函数的时候，函数自动复制一份引用，这个函数里的引用和外边的引用没有半毛关系了。</u>所以第一个例子里函数把引用指向了一个不可变对象，当函数返回的时候，外面的引用没半毛感觉。而第二个例子就不一样了，函数内的引用指向的是可变对象，对它的操作和定位了指针地址一样，在内存里进行修改。

## 2. Python中的元类（metaclass）

   ==元类就是用来创建类的“东西”==。你创建类就是为了创建类的实例对象，但是我们已经学习到了==Python中的类也是对象==。好吧，元类就是用来创建这些类（对象）的，元类就是类的类

   这个非常的不常用，详情请看：《深刻理解Python中的元类(metaclass)》

## 3. @staticmethod和@classmethod

   Python其实有3个方法，即静态方法（staticmethod），类方法（classmethod）和实例方法，如下：

   ```python
   class A(object):
       def foo(self, x):
           print("executing foo(%s,%s)"%(self,x))
           
           
       @classmethod
       def class_foo(cls, x):
           print("executing class_foo(%s,%s)"%(cls,x))
           
          
       @staticmethod
       def static_foo(x):
           print("executing static_foo(%s)"%x)
           
           
   a = A()
   
   ```

   ==这里先理解下函数参数里面的self和cls。这个self和cls是对实例或者类的绑定。==对于实例方法，我们知道在类里每次定义方法的时候都需要绑定这个实例，就是foo(self, x)，为什么要这么做呢？因为实例方法的调用离不开实例，我们需要把实例自己传给函数，调用的时候是这样的a.foo(x)(其实是foo(a, x))。类方法一样，只不过它传递的是类而不是实例，A.class_foo(x)。**注意**这里的self和cls可以替换为别的参数，但是python的约定是这俩，还是不要改的好。

   对于静态方法其实和普通的方法一样，不需要对谁进行绑定，唯一的区别是调用的时候需要使用a.static_foo(x)或者A.static_foo(x)来调用。

   

   

| \       | 实例方法 | 类方法         | 静态方法        |
| ------- | -------- | -------------- | --------------- |
| a = A() | a.foo(x) | a.class_foo(x) | a.static_foo(x) |
| A       | 不可用   | A.class_foo(x) | A.static_foo(x) |

## 4. 类变量和实例变量

   ```python
   class Person:
       name = "aaa"
       
       
   p1 = Person()
   p2 = Person()
   p1.name = "bbb"
   print(p1.name)  # bbb
   print(p2.name)  # aaa
   print(Person.name)  # aaa
   
   ```

   类变量就是供类使用的变量，实例变量就是供实例使用的。

   这里p1.name = "bbb"是==实例调用了类变量==，这其实和上面第一个问题一样，就是函数传参的问题，p1.name一开始指向的类变量name = "aaa"，但是==在实例的作用域里把类变量的引用改变了，就变成了一个实例变量==，self.name不再引用Person的类变量name了。

   可以看看下面的例子：

   ```python
   class Person:
       name = []
   
       
   p1 = Person()
   p2 = Person()
   p1.name.append(1)
   print(p1.name)  # [1]
   print(p2.name)  # [1]
   print(Person.name)  # [1]
   ```

   

## 5. Python自省

   这个也是python彪悍的特性。

   自省就是面向对象的语言所写的程序在运行时，所能知道对象的类型。简单一句就是运行时能够获得对象的类型。比如type(), dir(), getattr(), hasattr(), isinstance().

## 6. 字典推导式

  可能你见过列表推导式，却没有见过字典推导式，在2.7中才加入的：

  d = {key: value for (key, value) in iterable}

## 7. Python中单下划线和双下划线

  ```python
  class MyClass():
      def __init__(self):
          self.__superprivate = "Hello"
          self._semiprivate = ", world!"
          
          
  mc = MyClass()
  print(mc.__superprivate)
  Traceback(most recent call last):
  File"<stdin>", line 1, in<module>
  AttributeError: myClass instance has no attribute '__superprivate'
  
  print(mc._MyClass__superprivate)  # Hello
  print(mc._semiprivate)  # ,world!
  print(mc.__dict__)  # {'_MyClass__superprivate':'Hello', '_semiprivate':'world!'}
  
  ```

  `__foo__`: 一种约定，Python内部的名字，用来区别其他用户自定义的命名，以防冲突。

  `_foo`: 一种约定，用来指定变量私有。程序员用来指定私有变量的一种方式。

  `__foo`: 这个有真正的意义：解释器用_classname__foo来代替这个名字，以区别和其他类相同的命名。

  详情见：http://zhihu.com/question/19754941

## 8. 字符串格式化：% 和 .format

  一、%用法（==%的用法是写多少个，后面就要传多少个==）

  例1：

  ```python
  # 'my name is xiaoming and I am 18 years old'
  "my name is %s and I am %d years old" %("xiaoming", 18)
  ```

  例2：

  ```python
  # 'format test abc abc abc'
  "format test %s %s %s" %('abc','abc','abc')
  ```

  二、format用法:

  ​		相对基本格式化输出采用‘%’的方法，format()功能更强大，该函数把字符串当成一个模板，通过传入的参数进行		格式化，并且使用大括号‘{}’作为特殊字符代替‘%’

  ​		位置匹配

  　　		（1）不带编号，即“{}”

  　　		（2）带数字编号，可调换顺序，即“{1}”、“{2}”

  　　		（3）带关键字，即“{a}”、“{tom}”

  例3：

  ```python
  # 设置指定位置，按默认顺序
  "{} {}".format("hello","world")  # 'hello world'
  ```

  例4：

  ```python
  # 设置指定位置
  "{0} {1}".format("hello", "world")  # 'hello world'
  ```

  例5：

  ```python
  # 设置指定位置
  "{1} {0} {1}".format("hello", "world")  # 'world hello world'
  ```

  例6：

  ```python
  # 一个num变量可以接受不限个参数（可以例2对比）
  "format test {num} {num} {num}".format(num="abc")  # 'format test abc abc abc'
  ```

  

  

  `.format`在许多方面看起来更便利。对于%最烦人的是它无法同时传递一个变量和元组。你可能会想下面的代码不会有什么问题：

  ```python
  "hi there %s" % name
  ```

  但是，如果name恰好是（1，2，3），它将会抛出一个TypeError异常。为了保证它总是正确的，你必须这样做：

  ```python
  "hi there %s" %(name,)  # 提供一个单元素的元组而不是一个参数
  ```

## 9. 迭代器和生成器

  在Python中，这种一边循环一边计算的机制，称为生成器：generator。

  可以调用next()函数并不断返回下一个值的对象称为迭代器：Iterator。

  这个是stackoverflow里python排名第一的问题，值得一看：

  http://stackoverflow.com/questions/231767/what-does-the-yield-keyword-do-in-python

## 10. `*args and **kwargs`

  用`*args`和`**kwargs`只是为了方便并没有强制使用它们。

  当你不确定你的函数里将要传递多少参数时你可以用`*args`。例如，它可以传递任意数量的参数：

  ```python
  def print_everything(*args):
      for count, thing in enumerate(args):  # 遍历args里的每个元素，并返回该元素的下标和该元素
          print('{0}.{1}'.format(count, thing))  # format()格式化字符串
          
          
  print_everything('apple', 'banana', 'cabbage')
  # 0.apple
  # 1.banana
  # 2.cabbage
  ```

  相似的，`**kwargs`允许你使用没有事先定义的参数名：

  ```python
  def table_things(**kwargs):
      for name, value in kwargs.items():  # 遍历字典的项目
          print('{0} = {1}'.format(name, value))  # 格式化字符串
          
          
  table_things(apple='fruit', cabbage='vegetable')
  # apple = fruit
  # cabbage = vegetable       
  ```

  你也可以混着用。命名参数首先获得参数值然后所有的其他参数都传递给`*args`和`**kwargs`。命名参数在列表的最前端。例如：

  `def table_things(titlestring, **kwargs)`

  `*args`和`**kwargs`可以同时在函数的定义中，但是`*args`必须在`**kwargs`前面。

   当调用函数时你也可以用`*`和`**`语法。例如：  

  ```python
  def print_three_things(a, b, c):
      print('a = {0}, b = {1}, c = {2}'.format(a, b, c))
  
  
  mylist = ['aardvard', 'baboon', 'cat']
  print_three_things(*mylist)  # a = aardvard, b = baboon, c = cat
  ```

  就像你看到的一样，它可以传递列表（或者元组）的每一项并把它们解包。注意必须与它们在函数里的参数相吻合。当然，你也可以在函数定义或者函数函数调用时用`*`。    
  http://stackoverflow.com/questions/3394835/args-and-kwargs​    

## 11. 面向切面编程AOP和装饰器

  装饰器是一个很著名的设计模式，经常被用于有切面需求的场景，较为经典的有插入日志、性能测试、事务处理等。装饰器是解决这类问题的绝佳设计，有了装饰器，我们就可以抽离出大量函数中与函数功能本身无关的雷同代码并继续重用。概括的讲，**装饰器的作用就是为已经存在对象添加额外的功能**。

这个问题比较大，推荐：http://stackoverflow.com/questions/739654/how-can-i-make-a-chain-of-function-decorators-in-python

中文：http://taizilongxu.gitbooks.io/stackoverflow-about-python/content/3/README.html



## 12. 鸭子类型

 “当看到一只鸟走起来像鸭子、游泳起来像鸭子、叫起来也像鸭子，那么这只鸟就可以被称为鸭子。”

我们并不关心对象是什么类型，到底是不是鸭子，只关心行为。

比如在python中，有很多file-like的东西，比如StringIO, GzipFile, socket。他们有很多相同的方法，我们把它们当作文件使用。

又比如list.extend()方法中，我们并不关心它的参数是不是list, 只要它是可迭代的，所以它的参数可以是list/tuple/dict/字符串/生成器等。

鸭子类型在动态语言中经常使用，非常灵活，使得python不像java那样专门去弄一大堆的设计模式。

## 13. Python中重载

引自知乎：http://www.zhihu.com/question/20053359

函数重载主要是为了解决两个问题。

1.  可变参数类型。
2.  可变参数个数。

另外，一个基本的设计原则是，仅仅当两个函数除了参数类型和参数个数不同之外，其功能是完全相同的，此时才使用函数重载，如果两个函数的功能其实不同，那么不应当使用重载，而应当使用一个名字不同的函数。

好吧，那么对于情况1，函数功能相同，但是参数类型不同，python如何处理？答案是根本不需要处理，因为Python可以接受任何类型的参数，如果函数的功能相同，那么不同的参数类型在Python中很可能是相同的代码，没有必要做成两个不同函数。

那么对于情况2，函数功能相同，但参数个数不同，Python如何处理？大家知道，答案就是缺省参数。对那些缺少的参数设定为缺省参数即可解决问题。因为你假设函数功能相同，那么那些缺少的参数终归是需要用的。

好了，鉴于情况1跟情况2都有了解决方案，python自然就不需要函数重载了。

## 14 新式类和旧式类

这篇文章很好的介绍了新式类的特性：http://www.cnblogs.com/btchenguang/archive/2012/09/17/2689146.html

新式类很早在2.2就出现了，所以旧式类完全是兼容的问题，Python3里的类全部都是新式类，这里有一个MRO问题可以了解下（新式类是广度优先，旧式类是深度优先），<Python核心编程>里讲的也很多。

## 15 `__new__`和`__init__`的区别

这个`__new__`确实很少见到，先做了解吧。

1. `__new__`是一个静态方法，而`__init__`是一个实例方法。
2. `__new__`方法会返回一个创建的实例，而`__init__`什么都不返回。
3. 只有在`__new__`返回一个cls的实例时后面的`__init__`才能被调用。
4. 当创建一个新实例时调用`__new__`，初始化一个实例时用`__init__`。

PS: `__metaclass__`是创建类时起作用。所以我们可以分别使用`__metaclass__`，`__new__`和`__init__`来分别在类创建，实例创建和实例初始化的时候做一些小手脚。

## 16 单例模式

这个绝对常考啊。绝对要记住1~2个方法，当时面试官是让手写的。

1. 使用`__new__`方法

   ```python
   
   ```
   
2. 共享属性

   ```python
   
   ```

3. 装饰器版本

    ```python
      
    ```

4. import 方法

   ```python
   
   ```


## 17 Python中的作用域

Python中，一个变量的作用域总是由在代码中被赋值的地方所决定的。

当Python遇到一个变量的话他会按照这样的顺序进行搜索：

**本地(局部)作用域（Local）**--> **当前作用域被嵌入的本地(局部)作用域(Enclosing locals)** --> 全局/模块作用域(Global) --> 内置作用域(Built-in)

## 18 GIL线程全局锁

线程全局锁(Global Interpreter Lock)，即Python为了保证线程安全而采取的独立线程运行的限制，说白了就是一个核只能在同一时间运行一个线程。

解决办法就是多进程和下面的协程(协程也只是单CPU，但是能减小切换代价提升性能)。

## 19 协程

简单点说协程是进程和线程的升级版，进程和线程都面临着内核态和用户态的切换问题而耗费许多切换时间，而协程就是用户自己控制切换的时机，不再需要陷入系统的内核态。

Python里最常见的yield就是协程的思想！可以查看第九个问题。

## 20 闭包

闭包(closure)是函数式编程的重要的语法结构。闭包也是一种组织代码的结构，它同样提高了代码的可重复使用性。

当一个内嵌函数引用其外部作用域的变量，我们就会得到一个闭包。总结一下，创建一个闭包必须满足以下几点：

1. 必须有一个内嵌函数
2. 内嵌函数必须引用外部函数中的变量
3. 外部函数的返回值必须是内嵌函数

感觉闭包还是有难度的，几句话是说不明白的，还是查查相关资料。

重点是函数运行后并不会被销毁，就像16题的instance字典一样，当函数运行完后，instance并不会销毁，而是继续留在内存空间里。这个功能类似类里的类变量，只不过迁移到了函数上。

闭包就像个空心球一样，你知道外面和里面，但你不知道中间是什么样。





























