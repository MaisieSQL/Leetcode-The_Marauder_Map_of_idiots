
在算法面试中，DFS 有两种截然不同的形态：“网格/图遍历” 和 “回溯（Backtracking）排组问题”。
为了让你在遇到新题时能够闭着眼睛写出框架，下面为你整理了这两种形态的万能模板和对应的殿堂级“母题”。

## 1. 第一类：网格/图的 DFS 搜索模板
这类题目通常是在一个二维矩阵里探路（比如 Word Search、岛屿数量）。它的核心是**边界检查**和**防回头（Visited）**。

### 🧱 万能代码模板

```python
def solve(grid):
    ROWS, COLS = len(grid), len(grid[0])
    visited = set() # 或者直接在 grid 里修改（如改成 '#'）来省空间

    def dfs(r, c):
        # 1. 递归出口 ✗：越界、已被访问、或不满足题目条件（如不是陆地/不等于目标字符）
        if (r < 0 or c < 0 or 
            r >= ROWS or c >= COLS or 
            (r, c) in visited or grid[r][c] == "不合格条件"):
            return 
        
        # 2. 标记当前节点为已访问
        visited.add((r, c))
        
        # 3. 递归探索相邻节点（上下左右，或者八个方向）
        directions = [(1, 0), (-1, 0), (0, 1), (0, -1)]
        for dr, dc in directions:
            dfs(r + dr, c + dc)
            
        # 4. （可选）如果题目需要寻找单条特定路径，在这里回溯
        # visited.remove((r, c))

    # 外部循环遍历所有可能的起点
    for r in range(ROWS):
        for c in range(COLS):
            if grid[r][c] == "起点条件":
                dfs(r, c)
```

### 👑 经典母题：LeetCode 200. Number of Islands (岛屿数量) - Medium
题目核心：给你一个由 '1'（陆地）和 '0'（水）组成的二维网格，请你计算网格中岛屿的数量。

母题代码：
```python
class Solution:
    def numIslands(self, grid: list[list[str]]) -> int:
        if not grid: return 0
        ROWS, COLS = len(grid), len(grid[0])
        islands = 0

        def dfs(r, c):
            # 越界或遇到水直接返回
            if r < 0 or c < 0 or r >= ROWS or c >= COLS or grid[r][c] == '0':
                return
            
            # 将当前陆地“淹没”（标记为 '0'），防止重复访问
            grid[r][c] = '0'
            
            # 淹没相连的上下左右所有陆地
            dfs(r + 1, c)
            dfs(r - 1, c)
            dfs(r, c + 1)
            dfs(r, c - 1)

        for r in range(ROWS):
            for c in range(COLS):
                if grid[r][c] == '1': # 发现一块新陆地
                    dfs(r, c) # 用 DFS 把它连接的整块岛屿全部淹没
                    islands += 1 # 岛屿数加 1
        return islands
```

## 2. 第二类：回溯（Backtracking）排组模板
这类题目是在一个决策树中寻找某种排列（Permutation）、组合（Combination）或子集（Subset），比如 `Combination Sum`。它的核心是**撤销选择**。

### 🧱 万能代码模板

```python
def backtrack(choices, path, res):
    # 1. 成功出口 ✓：满足了某种条件（比如路径长度够了，或者和等于 target）
    if is_solution(path):
        res.append(path.copy()) # 必须拷贝！
        return
        
    # 2. 剪枝出口 ✗：已经不可能凑出解了（比如累加和已经超过 target）
    if should_prune(path):
        return

    # 3. 遍历当前可以做的所有选择（即决策树的分支）
    for choice in choices:
        if not is_valid(choice): 
            continue # 过滤非法选择
            
        # 做选择：把选择加入当前路径
        path.append(choice)
        
        # 递归：进入决策树的下一层
        backtrack(choices, path, res)
        
        # 撤销选择：回溯的核心！清理现场，退回上一步
        path.pop()
```

### 👑 经典母题：LeetCode 46. Permutations (全排列) - Medium
题目核心：给定一个没有重复数字的序列 nums，返回其所有可能的全排列。

母题代码：

```python
class Solution:
    def permute(self, nums: list[int]) -> list[list[int]]:
        res = []
        
        def backtrack(path, visited):
            # 成功出口：当路径长度等于原数组长度时，说明找到了一个全排列
            if len(path) == len(nums):
                res.append(path.copy())
                return
                
            for num in nums:
                # 排除已经选过的数字
                if num in visited:
                    continue
                
                # 1. 做选择
                path.append(num)
                visited.add(num)
                
                # 2. 递归
                backtrack(path, visited)
                
                # 3. 撤销选择（回溯）
                path.pop()
                visited.remove(num)
                
        backtrack([], set()) 
        return res
```

# 🗺️ DFS 与回溯算法黄金通关路径

## 🧱 第一类：网格 / 二维图遍历（迷宫探路型）
> **核心逻辑**：在 $M \times N$ 的网格上，利用 DFS 向四周探索。重点是**越界处理**、**连通分量标记**和**避免回头路**（在原网格修改标记，或使用 `visited` 集合）。

| 难度 | 题号与题目 | 刷题核心与破局点 | 题目链接 |
| :--- | :--- | :--- | :--- |
| 🟢 **Easy** | [200] Number of Islands | **【终极母题】** 发现陆地就用 DFS 顺藤摸瓜把它能连通的陆地全部“淹没”（改成 `'0'`）。统计一共发起过几次 DFS。 | [Link](https://leetcode.com/problems/number-of-islands/) |
| 🟢 **Easy** | [695] Max Area of Island | **【带返回值的 DFS】** 淹没岛屿的同时，递归函数需要返回一个整数。每次往四个方向扩散并累加面积：`1 + dfs(上) + dfs(下)...`。 | [Link](https://leetcode.com/problems/max-area-of-island/) |
| 🟡 **Medium**| [130] Surrounded Regions | **【逆向思维】** 不要去正面找被包围的区域。先从矩阵**最外围一圈的边界**出发，用 DFS 标记所有与边界相连的 `'O'`（因为它们肯定不会被包围）。最后把没标记的 `'O'` 变 `'X'`，标记过的还原。 | [Link](https://leetcode.com/problems/surrounded-regions/) |
| 🟡 **Medium**| [79] Word Search | **【网格回溯】** 在网格里找一条匹配单词的单单一路径。在前进时把当前格子改成 `'#'` 防止重复，**回溯时（退栈前）必须还原现场**。只要有一个方向走通，立即剪枝返回。 | [Link](https://leetcode.com/problems/word-search/) |
| 🔴 **Hard** | [329] Longest Increasing Path in a Matrix | **【记忆化 DFS】** 寻找最长递增路径。直接 DFS 会超时，必须引入一个 `memo[r][c]` 矩阵，存下从每个格子出发能走的最长距离。如果走到算过的格子，直接返回结果。 | [Link](https://leetcode.com/problems/longest-increasing-path-in-a-matrix/) |

---

## 🎒 第二类：组合与子集问题（不考虑顺序，去重是关键）
> **核心逻辑**：从一个数组中挑选数字凑成一个组合。因为不考虑顺序（例如 `[1, 2]` 和 `[2, 1]` 是同一个组合），我们在向下递归时，必须**传入当前索引 `i`，下一层决策只从 `i` 或 `i + 1` 往后选，绝不走回头路**。

| 难度 | 题号与题目 | 刷题核心与破局点 | 题目链接 |
| :--- | :--- | :--- | :--- |
| 🟡 **Medium**| [78] Subsets | **【子集母题】** 找所有子集。每个元素只有“要”和“不要”两个分支。决策树的每一个节点对应的路径都是一个合法的子集。 | [Link](https://leetcode.com/problems/subsets/) |
| 🟡 **Medium**| [39] Combination Sum | **【可重复选，无重复元素】** 每个数可以无限选。每次分叉：1. 选当前数（下一层递归仍停留在 `i`，但 target 减小）；2. 不选当前数（递归推进到 `i + 1`）。 | [Link](https://leetcode.com/problems/combination-sum/) |
| 🟡 **Medium**| [40] Combination Sum II | **【不可重复选，有重复元素】** 为了防止产生重复组合，先对数组**排序**。在 `for` 循环横向展开决策树分支时，如果 `nums[j] == nums[j - 1]` 且 `j > i`，直接 `continue`（剪枝跳过相同的数）。 | [Link](https://leetcode.com/problems/combination-sum-ii/) |
| 🟡 **Medium**| [90] Subsets II | **【有重复元素的子集】** 与组合 II 类似。先排序，在同层决策时利用 `if nums[j] == nums[j - 1]` 剪枝，避免产生重复的子集。 | [Link](https://leetcode.com/problems/subsets-ii/) |

---

## 🔁 第三类：排列问题（考虑顺序，利用全局状态去重）
> **核心逻辑**：因为顺序不同代表不同的排列（例如 `[1, 2]` 和 `[2, 1]` 是两个不同的排列），所以每一步决策我们都**必须从头开始遍历所有可用的数字**。但要用一个 `visited` 集合来标记哪些数字已经被装进小口袋了。

| 难度 | 题号与题目 | 刷题核心与破局点 | 题目链接 |
| :--- | :--- | :--- | :--- |
| 🟡 **Medium**| [46] Permutations | **【全排列母题】** 每次从头循环所有数。如果数字已经出现在 `path` 里就跳过（用 `visited` 集合优化 $O(1)$ 查找）。走到叶子节点拷贝结果。 | [Link](https://leetcode.com/problems/permutations/) |
| 🟡 **Medium**| [47] Permutations II | **【含重复元素的全排列】** 先排序。如果 `nums[i] == nums[i-1]`，且前一个相同的数字还没有被访问过（`not visited[i-1]`），必须跳过。这是回溯算法中最经典也最难想通的剪枝逻辑之一。 | [Link](https://leetcode.com/problems/permutations-ii/) |

---

## 🔀 第四类：切分与游戏策略（高阶回溯）
> **核心逻辑**：不仅是在数组里选数字，还需要对数据进行**切割**、**校验**，或者模拟多方对抗的博弈状态。

| 难度 | 题号与题目 | 刷题核心与破局点 | 题目链接 |
| :--- | :--- | :--- | :--- |
| 🟡 **Medium**| [131] Palindrome Partitioning | **【分割回溯】** 把字符串切成若干个回文子串。每一步决策代表“在哪个位置砍一刀”。如果当前切下来的前缀是回文，就对剩下的字符串继续递归分割。 | [Link](https://leetcode.com/problems/palindrome-partitioning/) |
| 🟡 **Medium**| [93] Restore IP Addresses | **【分割与合法性校验】** 恰好切三刀，把字符串分成 4 段。每一段必须满足：数值在 0-255 之间，且不能有前导 0（比如 `01` 是非法的，而单个 `0` 是合法的）。 | [Link](https://leetcode.com/problems/restore-ip-addresses/) |
| 🔴 **Hard** | [51] N-Queens | **【大厂高频面试题】** 经典的八皇后问题。用三个集合分别维护被攻击的**列**、**正对角线 (r + c)** 和 **反对角线 (r - c)**。利用数学规律极速校验棋子冲突。 | [Link](https://leetcode.com/problems/n-queens/) |
| 🔴 **Hard** | [37] Sudoku Solver | **【数独终结者】** 每一个空格都需要试 `1` 到 `9`。如果在某个格子上没有任何一个数能填，直接退回上一步（回溯）。使用行、列、九宫格三个布尔数组来进行常数级别的合法性校验。 | [Link](https://leetcode.com/problems/sudoku-solver/) |

---

# ⚖️ DFS 与回溯算法（Backtracking）的异同

在刷题时，DFS 和回溯（Backtracking）这两个词经常成对出现，代码结构也极其相似。但在**概念逻辑**、**搜索策略**和**核心目的**上，它们有着微妙且关键的区别。

### 1. 核心异同对照表

| 维度 | 深度优先搜索 (DFS) | 回溯 (Backtracking) |
| :--- | :--- | :--- |
| **学科分类** | **图论**算法（一种通用的图/树遍历方式） | **算法设计思想**（一种系统性的求解策略） |
| **核心目的** | **遍历**。要把所有节点完整地、不重不漏地访问一遍。 | **求解**。要在所有可能的候选解中，找出满足特定条件的解（可能是一个，也可能是全部）。 |
| **关注重点** | 关注的是**节点（Node）**。我有没有访问过这个格子/顶点？ | 关注的是**选择（Edge/Choice）**和**状态**。我把这个数装进小口袋后，下一步还能怎么选？ |
| **标志性动作**| 沿着一个分支走到最深，然后退回。 | **撤销选择（Backtrack）**。退回上一步时，必须主动“清理现场”，把状态恢复。 |
| **典型代表** | 岛屿数量、连通分量、二叉树前序遍历。 | 全排列、N 皇后、数独。 |

### 2. 相同点：共享同一种“走路方式”

在具体实现上，**回溯几乎总是利用 DFS 的形式来执行。**

* **递归与栈**：它们都借助“先进后出”的栈结构（系统递归栈），沿着某一条路径死磕到底，直到碰壁了才往回退。
* **搜索树的结构**：不管是遍历一个物理存在的网格图（DFS），还是遍历一棵虚拟的决策树（回溯），它们在空间中的搜索轨迹是一模一样的。

### 3. 不同点：用两个生活场景秒懂

#### 场景 A（纯 DFS）：你在公园里巡逻
* **任务**：要把公园里所有的景点（节点）都走一遍，登记每个景点的状况。
* **做法**：遇到分叉路口就随便挑一条走到底，碰壁了就顺着原路退回到上一个分叉口，走另一条路。
* **核心**：你只关心“我有没有漏掉哪个景点”。你走过的路，只要登记过（Visited），就不需要再擦除这个登记记录。

#### 场景 B（回溯）：你在破解一串密码锁
* **任务**：密码锁有 3 位，每一位可以是 `1`、`2`、`3`。你要试出所有数字不重复的密码组合（如 `123`）。
* **做法**：
  1. 第一位你转到了 `1`。（做出选择 1）
  2. 第二位你转到了 `2`。（做出选择 2）
  3. 第三位你想转 `1` 或 `2`，发现重复了，此路不通！（碰壁）
  4. **关键一步**：你把第三位转回空挡，退回到第二位，把第二位从 `2` 转回空挡（**撤销选择**），然后尝试把第二位转到 `3`。
* **核心**：你不仅在退步，你还在**拼命地擦除刚才做过的物理标记**，让密码锁重置到之前的状态，否则你下一轮就没法继续尝试新的组合。

### 4. 代码层面的“一字之差”

我们可以通过最直观的代码结构，来看看回溯相比于普通 DFS，多出来的灵魂动作是什么：

#### 纯 DFS（以“岛屿数量”淹没陆地为例）
```python
def dfs(r, c):
    if 越界 or grid[r][c] == '0':
        return
    
    grid[r][c] = '0' # 1. 标记访问（永久修改状态，不需要还原）
    
    dfs(r + 1, c)    # 2. 往下探索
    dfs(r - 1, c)
    # 走了就走了，不需要在退栈时把 '0' 再改回 '1'。
```

#### 回溯（以“全排列”为例）
```python
def backtrack(path):
    if len(path) == len(nums):
        res.append(path.copy())
        return
        
    for num in nums:
        if num in visited: 
            continue
            
        path.append(num)    # 1. 做选择
        visited.add(num)
        
        backtrack(path)     # 2. 往下探索（DFS）
        
        path.pop()          # 3. 撤销选择（回溯的精髓！清理现场）
        visited.remove(num)
```
