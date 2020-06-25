# Python数据结构常考题之链表

## 链表

链表涉及到指针操作较为复杂，容易出错，经常用作考题

* 熟悉链表的定义和常见操作

* 常考题：删除一个链表节点

  Leetcode [237. 删除链表中的节点](https://leetcode-cn.com/problems/delete-node-in-a-linked-list/)

  ```python
  # Definition for singly-linked list.
  # class ListNode:
  #     def __init__(self, x):
  #         self.val = x
  #         self.next = None
  
  class Solution:
      def deleteNode(self, node):  # node是要删除的节点
          """
          :type node: ListNode
          :rtype: void Do not return anything, modify node in-place instead.
          """
          nextnode = node.next  # 获取要删除节点的下一个节点
          after_next_node = node.next.next  # 获取要删除节点的下一个节点的下一个节点
          node.val = nextnode.val  # 拿要删除节点的下一个节点的数据值覆盖要删除节点的数据值
          node.next = after_next_node  # 将要删除节点的下一个节点更改为after_next_node
          
  ```

* 常考题：合并两个有序链表

  Leetcode 21. 合并两个有序链表

  ```python
  # Definition for singly-linked list.
  # class ListNode:
  #     def __init__(self, x):
  #         self.val = x
  #         self.next = None
  
  class Solution:
      def mergeTwoLists(self, l1: ListNode, l2: ListNode) -> ListNode:  # 传入的l1和l2是两个头节点
          root = ListNode(None)  # 首先建立一个空的节点，起名root  其实就是不需要管他的val值
          cur = root  # 需要一个当前的节点，这个节点每次会指向新加入的元素
          while l1 and l2:  # 如果l1和l2都不为空的话，就一直往下走  
              if l1.val < l2.val:      
                  node = ListNode(l1.val)  # 新建一个节点  需要把l1.val 和 l2.val中小的值加进去
                  l1 = l1.next  # 将l1前移
              else:  # l1.val >= l2.val
                  node = ListNode(l2.val)  # 将l2.val追加进新链表 
                  l2 = l2.next  # 再让l2往前面移动一个位置
              cur.next = node  # 让cur指向新建立的节点
              cur = node  # cur前移
          # 循环结束就必然l1和l2中有一个为None  l1或者l2的这两个链表里面可能还有剩余元素  把剩余的元素也追加进来
          if l1 is None:
              cur.next = l2  # 把最后一个节点指向l2
          else:  # l2 is None
              cur.next = l1  # 把最后一个节点指向l1
          return root.next  # root节点指向结果链表的第一个节点
  
  
  ```

简化一下代码

```python
# 循环结束就必然l1和l2中有一个为None  l1或者l2的这两个链表里面可能还有剩余元素  把剩余的元素也追加进来
cur.next = l1 or l2  # l1为None的话，cur.next就是l2了；l1不为None的话，cur.next就是l1了  短路操作符   

```
  # 多写多练

  ## 找到相关的题目，多做一些练习

  * 一般可能一次很难写对
  * 尝试自己先思考，先按照自己的方式编写代码，提交后发现问题
  * 如果实在没有思路或者想参考别人的思路可以搜题解

  

