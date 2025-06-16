---
title: 打卡Day2
date: 2023-09-24 15:00:00
---
# 算法打卡

### 题目：

请你设计并实现一个满足 [LRU (最近最少使用) 缓存](https://baike.baidu.com/item/LRU) 约束的数据结构。

实现 `LRUCache` 类：

- `LRUCache(int capacity)` 以 **正整数** 作为容量 `capacity` 初始化 LRU 缓存
- `int get(int key)` 如果关键字 `key` 存在于缓存中，则返回关键字的值，否则返回 `-1` 。
- `void put(int key, int value)` 如果关键字 `key` 已经存在，则变更其数据值 `value` ；如果不存在，则向缓存中插入该组 `key-value` 。如果插入操作导致关键字数量超过 `capacity` ，则应该 **逐出** 最久未使用的关键字。

函数 `get` 和 `put` 必须以 `O(1)` 的平均时间复杂度运行。

### 示例：

```golang
输入
["LRUCache", "put", "put", "get", "put", "get", "put", "get", "get", "get"]
[[2], [1, 1], [2, 2], [1], [3, 3], [2], [4, 4], [1], [3], [4]]
输出
[null, null, null, 1, null, -1, null, -1, 3, 4]

解释
LRUCache lRUCache = new LRUCache(2);
lRUCache.put(1, 1); // 缓存是 {1=1}
lRUCache.put(2, 2); // 缓存是 {1=1, 2=2}
lRUCache.get(1);    // 返回 1
lRUCache.put(3, 3); // 该操作会使得关键字 2 作废，缓存是 {1=1, 3=3}
lRUCache.get(2);    // 返回 -1 (未找到)
lRUCache.put(4, 4); // 该操作会使得关键字 1 作废，缓存是 {4=4, 3=3}
lRUCache.get(1);    // 返回 -1 (未找到)
lRUCache.get(3);    // 返回 3
lRUCache.get(4);    // 返回 4
```

 

### 题解：

这题可以通过双向链表来实现，首先我们把每个节点当作一本书，我们使用多的是不是就放在了书堆的上面，而使用少的就放在下面了，而lru算法就是这样使用多的我们保留，使用少的在我们链表达到最大的情况下需要在插入新的书时就会把使用次数少的删除。首先，get函数里面去查找key如果key没有就返回-1，如果有就把该value返回，并且把该值前移到前面，put函数首先需要去判断是否存在，如果存在将value替换并把该值前移，如果没有就添加该值，然后判断是否大于该链表的长度如果大于就删除掉链表的最后一个

### 代码

```golang
type LRUCache struct {

​    list *list.List

​    capacity int

​    keyToNode map[int]*list.Element

}

type entry struct{

  key int

  value int

}





func Constructor(capacity int) LRUCache {

  return LRUCache{list.New(),capacity,map[int]*list.Element{}}

}





func (this *LRUCache) Get(key int) int {

​    node:=this.keyToNode[key]

​    if node ==nil{

​      return -1

​    }

​    this.list.MoveToFront(node)

​    return node.Value.(entry).value

}





func (this *LRUCache) Put(key int, value int)  {

  node:=this.keyToNode[key]

  if node!=nil{

​    node.Value=entry{key,value}

​    this.list.MoveToFront(node)

​    return

  }

  this.keyToNode[key]=this.list.PushFront(entry{key,value})

  if len(this.keyToNode)>this.capacity{

​    delete(this.keyToNode,this.list.Remove(this.list.Back()).(entry).key)

  }

}





/**

 \* Your LRUCache object will be instantiated and called as such:

 \* obj := Constructor(capacity);

 \* param_1 := obj.Get(key);

 \* obj.Put(key,value);

 */
```

