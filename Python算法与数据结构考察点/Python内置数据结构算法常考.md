# Python内置数据结构算法常考

## 1. 你使用过哪些常用内置算法和数据结构？

常用内置数据结构和算法

| **数据结构/算法** | **语言内置**                     | **内置库**                                         |
| ----------------- | -------------------------------- | -------------------------------------------------- |
| 线性结构          | list(列表)/tuple(元组)           | array(数组，不常用)/ collections.namedtuple        |
| 链式结构          |                                  | collections.deque(双端队列)                        |
| 字典结构          | dict(字典)                       | collections.Counter(计数器)/ OrderedDict(有序字典) |
| 集合结构          | set(集合)/ frozenset(不可变集合) |                                                    |
| 排序算法          | sorted                           |                                                    |
| 二分算法          |                                  | bisect模块                                         |
| 堆算法            |                                  | heapq模块                                          |
| 缓存算法          |                                  | functools.lru_cache(Least Recent Used, python3)    |

## 2. 有用过collections模块吗？

collections模块提供了一些内置数据结构的拓展

| namedtuple() | namedtuple让tuple属性可读                                    |
| ------------ | ------------------------------------------------------------ |
| deque        | deque可以方便地实现queue(队列)/stack(栈)                     |
| Counter      | 字典的一个子类，需要计数器的地方使用Counter                  |
| OrderedDict  | 也是Dict的一个子类，OrderedDict的Key顺序是第一次插入的顺序，例题：使用OrderedDict实现LRUCache |
| defaultdict  | 也是Dict的一个子类。对于缺失的值来说，可以给他一个默认值。也可以用它来非常方便地实现这个计数器的功能 |

```python
import collections

# namedtuple让tuple属性可读
Point = collections.namedtuple('Point', 'x, y')
p = Point(1, 2)
p.x  # 1
p.y  # 2
p[0] == p.x  # True
p[1] == p.y  # True


# 双端队列的用法
de = collections.deque()
de.append(1)  # 往右边append值
de.appendleft(0)  # 往左边append值
de  # deque([0, 1])
de.pop()  # 从右端pop值
de.popleft()  # 从左端pop值


c = collections.Counter()  # 定义一个Counter的对象
c = collections.Counter('abcab')# 统计字符的数目
c  # Counter({'a': 2, 'b': 2, 'c': 1})
c['a']  # 2
c.most_common()  # [('a',2), ('b',2), ('c',1)]  # 从大到小获取字符重复的数量


od = collections.OrderedDict()
od['c'] = 'c'
od['a'] = 'a'
od['b'] = 'b'
list(od.keys())# 按照key进入的顺序应该是['c','a','b']


dd = collections.defaultdict(int)
dd['a']  # 0  'a'这个key他虽然不存在，但依然会返回一个0而不是抛出一个Key Error这种异常(带有默认值的字典)
dd['b'] += 1  # 'b'这个key不存在，会返回一个0，然后dd['b'] = 0+1，所以返回1
dd  # defaultdict(int, {'a': 0, 'b': 1})
```

## 3. Python dict 底层结构

dict底层使用的哈希表

* 为了支持快速查找使用了哈希表作为底层结构
* 哈希表平均查找时间复杂度O(1)
* Cpython 解释器使用二次探查解决哈希冲突问题

**哈希冲突和扩容是常考题**

* 如何解决哈希冲突：

  1. 链接法：
  2. 探查法：分为线性探查，二次探查等

* 哈希表如何扩容：

    哈希表其实就是一个数组，当这个数组我们不断去添加元素的时候，如何去触发这个扩容操作。

## 4. Python list/ tuple 区别

list vs tuple

* 都是线性结构，支持下标访问

* list是可变对象，tuple保存的引用不可变

  ```python
  t = ([1], 2, 3)
  # 改他的第二个值
  t[2] = 3  # 会报错，tuple不支持元素赋值('tuple' object does not support item assignment)
  # 改他的第零个元素
  t[0].append(1)
  t  # ([1, 1], 2, 3)
  ```

  **注意**：保存的引用不可变指的是你没法替换掉这个对象，但是如果对于那个本身是一个可变对象，是可以修改这个引用指向的可变对象的

* list没法作为字典的key, tuple可以(可变对象不可 hash)

## 5. 什么是 LRUCache?

**Least-Recently-Used 替换掉最近最少使用的对象**

* 缓存剔除策略，当缓存空间不够用的时候需要一种方式剔除key
* 常见的有LRU, LFU等
* LRU通过使用一个循环双端队列不断把最新访问的key放到表头实现

## 6. 如何实现LRUCache?

**字典用来缓存，循环双端链表用来记录访问顺序**

* 利用Python内置的dict + collections.OrderedDict实现

* dict用来当作k/v键值对的缓存

* OrderedDict用来实现更新最近访问的key

  ```python
  from collections import OrderedDict
  
  
  class LRUCache:
      
      def __int__(self, capacity=128):  # 传递参数capacity=128表示容量，也就是说我们最多只允许128个key/ value存在
          '''初始化了两个结构
          '''
          self.od = OrderedDict()
          self.capacity = capacity
          
      def get(self, key):  
          '''每次访问更新最新使用的key
          '''
          if key in self.od:
              val = self.od[key]
              self.od.move_to_end(key)  # 把这个key放到最尾部，最右边  也就实现了把最近访问的放到了最右边
              return val
          else:  # 这个key不存在
              reutrn -1
              
      def put(self, key, value):
          '''更新 k/v
          '''
          if key in self.od:
              del self.od[key]  # 删除k/v
              self.od[key] = value  # 重新赋值，实际上是把这个key又放到了最后，相当于更新key到表头，表示它是最新访问的 
          else:  # insert
              self.od[key] = value
              # 判断当前容量是不是满了
              if len(self.od) > self.capacity:
                  self.od.popitem(last=False)  # 将最早的key删除
                  
              
      
  ```

  请自己实现LRUCache并编写单元测试



