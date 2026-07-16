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

---

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

