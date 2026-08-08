# Python 积跬步：LeetCode 随题点拨清单

每题只主动讲 1 个，最多 2 个。必须和当前题直接相关。

---

## 数组 / 哈希阶段

### `enumerate`
```python
for i, x in enumerate(nums):
    ...
```
同时得到索引和值。比手写 `for i in range(len(nums)): x = nums[i]` 更直接。

### dict 成员查询
```python
if need in pos:
    return [pos[need], i]
```
`in` 检查的是 key。

### `dict.get`
```python
count[x] = count.get(x, 0) + 1
```
key 不存在时返回默认值，适合计数。

### set
```python
seen = set()
seen.add(x)
if x in seen:
    ...
```
只需要“在不在”时优先考虑。

---

## 双指针 / 滑窗

### `while l < r`
当指针移动依赖当前状态时，`while` 比 `for` 更自然。

### 字典/数组计数增减
```python
freq[ch] = freq.get(ch, 0) + 1
freq[left_ch] -= 1
```
理解“右端加入、左端移除”的对称更新。

### `max` / `min`
```python
ans = max(ans, r - l + 1)
```
先确认区间是闭区间 `[l, r]`，长度才是 `r-l+1`。

---

## 链表

### Python 变量保存的是对象引用
```python
nxt = cur.next
cur.next = prev
prev = cur
cur = nxt
```
关键不是 C++ 指针语法，而是“改链之前先保存原来的下一个对象”。

### 多变量赋值
```python
prev, cur = cur, cur.next
```
右侧先整体求值，再赋给左侧。只在逻辑已经很清楚时使用；初学阶段复杂断链优先分行写。

---

## 栈 / 队列

### list 当栈
```python
stack.append(x)
x = stack.pop()
```
末尾进出都高效。

### `deque` 当队列
```python
from collections import deque
q = deque([start])
node = q.popleft()
```
BFS 不要频繁 `list.pop(0)`。

---

## 树 / DFS / 回溯

### Python 递归函数
```python
def dfs(node):
    if node is None:
        return ...
```
先理解函数契约，不只背语法。

### 路径回溯
```python
path.append(x)
dfs(...)
path.pop()
```
`append/pop` 是“做选择/撤销选择”的代码对应。

### `nonlocal`
若嵌套函数需要修改外层局部变量，可以：
```python
nonlocal ans
```
但不必为了炫技使用；也可让 DFS 返回结果，往往更清晰。

---

## 堆

### `heapq`
```python
import heapq
heapq.heappush(heap, x)
smallest = heapq.heappop(heap)
```
Python 标准库是最小堆。

需要“最大堆”时常存负数，但第一次遇到要解释为什么。

---

## 常用工具

### `defaultdict`
```python
from collections import defaultdict
groups = defaultdict(list)
groups[key].append(x)
```
适合“key 对应一个容器”的分组。

### `Counter`
```python
from collections import Counter
freq = Counter(s)
```
只在题目核心不是“如何自己维护计数”时使用。若计数本身就是学习重点，优先手写 dict 一次。

### `float('inf')`
```python
best = float('inf')
```
表示一个足够大的初始值，常用于求最小值。

### 倒序 range
```python
for i in range(n - 1, -1, -1):
    ...
```
结束值 `-1` 不会被取到，因此会遍历 `n-1 ... 0`。

### 切片会创建新 list（常见情况）
```python
sub = nums[l:r]
```
不要默认它 O(1)。长度为 k 的切片通常 O(k) 时间和空间。

### 排序
```python
nums.sort()      # 原地，返回 None
b = sorted(nums) # 返回新列表
```
要明确题目是否允许修改原数组。

---

# 教练选择 Python 点拨的优先级

1. 用户刚刚因为它报错/困惑；
2. 它是当前解法关键 API；
3. 它未来 Hot100 会高频复用；
4. 它能帮助理解复杂度或对象语义。

不要为了“每题必须有新语法”强行引入冷门写法。没有值得讲的新点时，可以复习一个旧点。
