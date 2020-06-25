# 设计模式：行为型模式Python应用面试题

## 常见学习行为型设计模式

* 迭代器模式(Iterator): 通过统一的接口迭代对象
* 观察者模式(Observer): 对象发生改变的时候，观察者执行相应动作
* 策略模式(Strategy): 针对不同规模输入使用不同的策略

## 迭代器模式(Iterator)

* Python内置对迭代器模式的支持
* 比如我们可以用for遍历各种Iterable的数据类型。但在Python里面我们需要注意：什么是Iterator？什么是Iterable？
* Python里可以实现`__next__`和`__iter__`实现迭代器。如果它是一个迭代器(Iterator)的话，我们需要来去实现两个魔术方法：一个是`__next__`；一个是`__iter__`。但是如果是可迭代的对象(Iterable)的话，我们只要实现这个`__iter__`就可以。

接下来演示如何让一个对象变得可迭代

```python
from collections import deque

class Stack(object):  # 使用组合的例子
    
    def __init__(self):
        self._deque= deque()  # has a deque()
        
    def push(self, value):
        return self._deque.append(value)
    
    def pop(self):
        return self._deque.pop()
    
    def empty(self):
        return len(self._deque) == 0

s = Stack()
s.push(1)
s.push(2)
for i in s:  # 想去迭代这个栈
    print(i)
```

执行结果：

```python
TypeError: 'Stack' object is not iterable
```

想定义一个方法来让它支持迭代。可以定义`__iter__`这个魔术方法。

```python
from collections import deque

class Stack(object):  # 使用组合的例子
    
    def __init__(self):
        self._deque= deque()  # has a deque()
        
    def push(self, value):
        return self._deque.append(value)
    
    def pop(self):
        return self._deque.pop()
    
    def empty(self):
        return len(self._deque) == 0
    
    def __iter__(self):
        res = []
        for i in self._deque:
            res.append(i)
        for i in reversed(res):  # reverse: 反转
            yield i

s = Stack()
s.push(1)
s.push(2)
for i in s:  # 想去迭代这个栈
    print(i)


```

执行结果：

```python
2
1
```

我们要去区分什么是可迭代对象(Iterable)，什么是迭代器(Iterator)。在Python里面他们的定义是不一样的。

## 观察者模式

### 观察者模式

* 发布订阅是一种最常用的实现方式
* 发布订阅用于解耦逻辑
* 可以通过回调等方式实现，当发生事件时，调用相应的回调函数

```python

```

## 策略模式

### 策略模式(Strategy)

* 根据不同的输入采用不同的策略
* 比如买东西超过10个打八折，超过20个打七折
* 对外暴露统一的接口，内部采用不同的策略计算

```python

```





