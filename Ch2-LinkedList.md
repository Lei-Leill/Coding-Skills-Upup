# Day3

**203. 移除链表元素**

https://leetcode.cn/problems/remove-linked-list-elements/description/

这一题单指针就可以满足了，而且思考起来会更简单，要点：若找到val，pointer不往后移，接着判断下一个是否为val
```
#题解
    dummy = ListNode(-1)
    dummy.next = head
    pointer = dummy
    while pointer.next:
        if pointer.next.val == val:
            pointer.next = pointer.next.next
        else:
            pointer = pointer.next
    return dummy.next
```
之前做过一次，这次还是有点绕迷糊了，用了双指针，第一次尝试能够移除第一次出现的val，但是到第二次就不行了，这源于进入curr.val == val里面的东西写的不对，**要保证prev一定在next前面**，
8把这个该正确之后，在[7,7,7]这个case出错，因为没法移除挨在一起的两个元素，**要注意在curr.val==val的时候prev不往后移，否则跳过了元素**

```
#我的双指针法
      dummy = ListNode(-1)
      dummy.next = head
      prev = dummy
      curr = head
      while curr!= None:
          if curr.val == val:
              prev.next = curr.next
              curr = curr.next
          else:
              curr = curr.next
              prev = prev.next
      return dummy.next
```


**707. 设计链表**

https://leetcode.cn/problems/design-linked-list/description/

好久没有define一个data strcuture了，快忘了node和linkedlist的关系，size是很有用的东西

单向链表 Singly Linked List

**Dummy虚拟节点的方便性**
```
class LinkedNode:
    def __init__(self, val=0, next = None):
        self.val = val
        self.next = next

class MyLinkedList:

    def __init__(self, val = 0, next = None):
        self.dummy = LinkedNode()
        self.size = 0
        

    def get(self, index: int) -> int:
        if index < 0 or index >= self.size:
            return -1
        
        curr = self.dummy.next
        for i in range(index):
            curr = curr.next
        return curr.val      

    def addAtHead(self, val: int) -> None:
        self.dummy.next = LinkedNode(val, self.dummy.next)
        self.size += 1
        

    def addAtTail(self, val: int) -> None:
        curr = self.dummy
        for i in range(self.size):
            curr = curr.next
        curr.next = LinkedNode(val)
        self.size += 1

    def addAtIndex(self, index: int, val: int) -> None:
        if index < 0 or index >= self.size:
            return
        curr = self.dummy
        for i in range(index):
            curr = curr.next
        temp = curr.next
        curr.next = LinkedNode(val, temp)
        self.size += 1

    def deleteAtIndex(self, index: int) -> None:
        if index < 0 or index >= self.size:
            return
        curr = self.dummy
        for i in range(index):
            curr = curr.next
        if curr.next:
            curr.next = curr.next.next
        else:
            curr.next = None
        self.size -= 1
```
双向链表 Doubly Linked List
1. head 和 tail 查找和增删可以更高效，如果离前面近则cur = head, curr = curr.next; 如果离后面近，则cur = tail, curr = tail.prev
2. 虚拟节点不需要，因为有prev这个功能，所以删除/改变首节点挺容易


**206. 反转链表**

https://leetcode.cn/problems/reverse-linked-list/description/

自己做的时候没想出来，看了动画一下子想通了**双指针prev, curr + temp存储curr.next的做法**

```
def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
    prev = None
    curr = head
    while curr != None:
        temp = curr.next
        curr.next = prev
        prev = curr
        curr = temp
    return prev
```

recursion
```
def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
    return self.reverse(None, head) #初始化

def reverse(self, prev, curr): #和loop一样的逻辑
    if curr != None: # while 改成 if
        temp = curr.next
        curr.next = prev
        return self.reverse(curr, temp)
    return prev
```

# Day4

**24. Swap Pairs**

Recursion
```

```

