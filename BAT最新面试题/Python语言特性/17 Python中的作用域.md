# Python3 命名空间和作用域

## 命名空间

命名空间(Nmaespace)是从名称到对象的映射，大部分的命名空间都是通过Python字典实现的。

命名空间提供了在项目中避免名字冲突的一种方法。各个命名空间是独立的，没有任何关系的，所以一个命名空间中不能有重名，但不同的命名空间是可以重名而没有任何影响。

我们举一个计算机系统的例子，一个文件夹（目录）中可以包含多个文件夹，每个文件夹中不能有相同的文件名，但不同文件夹中的文件可以重名。

一般有三种命名空间：全局命名空间，局部命名空间，内置命名空间

* ==内置名称（built-in names），Python语言内置的名称==，比如函数名abs、char和异常名称BaseException、Exception等等。
* ==全局名称（global names），模块中定义的名称，记录了模块的变量==，包括函数、类、其他导入的模块、模块级的变量和常量。
* ==局部名称（local names），函数中定义的名称，记录了函数的变量，包括函数的参数和局部定义的变量。（类中定义的也是）==

![三种命名空间](图片\三种命名空间.png)

命名空间查找顺序：

假设我们要使用变量runoob，则Python的查找顺序为：局部的命名空间 --> 全局命名空间 --> 内置命名空间。

如果找不到runoob，它将放弃查找并引发一个NameError异常：

```python
NameError: name 'runoob' is not defined
```

命名空间的生命周期：

命名空间的生命周期取决于对象的作用域，如果对象执行完成，则该命名空间的生命周期就结束。

因此，==我们无法从外部命名空间访问内部命名空间的对象。==

```python
a = 10  # 全局命名空间

# 函数定义
def my_max(x, y):  # 形参  # 开辟一块内存，放函数进去 
    '''返回两个数中最大的值'''
    m = x if x > y else y  # 解析式 

# 函数调用
my_max(10, 9)  # 实参  函数的调用完成，解释器会把刚开辟的那块内存空间删掉，清理掉
print(m)  # Name 'm' is not defined  # 函数的变量只能在函数内部去使用
```

```python
def f1():
    a = 1
    def f2():
        a = 2
    f2()  # 函数的调用结束完成之后，给f2分配的内存空间就会被销毁。销毁的话就不会再有a = 2这么一个对应关系了
    print(a)  # 1  
    
f1()
```



```python
# var1 是全局名称
var1 = 5
def some_func():
    
    # var2 是局部名称
    var2 = 6
    def some_inner_func():
        
        # var3是内嵌的局部名称
        var3 = 7

```

![相同的对象名称可以存在于多个命名空间中](图片\相同的对象名称可以存在于多个命名空间中.png)

# 作用域

作用域就是一个Python程序可以直接访问命名空间的正文区域。

在一个Python程序中，直接访问一个变量，会从内到外依次访问所有的作用域直到找到，否则会报未定义的错误。

==Python中，程序的变量并不是在哪个位置都可以访问的，访问权限决定于这个变量是在哪里赋值的。==

==变量的作用域决定了在哪一部分程序可以访问哪个特定的变量名称。==Python的作用域一共有4种，分别是：

有四种作用域：

* L(Local)：==最内层==，包含局部变量，比如一个函数/方法内部。

* E(Enclosing)：包含了非局部(non-local)也非全局(non-global)的变量。比如两个嵌套函数，一个函数（或类）A里面又包含了一个函数B，那么对于B中的名称来说A中的作用域就为nonlocal。封闭的（Enclosing）：“封闭式”的作用域规则适应于函数定义函数时，也就是说，在函数体内定义了一个新的函数。

  ![nonlocal](图片\nonlocal.png)

* G(Global)：当前脚本的最外层，比如当前模块的全局变量。
* B(Built-in)：包含了内建的变量/关键字等，最后被搜索。

规则顺序：L --> E --> G --> B。

在局部找不到，便会去局部外的局部找(例如闭包)，再找不到就会去全局找，再者去内置中找。

![规则顺序](图片\规则顺序.png)



```python
g_count = 0  # 全局作用域
def outer():
    o_count = 1  # 闭包函数外的函数中
    def inner():
        i_count = 2  # 局部作用域
```

内置作用域是通过一个名为builtin的标准模块来实现的，但是这个变量名自身并没有放入内置作用域内，所以必须导入这个文件才能够使用它。在Python3.0中，可以使用以下的代码来查看到底预定义了哪些变量：

```python
import builtins
dir(builtins)
```

Python中只有模块(module)、类（class）以及函数（def、lambda）才会引入新的作用域，其他的代码块（如 if/elif/else/、try/except、for/while等）是不会引入新的作用域的，也就是说这些语句内定义的变量，外部也可以访问，如下代码：

```python
if True:
    msg = 'I am from Runoob'
    
msg
# 'I am from Runoob'
```

实例中msg变量定义在if语句块中，但外部还是可以访问的。

如果将msg定义在函数中，则它就是局部变量，外部不能访问：

```python
def test():
    msg_inner = 'I am from Runoob'
    
msg_inner  # name 'msg_inner' is not defined
```

从报错的信息上来看，说明了msg_inner未定义，无法使用，因为它是局部变量，只有在函数内可以使用。

### 全局变量和局部变量

定义在函数内部的变量拥有一个局部作用域，定义在函数外的拥有全局作用域。

局部变量只能在其被声明的函数内部访问，而全局变量可以在整个程序范围内访问。调用函数时，所有在函数内声明的变量名称都将被加入到作用域中。如下实例：

```python
total = 0  # 这是一个全局变量
# 可写函数说明
def sum(arg1, arg2):
    # 返回2个参数的和
    total = arg1 + arg2  # total在这里是局部变量
    print("函数内是局部变量：", total)
    return total

# 调用sum函数
sum(10, 20)
print("函数外是全局变量：", total)

# 函数内是局部变量： 30
# 函数外是全局变量： 0
```

#### global和nonlocal关键字

当内部作用域想修改外部作用域的变量时，就要用到global和nonlocal关键字了。

以下实例修改全局变量num：

```python
num = 1
def fun1():
    global num  # 需要使用global关键字声明
    print(num)  # 1
    num = 123
    print(num)  # 123
    
fun1()
print(num)  # 123
```

```python
def func():
    global a  # 将a变量变成全局有效的变量  global--全局声明关键字  全局声明
    a = 10
    b = 20
    
func()
print(a)  # 10

```

如果要修改嵌套作用域(enclosing作用域，外层非全局作用域)中的变量则需要nonlocal关键字了，如下实例：

```python
def outer():
    num = 10
    def inner():
        nonlocal num  # nonlocal关键字声明
        num = 100
        print(num)  # 100
    inner()
    print(num)  # 100
outer()
```

```python
def f1():
    a = 1
    def f2():
        nonlocal a  
        a = 2
    f2()  
    print(a)  # 2
  
f1()
```



还有另外一种特殊情况，假设下面这段代码被运行：

```python
a = 10
def test():
    a = a + 1
    print(a)
test()
```

以上程序执行，报错信息如下：

```python
# UnboundLocalError: local variable 'a' referenced before assignment
```

错误信息为局部作用域引用错误，因为test函数中的a使用的是局部，未定义，无法修改。

修改a为全局变量，通过函数参数传递，可以正常执行输出结果为：

```python
a = 10
def test(a):
    a = a + 1
    print(a)  # 11
test(a) 

```





