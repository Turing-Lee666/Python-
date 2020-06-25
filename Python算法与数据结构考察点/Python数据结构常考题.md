# 常考题型

## Python web后端常考数据结构

* 常见的数据结构链表、队列、栈、二叉树、堆
* 使用内置结构实现高级数据结构，比如内置的list\deque实现栈
* Leetcode或者《剑指offer》上的常见题

# 常考数据结构之链表

链表有单链表、双链表、循环双端链表

* 如何使用Python来表示链表结构
* 实现链表常见操作，比如插入节点，反转链表，合并多个链表等
* Leetcode练习常见链表题目

![链表](图片\链表.png)

Leetcode 206. 反转链表

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def reverseList(self, head: ListNode) -> ListNode:
        pre = None
        cur = head
        while cur:  # 当cur不为空值的时候
            nextnode = cur.next
            cur.next = pre  # 往回指
            pre = cur  # pre指针前移
            cur = nextnode
        return pre
```

反转链表，合并两个链表，求两个链表的公共节点都是常考题

# 常考数据结构之队列

队列（queue）是先进先出结构

* 如何使用Python实现队列？

* 实现队列的append（进入队尾）和pop操作，如何做到先进先出

* 使用Python的list或者collections.deque实现队列

  ```python
  # 实现队列(先进先出的结构)。使用deque
  from collections import deque  # deque 双端队列

  class Queue:  # 队列
      def __init__(self):
          self.items = deque()
          
      def append(self, val):  # 往队列里面塞一个值  往右边追加一个元素
          return self.items.append(val)
      def pop(self)  # 队列的pop是先进先出。往右边添加(push)元素，需要从左边去取
          return self.items.popleft()
      
      def empty(self):
          return len(self.items) == 0
      
  def test_queue():
      q = Queue()
      q.append(0)
      q.append(1)
      q.append(2)
      print(q.pop())  
      print(q.pop())
      print(q.pop())
      
  test_queue()  # 0 1 2
      

  ```


Collections.deque 双端队列可以很高效地往两边追加或者删除元素。但是对于列表的话可以很方便的往后面去追加元素，但比如说往第一个位置去插入一个元素的话，可能就需要去移动后面所有的元素，所以可能在这个地方效率就不如deque。

# 常考数据结构之栈

栈(stack)是后进先出结构

* 如何使用Python实现栈？

* 实现栈的push和pop操作，如何做到后进先出（想象往桶里放盘子，取盘子）

* 同样可以用Python list或者collections.deque实现栈

  ```python
  # 借助内置的数据结构非常容易实现一个栈(Stack)，后入先出
  from collections import deque
  
  class Stack(object):
      def __init__(self):
          self.deque = deque()  # 或者用list
          
      def push(self, value):
          self.deque.append(value)  # 往右边添加一个元素
      def pop(self):
          return self.deque.pop()  # 获取最右边一个元素，并在队列中删除
  ```

常考问题： 1. 如何用两个栈实现队列(剑指offer和Leetcode都有这个题)    2. 如何使用栈来实现获取最小值的栈MinStack(剑指offer和Leetcode都有这个题)

# 常考数据结构之字典与集合

## Python dict/set 底层都是哈希表

* 哈希表的实现原理，底层其实就是一个数组
* 根据哈希函数快速定位一个元素，平均查找O(1)，非常快
* 不断加入元素会引起哈希表重新开辟空间，拷贝之前元素到新数组

> 哈希冲突和哈希扩容是两个非常重要的概念。一定是要能回答上来这种问题。

## 哈希表如何解决冲突

### 链接法和开放寻址法(探查法)

* 元素key冲突之后使用一个链表填充相同key的元素
* 开放寻址法是冲突之后根据一种方式（二次探查）寻找下一个可用的槽
* cpython使用的二次探查

# 常考数据结构之二叉树

先序、中序、后序遍历

* 先(根)序：先处理根，之后是左子树，然后是右子树
* 中(根)序：先处理左子树，然后是根，然后是右子树
* 后(根)序：先处理左子树，然后是右子树，最后是根

## 树的遍历方式

### 先序遍历，其实很简单，递归代码里先处理根就好了

```python
class BinTreeNode(object):
    """定义树的节点
    三个属性：data用来保存数值, left用来保存左子树节点, right用来保存右子树节点
    left=None表示没有左子树，同理，right=None表示没有右子树
    left和right都为None的话，就说明是一个叶子节点
    """
    def __init__(self, data, left=None, right=None):
        self.data, self.left, self.right=data, left, right
        
class BinTree(object):
    """"""
    def __init__(self, root=None):
        self.root = root
        
    def preorder_trav(self, subtree):
        """先(根)序遍历
        subtree应该就是BinTreeNode类创建的节点对象
        """
        if subtree is not None:  # 只要subtree不为None
            print(subtree.data)  # 递归函数里先处理根  打印根里面的值
            self.preorder_trav(subtree.left)  # 递归处理左子树  递归调用自己
            self.preorder_trav(subtree.right)  # 递归处理右子树  
```

> 二叉树本身就是一个递归定义。因为从任何一个非叶节点来看的话，它其实还是一棵二叉树

### 中序遍历

中序遍历，调整下把print(subtree.data)放中间就好啦

```python
def inorder_trav(self, subtree):
    """中序遍历"""
    if subtree is not None:
        self.inorder_trav(subtree.left)
        print(subtree.data)  # 递归函数里中间处理根  打印当前节点的data值
        self.inorder_trav(subtree.right)
```

# 常考数据结构之堆

## 堆其实是完全二叉树，有最大堆和最小堆

* 最大堆：对于每个非叶子节点V，V的值都比它的两个孩子大
* 最小堆：对于每个非叶子节点V，它两个孩子的值都比它大
* 最大堆支持每次pop操作获取最大的元素，最小堆获取最小元素
* 常见问题：用堆来完成 topk 问题，从海量数字中寻找最大的k个

```python
import heapq


class TopK:
    """获取大量元素 topk大个元素，固定内存
    思路：
    1. 先放入元素前 k 个建立一个最小堆
    2. 迭代剩余元素：
        如果当前元素小于堆顶元素，跳过该元素（肯定不是前 k 大）
        否则替换堆顶元素为当前元素，并重新调整堆
    """
    
    def __init__(self, iterable, k):
        self.minheap = []  # 初始化一个小根堆
        self.capacity = k  # 小根堆的容量
        self.iterable = iterable  # 一个可迭代的对象  列表  放所有元素
        
    def push(self, val):  
        """往堆里添元素
        """
        if len(self.minheap) >= self.capacity:  # 如果小根堆的元素个数>=k  超了
            min_val = self.minheap[0]  # 小根堆的0号元素（堆顶元素）就是堆中元素最小值，这里用min_val变量来记录一下
            if val < min_val:  #  如果当前元素小于堆顶元素，跳过该元素（肯定不是前 k 大）  当然你可以直接if val > min_val操作（替换堆顶元素为当前元素，并重新调整堆）
                pass
            else:  # 替换堆顶元素为当前元素，并重新调整堆
                heapq.heapreplace(self.minheap, val)  # 返回并且pop堆顶最小值，推入新的val值并调整堆
        else:
            heapq.heappush(self.minheap, val)  # 前面k个元素直接放入minheap
            
     def get_topk(self):
        for val in self.iterable:  # 遍历列表所有元素
            self.push(val)  # 将每个元素都push一遍
        return self.minheap
    
    
def test():
    import random
    i = list(range(1000))  # 这里可以是一个可迭代元素，节省内存
    random.shuffle(i)
    _ = TopK(i, 10)  # 声明对象，并初始化它的两个属性（传入打乱元素顺序后的列表和小根堆的容量）
    print(_.get_topk())  # [990, 991, 992, 993, 994, 995, 996, 997, 998, 999]

    
test()    
```

