# 设计模式：结构型模式Python应用面试题

## 常见结构型设计模式

* 装饰器模式(Decorator)：无需子类化拓展对象功能
* 代理模式(Proxy)：把一个对象的操作代理到另一个对象
* 适配器模式(Adapter)：通过一个间接层适配统一接口
* 外观模式(Facade)：简化复杂对象的访问问题
* 享元模式(Flyweight)：通过对象复用(池)改善资源利用，比如连接池
* Model-View-Controller(MVC)：解耦展示逻辑和业务逻辑

## 代理模式

### 什么是代理模式(Proxy)

* 把一个对象的操作代理到另一个对象
* 这里又要提到我们之前实现的Stack/Queue，把操作代理到deque
* 通常使用has-a组合关系

```python
form collections import deque

class Stack(object):  # 使用组合的例子  组合是使用其他的类实例作为自己的一个属性(Has-a关系)
    
    def __init__(self):
        self._deque = deque()  # has a deque()
        
    
    def push(self, value):
        return self._deque.append(value)
    
    
    def pop(self):
        return self._deque.pop()
    
    
    def empty(self):
        return len(self._deque) == 0
```

## 适配器模式

### 什么是适配器模式(Adapter)

* 把不同对象的接口适配到同一个接口
* 想象一个多功能充电头，可以给不同的电器充电，充当了适配器
* 当我们需要给不同的对象统一接口的时候可以使用适配器模式

```python
class Adapter:
    def __init__(self, obj, **adapted_methods):  # **表示接受的参数是多值字典型
        """We set the adapted methods in the object's dict"""
        self.obj = obj
        self.__dict__.update(adapted_methods)  # 使用类的实例对象调用__dict__，会输出由类中所有实例属性组成的字典  Python字典update()函数把字典参数dict2的key/value(键/值)对更新到字典dict里。
        
    
    def __getattr__(self, attr):
        """All non-adapted calls are passed to the object"""
        return getattr(self.obj, attr)
        
        
objects = []
dog = Dog()
objects.append(Adapter(dog, make_noise=dog.bark))        
```

