# 设计模式：创建型模式Python应用面试题

## 创建型、结构型、行为型

## 创建型模式常考题

### 常见创建型设计模式

* 工厂模式(Factory)：解决对象创建问题
* 构造模式(Builder)：控制复杂对象的创建
* 原型模式(Prototype)：通过原型的克隆创建新的实例
* 单例(Brog/Singleton)：一个类只能创建同一个对象
* 对象池模式(Pool)：预先分配同一类型的一组实例
* 惰性计算模式(Lazy Evaluation)：延迟计算(python的property)

## 工厂模式

### 什么是工厂模式(Factory)

* 解决对象创建问题
* 解耦对象的创建和使用
* 包括工厂方法和抽象工厂

**我们可以把它理解为是创建对象的一个工厂**

一个工厂方法的例子

```python
class DogToy:
    def speak(self):
        print("wang wang")
        
        
class CatToy:
    def speak(self):
        print("miao miao")
        
        
def toy_factory(toy_type):  # 我们可以把它理解为是创建对象的一个工厂
    """toy_factory就是一个工厂方法"""
    if toy_type == "dog":
        return DogToy()
    elif toy_type == "cat":
        return CatToy()
```

学习设计模式的一个有效的方式是自己尝试写个示例代码来演示它。

---

## 构造模式

### 什么是构造模式(Builder)

* 用来控制复杂对象的构造
* 创建和表示分离。比如你要买电脑，工厂模式直接给你需要的电脑
* 但是构造模式允许你自己定义电脑的配置，组装完成后给你

---

## 原型模式

### 什么是原型模式(Prototype)

* 通过克隆原型来创建新的实例
* 可以使用相同的原型，通过修改部分属性来创建新的示例
* 用途：对于一些创建实例开销比较高的地方可以用原型模式

---

## 单例模式(重要，经常让人手写)

### 单例模式的实现有多种方式

* 单例模式：一个类创建出来的对象都是同一个
* Python的模块其实就是单例的，只会导入一次
* 使用共享同一个实例的方式来创建单例模式

```python
# 单例模式
class Singleton:
    def __new__(cls, *args, **kwargs):  # __new__这一个魔术方法是用来创建实例的  *args, **kwargs表示接收多个可变参数
        if not hasattr(cls, '_instance'):  # 如果没有_instance这个属性的话  hasattr--->有...属性吗？  _instance表示所有的共享实例
            _instance = super().__new__(cls, *args, **kwargs)  # 就创建一个  使用父类的__new__方法
            cls._instance = _instance  # 然后我们把它赋值给当前的类
        return cls._instance  # 这样的话，实际上我们每次创建一个新的实例的时候它都会使用这一个_instance来做为一个共享的实例。这样的话，我们就实现了这个单例的模式
    
 
class MyClass(Singleton):  # 定义类MyClass继承一下单例类Singleton
    pass

# 创建两个实例
c1 = MyClass()
c2 = MyClass()
# 用is判断它们是不是同一个实例  id一样说明是同一个实例
print(id(c1))
print(id(c1))
print(c1 is c2)  # 判断是不是同一个对象
# assert c1 is c2
```

执行结果：

```python
49616112
49616112
True
```

验证了确实是单例的。c1和c2是同一个实例。



