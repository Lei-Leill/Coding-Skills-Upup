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

Recursion，之前做过一次，记得大概
```
def swapPairs(self, head: Optional[ListNode]) -> Optional[ListNode]:
        if head == None or head.next == None: # if it is the last one or null
            return head # no action needed, just return head
        prev = head
        curr = head.next
        nexthead = self.swapPairs(head.next.next) # 关键点
        curr.next = head
        prev.next = nexthead
        return curr
```

**19.删除链表的倒数第n个节点**

一开始没什么好思路，看到视频讲解通了，自己写出来，稍微卡了一下，通过画图明白了

关键点：如何让指针指向倒数第n个节点的前面一个，通过快慢指针来实现，快指针指向链表最后，快慢之间相差n，则慢指针正好指向target节点，特别妙的快慢指针用法

```
def removeNthFromEnd(self, head: Optional[ListNode], n: int) -> Optional[ListNode]:
    dummy = ListNode(-1)
    dummy.next = head
    slow, fast = dummy, dummy
    i = 0
    while fast and i < n:
        fast = fast.next
        i += 1

    while fast and fast.next: #fast走到最后一个节点停
        slow = slow.next
        fast = fast.next
    slow.next = slow.next.next
    return dummy.next
```

**160.链表相交**

之前做过，不太记得了，

是一个非常妙O(m+n)的解法，因为链表A和链表B加起来的长度一致，所以我们只需要设计两个pointer分别指向headA和headB，pointerA遍历完headA就指向headB，headB遍历完headB指向headA。不用担心停不下来，因为他们加起来的长度一致，假如说worst case，没有交点，当pointerA和B都指向None的时候停下
```
def getIntersectionNode(self, headA: ListNode, headB: ListNode) -> ListNode:
        pointerA = headA
        pointerB = headB

        while(pointerA != pointerB):
            if pointerA == None:
                pointerA = headB
            else:
                pointerA = pointerA.next
            
            if pointerB == None:
                pointerB = headA
            else:
                pointerB = pointerB.next
        return pointerA
```

**142.环形列表**

关键点：
1. 判断是否有环：快慢指针，慢指针一次往后走一个，快指针往后走两个，如果有环的话他们一定能相遇，因为一开始快指针快 走在前面，进入环之后快指针开始对慢指针进行追击，每次比慢指针快一步
2. 找出环的入口：通过模拟过程和数学计算，我们可以发现找到节点的办法，看代码随想录的题解，讲的特别好

照着这个思路，自己一遍写出来了
```
def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
    slow = fast = head
    while fast and fast.next:
        fast = fast.next.next
        slow = slow.next
        if slow == fast:
            pointer1 = slow
            pointer2 = head
            while pointer1 != pointer2:
                pointer1= pointer1.next
                pointer2 = pointer2.next
            return pointer1
    return None
```

# 总结

这次做链表比之前好一些些了

pointer来遍历节点是常用手段，这样可以保证要被返回的头节点

删除节点关键： 
1. 指针要指向要删的节点前一个
2. Dummyhead 可以用来保证删除节点的时候不用额外考虑头节点

快慢指针：
1. 找倒数第n个节点那题快慢指针用的挺妙了

要在实现代码之前visualize过程，比如反转链表，两两交换

找出相交节点 和 环形链表的思路挺妙的



