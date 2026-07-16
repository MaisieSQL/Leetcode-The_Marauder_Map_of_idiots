# 07/16/2026
今天下午刷neetcode6道(昨天discord小组会的以及今天要讨论的)+leetcode4道heap.很难...我需要做好心理准备.再次声明:整个下午遇到的所有状况和不顺,都不是我个人人品和能力问题,都是学习不会知识的正常情况,请务必上升个人.学习有快慢,掌握有深浅,小蜗牛也可以快乐的过一生

---
## Section 1 复习昨天discord的3道题:
* **Word Search**:(Medium)
  DFS/Backtracking
* **CombinationSum**:(Medium)
  Backtracking
* **Find Median From Data Stream**:(Hard)
  Two Pointers/Heap/Sorting


# 🟢 LeetCode 79. Word Search (Medium)

### 1. 核心思想：DFS + 回溯 (Backtracking)
在一维或二维空间中搜索一条满足特定条件的路径。因为同一个格子在一次路径中不能重复使用，我们需要在探索时**标记已访问**，并在回溯（退栈）时**还原现场**。

### 2. 算法步骤拆解
1. **寻找起点**：双重循环遍历网格，当 `board[r][c] == word[0]` 时，以此为起点开始 DFS 搜索。
2. **递归出口 (Base Cases)**：
   * **成功**：匹配字符索引 `i == len(word)`，说明单词全部匹配成功，返回 `True`。
   * **失败**：越界（行/列超出范围）或当前字符不匹配 `board[r][c] != word[i]`，返回 `False`。
3. **标记已访问**：保存当前字符 `temp = board[r][c]`，然后将当前位置修改为特殊字符（如 `board[r][c] = '#'`），防止在当前路径中重复走回头路。
4. **四向探索**：递归调用上下左右四个方向 `dfs(r + 1, c, i + 1) or dfs(r - 1, c, i + 1) ...`。
5. **回溯（清理现场）**：递归返回前，必须将当前位置还原 `board[r][c] = temp`，以免影响其他路径的搜索。

### 3. Python 核心实现

```python
class Solution:
    def exist(self, board: list[list[str]], word: str) -> bool:
        ROWS, COLS = len(board), len(board[0])
        
        def dfs(r, c, i) -> bool:
            # 1. 成功出口：单词所有字符匹配完毕
            if i == len(word):
                return True
                
            # 2. 失败出口：越界、或者字符不匹配
            if (r < 0 or c < 0 or 
                r >= ROWS or c >= COLS or 
                board[r][c] != word[i]):
                return False
            
            # 3. 标记当前路径已访问（用 '#' 替代，省去额外 visited 空间）
            temp = board[r][c]
            board[r][c] = "#"
            
            # 4. 往四个方向探索
            found = (dfs(r + 1, c, i + 1) or
                     dfs(r - 1, c, i + 1) or
                     dfs(r, c + 1, i + 1) or
                     dfs(r, c - 1, i + 1))
            
            # 5. 回溯：清理现场，还原字符
            board[r][c] = temp
            
            return found

        # 遍历网格，寻找匹配 word[0] 的起点
        for r in range(ROWS):
            for c in range(COLS):
                if board[r][c] == word[0]:
                    if dfs(r, c, 0):
                        return True
        return False
```

# 🟡 LeetCode 39. Combination Sum

### 1. 题目描述
给你一个**无重复元素**的整数数组 `candidates` 和一个目标整数 `target` ，找出 `candidates` 中可以使数字和为目标数 `target` 的所有**唯一组合**。

`candidates` 中的同一个数字可以**无限制重复被选取**。如果至少一个数字的被选数量不同，则两种组合是唯一的。

**示例 1:**
> **输入:** candidates = [2,3,6,7], target = 7  
> **输出:** [[2,2,3],[7]]  
> **解释:** 2 和 3 可以形成 2 + 2 + 3 = 7 。注意 2 可以使用多次。7 也是一个候选， 2 + 2 + 3 = 7 。这里只有两种组合。

### 2. 核心思路：二叉决策树 (Decision Tree)
为了**彻底避免产生重复的组合**（例如产生 `[2, 2, 3]` 后又产生 `[2, 3, 2]`），我们采用**顺序选择法**。在决策树的每一层，我们只针对当前索引 `i` 的元素做两个选择：

1. **选当前元素**：
   * 将 `candidates[i]` 加入路径 `cur`。
   * 递归进入下一层，**索引保持为 `i`**（因为可以重复使用），`total` 累加。
2. **不选当前元素**：
   * 将刚才加进去的 `candidates[i]` 从 `cur` 中弹出（**回溯清理**）。
   * 递归进入下一层，**索引强制推进到 `i + 1`**（此后该分支再也无法选取 `candidates[i]`）。

这种“要或不要”的二叉树分叉，保证了元素的选择顺序永远是单向向右的，天然去重。

### 3. Python 完整实现

```python
class Solution:
    def combinationSum(self, candidates: list[int], target: int) -> list[list[str]]:
        res = []
        
        # 1. 排序可以让我们在 total > target 时提前退出，减少不必要的计算
        candidates.sort()
        
        def dfs(i: int, cur: list[int], total: int):
            # 成功出口
            if total == target:
                res.append(cur.copy())  # 必须使用深拷贝
                return
            
            # 失败出口：索引越界 或 累加和已超标
            if i >= len(candidates) or total > target:
                return
            
            # 选择 1：包含 candidates[i]
            cur.append(candidates[i])
            dfs(i, cur, total + candidates[i])  # 索引保持 i
            
            # 选择 2：不包含 candidates[i]（先 pop 回溯，再推进索引）
            cur.pop()
            dfs(i + 1, cur, total)              # 索引推进到 i + 1

        dfs(0, [], 0)
        return res
```

# 🔴 LeetCode 295. Find Median From Data Stream

### 1. 题目描述
实现一个 `MedianFinder` 类，支持以下两个操作：
* `addNum(int num)`：从数据流中添加一个整数到数据结构中。
* `findMedian()`：返回目前所有元素的中位数。

**示例:**
```python
finder = MedianFinder()
finder.addNum(1)
finder.addNum(2)
# 目前数据为 [1, 2]，中位数是 (1 + 2) / 2 = 1.5
print(finder.findMedian()) # 输出 1.5

finder.addNum(3)
# 目前数据为 [1, 2, 3]，中位数是中间的 2
print(finder.findMedian()) # 输出 2.0
```

### 2. 核心思路：双堆对顶 (Two Heaps)
如果每次求中位数都对数组进行排序，时间复杂度会达到 $O(N \log N)$，即使插入时用二分查找，移动元素也需要 $O(N)$。

为了实现高效的动态查询，我们将有序数据流一分为二：
* **前半部分（较小的一半）**：用一个 **大顶堆 (Max-Heap)** 维护，我们只需要它的最大值。
* **后半部分（较大的一半）**：用一个 **小顶堆 (Min-Heap)** 维护，我们只需要它的最小值。

#### 核心维护规则（平衡约束）：
1. **数值大小关系**：大顶堆的最大值（堆顶）必须永远小于或等于小顶堆的最小值（堆顶）。
2. **元素数量平衡**：两个堆的元素个数差值绝对值不能超过 1：
   $$\left| \text{len(small)} - \text{len(large)} \right| \le 1$$

### 3. Python 完整实现
> **注意**：Python 内建的 `heapq` 库仅支持小顶堆。为了实现大顶堆，我们需要在存入数据时将数字乘以 -1，取出使用或比较时再乘以 -1 还原。

```python
import heapq

class MedianFinder:

    def __init__(self):
        # small 是大顶堆，用来存较小的前半部分。
        # 存入负数来模拟大顶堆。
        self.small = [] 
        # large 是小顶堆，用来存较大的后半部分。
        self.large = [] 

    def addNum(self, num: int) -> None:
        # 1. 默认先放进大顶堆 small（存为负数）
        heapq.heappush(self.small, -num)
        
        # 2. 确保大顶堆的最大值 <= 小顶堆的最小值
        if self.small and self.large and (-self.small[0] > self.large[0]):
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
            
        # 3. 确保两个堆的数量差不超过 1
        if len(self.small) > len(self.large) + 1:
            val = -heapq.heappop(self.small)
            heapq.heappush(self.large, val)
        elif len(self.large) > len(self.small) + 1:
            val = heapq.heappop(self.large)
            heapq.heappush(self.small, -val)

    def findMedian(self) -> float:
        # 如果两堆数量不等，数量多的那个堆的堆顶就是中位数
        if len(self.small) > len(self.large):
            return float(-self.small[0])
        elif len(self.large) > len(self.small):
            return float(self.large[0])
        
        # 如果两堆数量相等，中位数是两个堆顶元素的平均值
        return (-self.small[0] + self.large[0]) / 2.0
```

---

