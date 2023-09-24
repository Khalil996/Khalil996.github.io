---
title: 打卡Day1
top: 1
---
# 算法每日打卡

### 题目：

给你一棵 `n` 个节点的树，编号从 `0` 到 `n - 1` ，以父节点数组 `parent` 的形式给出，其中 `parent[i]` 是第 `i` 个节点的父节点。树的根节点为 `0` 号节点，所以 `parent[0] = -1` ，因为它没有父节点。你想要设计一个数据结构实现树里面对节点的加锁，解锁和升级操作。

数据结构需要支持如下函数：

- **Lock：**指定用户给指定节点 **上锁** ，上锁后其他用户将无法给同一节点上锁。只有当节点处于未上锁的状态下，才能进行上锁操作。
- **Unlock：**指定用户给指定节点 **解锁** ，只有当指定节点当前正被指定用户锁住时，才能执行该解锁操作。
- **Upgrade：**指定用户给指定节点 上锁 ，并且将该节点的所有子孙节点 解锁 。只有如下 3 个条件全部满足时才能执行升级操作：
  - 指定节点当前状态为未上锁。
  - 指定节点至少有一个上锁状态的子孙节点（可以是 **任意** 用户上锁的）。
  - 指定节点没有任何上锁的祖先节点。

请你实现 `LockingTree` 类：

- `LockingTree(int[] parent)` 用父节点数组初始化数据结构。
- `lock(int num, int user)` 如果 id 为 `user` 的用户可以给节点 `num` 上锁，那么返回 `true` ，否则返回 `false` 。如果可以执行此操作，节点 `num` 会被 id 为 `user` 的用户 **上锁** 。
- `unlock(int num, int user)` 如果 id 为 `user` 的用户可以给节点 `num` 解锁，那么返回 `true` ，否则返回 `false` 。如果可以执行此操作，节点 `num` 变为 **未上锁** 状态。
- `upgrade(int num, int user)` 如果 id 为 `user` 的用户可以给节点 `num` 升级，那么返回 `true` ，否则返回 `false` 。如果可以执行此操作，节点 `num` 会被 **升级** 。

### 示例：

![](https://assets.leetcode.com/uploads/2021/07/29/untitled.png)

```
输入：
["LockingTree", "lock", "unlock", "unlock", "lock", "upgrade", "lock"]
[[[-1, 0, 0, 1, 1, 2, 2]], [2, 2], [2, 3], [2, 2], [4, 5], [0, 1], [0, 1]]
输出：
[null, true, false, true, true, true, false]

解释：
LockingTree lockingTree = new LockingTree([-1, 0, 0, 1, 1, 2, 2]);
lockingTree.lock(2, 2);    // 返回 true ，因为节点 2 未上锁。
                           // 节点 2 被用户 2 上锁。
lockingTree.unlock(2, 3);  // 返回 false ，因为用户 3 无法解锁被用户 2 上锁的节点。
lockingTree.unlock(2, 2);  // 返回 true ，因为节点 2 之前被用户 2 上锁。
                           // 节点 2 现在变为未上锁状态。
lockingTree.lock(4, 5);    // 返回 true ，因为节点 4 未上锁。
                           // 节点 4 被用户 5 上锁。
lockingTree.upgrade(0, 1); // 返回 true ，因为节点 0 未上锁且至少有一个被上锁的子孙节点（节点 4）。
                           // 节点 0 被用户 1 上锁，节点 4 变为未上锁。
lockingTree.lock(0, 1);    // 返回 false ，因为节点 0 已经被上锁了。
```

 

### 提示：

- `n == parent.length`
- `2 <= n <= 2000`
- 对于 `i != 0` ，满足 `0 <= parent[i] <= n - 1`
- `parent[0] == -1`
- `0 <= num <= n - 1`
- `1 <= user <= 104`
- `parent` 表示一棵合法的树。
- `lock` ，`unlock` 和 `upgrade` 的调用 **总共** 不超过 `2000` 次。



### 题解：

#### DFS：

定义变量：

- `locked`:记录每个节点的锁定状态，其中*`locked[i]`*表示节点i的锁定状态，如果节点i未被上锁，则`locked[i]`=-1，否则`locked[i]`为锁定节点i的用户编号。

- `parent`:记录每个节点的父节点

- `children`:记录每个节点的子节点

调用lock函数，如果节点num未被上锁，则将节点num上锁，返回true，否则返回false。

调用unlock函数，如果节点被上锁且用户为user，则节点num解锁返回true，否则返回false。

调用upgrade函数，先判断节点num及其祖先节点是否被上锁，如果是，则返回false；否则，我们判断节点num的子孙节点是否有被上锁，没用则返回false，否则，我们将节点num和他的子孙节点解锁，然后将节点num上锁，返回true。

### 代码：

```golang
type LockingTree struct {
    locked []int
    parent []int
    children [][]int
}


func Constructor(parent []int) LockingTree {
    n:=len(parent)
    locked :=make([]int,n)
    for i:=range locked{
        locked[i]=-1
    }
    children:=make([][]int,n)
     for i:=1;i<n;i++{
         children[parent[i]]=append(children[parent[i]],i)
     }
     return LockingTree{locked, parent, children}

}


func (this *LockingTree) Lock(num int, user int) bool {
    if this.locked[num]==-1{
        this.locked[num]=user
        return true
    }
    return false
}


func (this *LockingTree) Unlock(num int, user int) bool {
    if this.locked[num]==user{
        this.locked[num]=-1
        return true
    }
    return false
}


func (this *LockingTree) Upgrade(num int, user int) bool {
    x:=num
    for ;x!=-1;x=this.parent[x]{
        if this.locked[x]!=-1{
            return false
        }
    }
    find:=false
    var dfs func(int)
    dfs=func(x int){
        for _,y:=range this.children[x]{
            if this.locked[y]!=-1{
                find=true
                this.locked[y]=-1
            }
            dfs(y)
        }
    }   
    dfs(num)
    if !find {
        return false
    }
    this.locked[num]=user
    return true
    
}


/**
 * Your LockingTree object will be instantiated and called as such:
 * obj := Constructor(parent);
 * param_1 := obj.Lock(num,user);
 * param_2 := obj.Unlock(num,user);
 * param_3 := obj.Upgrade(num,user);
 */
```



