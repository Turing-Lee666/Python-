# Python装饰器常见考题

> 如果想要给一个类扩充功能的话，可以通过组合或者继承这种方式；而装饰器模式的话，我们通过实现一个装饰器，在不修改原来函数的情况下，可以给他增添一些功能。这个其实就是装饰器的一个非常重要的作用。

## 什么是装饰器？

### Decorator

* Python中一切皆对象，函数也可以当作参数传递
* 装饰器是接受函数作为参数，添加功能后返回一个新函数的函数(类)
* Python中通过@使用装饰器

```python
import time


# 定义一个装饰器  它的功能就是记录了被装饰函数它所执行的时间
def log_time(func):  # 接受一个函数(被装饰函数)作为参数
    def _log(*args, **kwargs):  # 定义一个里层的函数
        beg = time.time()  # 记录开始的时间
        res = func(*args, **kwargs)  # 调用被装饰的函数
        print('uses time: {}'.format(time.time()-beg))  # 把使用的时间打出来
        return res  # 把被装饰函数func的执行结果返回
    return _log  # 在装饰器里面返回新定义的函数


@log_time  # @是装饰器的语法糖
def mysleep():  # 编写一个函数来使用装饰器
    time.sleep(1)
    
    
mysleep()    
```

```python
@log_time  # @是装饰器的语法糖
等价于
newsleep = log_time(mysleep)  # newsleep = _log
newsleep()  # _log()
```

执行结果：

```python
uses time: 1.000819206237793
```



## 如何使用类编写装饰器？

使用到了类里面的一个魔术方法：`__call__`

```python
import time


class LogTime:  # 定义一个名为LogTime的装饰器
    def __call__(self, func):  # 接受一个函数来作为参数
        def _log(*args, **kwargs):
            beg = time.time()  # 记录一下开始的时间
            res = func(*args, **kwargs)  # 调用被装饰函数
            print('use time: {}'.format(time.time()-beg))# 把时间记录下来
            return res  # 把被装饰函数的调用结果返回
        return _log  # 返回内层函数
    

@LogTime()  # LogTime() ---> 类名+()：初始化一个装饰器类的实例
def mysleep():
    time.sleep(1)

    
mysleep()

```

```python
@LogTime()  # LogTime() ---> 类名+()：初始化一个装饰器类的实例
等价于
newsleep = LogTime().__call__(mysleep)  # newsleep = _log
newsleep()  # _log()
```

执行结果：

```python
use time: 1.0002562999725342
```

---

## 如何给装饰器它本身来增加参数？

### 使用类装饰器比较方便实现装饰器参数

```python
import time


class LogTime:  # 定义一个名为LogTime的装饰器
    def __init__(self, use_int=False):
        self.use_int = use_int
        
        
    def __call__(self, func):  # 接受一个函数来作为参数
        def _log(*args, **kwargs):
            beg = time.time()  # 记录一下开始的时间
            res = func(*args, **kwargs)  # 调用被装饰函数
            if self.use_int:  #     
                print('use time: {}'.format(
                    int(time.time()-beg))
                )# 把时间记录下来
            else:
                print('use time: {}'.format(
                    time.time()-beg)
                )# 把时间记录下来     
                      
            return res  # 把被装饰函数的调用结果返回
        return _log  # 返回内层函数
    

@LogTime(True)  # LogTime() ---> 类名+()：初始化一个装饰器类的实例
def mysleep():
    time.sleep(1)

    
mysleep()

```

```python
@LogTime(True)  # LogTime() ---> 类名+()：初始化一个装饰器类的实例
等价于
newsleep = LogTime(True).__call__(mysleep)  # newsleep = _log
newsleep()  # _log()
```



执行结果：

```python
use time: 1
```

这样我们就是通过装饰器类的这个`__init__`来去给它传了一个参数

装饰器说白了就是**装饰器函数log_time**或者**装饰器类`LogTime`所创建的实例`LogTime()`的`__call__`方法**接收一个**函数(被装饰函数mysleep)**作为参数然后返回一个新函数(_log)的函数或者是类。

