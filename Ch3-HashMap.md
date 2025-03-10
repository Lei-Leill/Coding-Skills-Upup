# Day6

当我们需要判断一个元素是否在集合中时，就要想到用哈希表了


**理论基础**

map逻辑： {key: value} pair； 
每个key通过Hash function映射为一个index，index%hashtable_size, 以防index超过size，把value填入hash table中

哈希碰撞发生在两个不同的key映射到同一个位置的时候，这时候用 
1. LinkedList来连起来同一位置的key对应的value
2. 线性探索法，如果这个index有元素，则把value填到下一个空缺的位置。这个办法一定要保证table size > data size

常见的三种哈希结构, 
- array: 在datasize不大的情况下，character/int可以直接转换为index的时候最好用
- set: 数组为基础 unordered set,  tree为基础set, multiset
- map：


**242.有效的字母异位词**

暴力解法 O(N^2) --> Array/ Hashmap O(2N)

不用初始化key，用dic的形式：if key not in dict: ... else: ...
``` 
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        from collections import defaultdict
        
        s_dict = defaultdict(int)
        t_dict = defaultdict(int)
        for x in s:
            s_dict[x] += 1
        
        for x in t:
            t_dict[x] += 1
        return s_dict == t_dict
```
特别方便的fucntion用来count occurence of element of list/string
``` 
class Solution(object):
    def isAnagram(self, s: str, t: str) -> bool:
        from collections import Counter
        a_count = Counter(s) #stores elements as keys and their counts as values
        b_count = Counter(t)
        return a_count == b_count
```
我之前想的类似这个解法, 类似数组方式
```
class Solution:
    def isAnagram(self, s: str, t: str) -> bool:
        record = [0] * 26
        for i in s:
            #并不需要记住字符a的ASCII，只要求出一个相对数值就可以了
            record[ord(i) - ord("a")] += 1
        for i in t:
            record[ord(i) - ord("a")] -= 1
        for i in range(26):
            if record[i] != 0:
                #record数组如果有的元素不为零0，说明字符串s和t 一定是谁多了字符或者谁少了字符。
                return False
        return True
```

**349.两个数组的交集

由于题目强调了输出结果中的每一个元素是唯一的，所以考虑用集合set

```

```
