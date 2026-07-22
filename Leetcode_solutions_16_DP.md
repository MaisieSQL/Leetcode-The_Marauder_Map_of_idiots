# 💡 动态规划（Dynamic Programming）通关指南与经典路线图

动态规划（Dynamic Programming，简称 DP）是算法面试和刷题中最核心、也是最容易产生畏难情绪的板块。DP 的核心思想是**“拆分子问题、记录中间状态、避免重复计算”**。

---

## 🛠️ 动态规划通关解题五步法（DP 破局公式）

无论做哪一道 DP 题目，都可以套用以下标准五步分析法：

1. **确定 dp 数组及其下标的含义**：明确 `dp[i]` 或 `dp[i][j]` 到底代表什么。
2. **确定递推公式 / 状态转移方程**：分析 `dp[i]` 如何由之前的状态推导出来。
3. **dp 数组如何初始化**：设定基础 base cases（如 `dp[0]`），避免边界越界和推导错误。
4. **确定遍历顺序**：从前向后、从后向前，还是先物品后背包？
5. **举例推导 dp 数组**：手动模拟，打印/画表核验推导结果。

---

## 🚀 动态规划经典题目推荐路线图

### 🟢 第一阶：入门与基础递推（体会状态转移）
> **核心**：理解当前状态只依赖于前几个状态。

1. **LeetCode 70. 爬楼梯 (Climbing Stairs)** `[NeetCode / Blind 75]`
   - **核心考点**：最基础的一维 DP。
   - **状态转移**：`dp[i] = dp[i-1] + dp[i-2]`（本质是斐波那契数列）。

2. **LeetCode 746. 使用最小花费爬楼梯 (Min Cost Climbing Stairs)**
   - **核心考点**：在一维递推中加入成本选择。
   - **状态转移**：`dp[i] = min(dp[i-1] + cost[i-1], dp[i-2] + cost[i-2])`。

3. **LeetCode 62. 不同路径 (Unique Paths)**
   - **核心考点**：二维网格 DP 入门，只能向下或向右移动。
   - **状态转移**：`dp[i][j] = dp[i-1][j] + dp[i][j-1]`。

---

### 🟡 第二阶：打家劫舍与序列决策问题（选择与状态转换）
> **核心**：选还是不选？包含状态转换与约束逻辑。

1. **LeetCode 198. 打家劫舍 (House Robber)** `[NeetCode / Blind 75]`
   - **核心考点**：相邻房屋不能连偷（偷 vs 不偷）。
   - **状态转移**：`dp[i] = max(dp[i-1], dp[i-2] + nums[i])`。

2. **LeetCode 213. 打家劫舍 II (House Robber II)** `[NeetCode / Blind 75]`
   - **核心考点**：房屋首尾相连成环。
   - **解题思路**：拆分为两个一维问题（偷第一家不偷最后一家 vs 偷最后一家不偷第一家）。

3. **LeetCode 91. 解码方法 (Decode Ways)** `[NeetCode / Blind 75]`
   - **核心考点**：序列拆分决策（单字符解码 vs 双字符组合解码）。
   - **解题思路**：结合前 1 位与前 2 位字符的合法性，判定是否将累加方案转移至 `dp[i]`。

4. **LeetCode 152. 乘积最大子数组 (Maximum Product Subarray)** `[NeetCode / Blind 75]`
   - **核心考点**：状态机/双状态维持（处理负数乘负数变正数的情况）。
   - **解题思路**：同时维护 `max_dp[i]` 和 `min_dp[i]` 两个状态。

---

### 🔴 第三阶：背包问题与模型转化（DP 的重难点）
> **核心**：限制容量下的收益最大化 / 组合数 / 拼装填充。

1. **0-1 背包系列**
   - **LeetCode 416. 分割等和子集 (Partition Equal Subset Sum)**
     - **核心考点**：判断能否凑出目标和 `sum / 2`，每个元素只能用一次。

2. **完全背包系列**
   - **LeetCode 322. 零钱兑换 (Coin Change)** `[NeetCode / Blind 75]`
     - **核心考点**：硬币数量无限，求凑成总金额所需的最少硬币数。
     - **状态转移**：`dp[i] = min(dp[i], dp[i - coin] + 1)`。
   - **LeetCode 518. 零钱兑换 II (Coin Change II)**
     - **核心考点**：求凑成总金额的组合数（注意遍历外层硬币、内层金额的顺序）。

3. **背包变体（拼接问题）**
   - **LeetCode 139. 单词拆分 (Word Break)** `[NeetCode / Blind 75]`
     - **核心考点**：字符串拼接转化为完全背包模型。
     - **解题思路**：将单词库看作物品，字符串看作背包容量，`dp[i]` 表示前 `i` 个字符能否被组合凑出。

---

### 🟣 第四阶：子序列与字符串/区间 DP（高频面霸题）
> **核心**：双指针/双序列/区间延伸，关注字符匹配与连续性。

1. **LeetCode 300. 最长递增子序列 (LIS - Longest Increasing Subsequence)** `[NeetCode / Blind 75]`
   - **核心考点**：一维状态嵌套循环，时间复杂度 $O(n^2)$（结合二分查找可优至 $O(n \log n)$）。

2. **LeetCode 53. 最大子数组和 (Maximum Subarray)**
   - **核心考点**：连续子数组最大和。
   - **状态转移**：`dp[i] = max(nums[i], dp[i-1] + nums[i])`。

3. **LeetCode 1143. 最长公共子序列 (LCS - Longest Common Subsequence)**
   - **核心考点**：二维双字符串匹配，看 `text1[i-1] == text2[j-1]` 是否成立。

4. **LeetCode 5. 最长回文子串 (Longest Palindromic Substring)** `[NeetCode / Blind 75]`
   - **核心考点**：区间 DP / 双指针扩展。
   - **状态转移**：`dp[i][j]` 表示从 `i` 到 `j` 的子串是否回文，当 `s[i] == s[j]` 且 `dp[i+1][j-1]` 为真时成立。

5. **LeetCode 647. 回文子串 (Palindromic Substrings)** `[NeetCode / Blind 75]`
   - **核心考点**：区间 DP 状态统计。
   - **解题思路**：基于最长回文子串的状态转移，累计所有 `dp[i][j] == true` 的区间数量。

6. **LeetCode 72. 编辑距离 (Edit Distance)**
   - **核心考点**：字符串 DP 的经典难点，涉及插入、删除、替换三种操作。

---

# 🪜 LeetCode 70. 爬楼梯 (Climbing Stairs)

> **题目链接**：[LeetCode 70. 爬楼梯](https://leetcode.cn/problems/climbing-stairs/) `[NeetCode / Blind 75]`  
> **难度**：🟢 简单（DP 破冰入门题）

## 📌 题目描述

假设你正在爬楼梯。需要 $n$ 阶你才能到达楼顶。

每次你可以爬 `1` 或 `2` 个台阶。你有多少种不同的方法可以爬到楼顶呢？

## 💡 状态推导：DP 破局五步法

按照动态规划的标准解题五步法进行分析：

### 1. 确定 dp 数组及其下标的含义
* **`dp[i]` 的含义**：到达第 `i` 阶台阶一共有 `dp[i]` 种不同的方法。

### 2. 确定递推公式 / 状态转移方程
* 到达第 `i` 阶台阶，只有两种可能：
  1. 从第 `i - 1` 阶台阶向上跨 **1** 步到达；
  2. 从第 `i - 2` 阶台阶向上跨 **2** 步到达。
* 因此，到达第 `i` 阶的方法数等于到达这两阶的方法数之和：
  
  $$\text{dp}[i] = \text{dp}[i - 1] + \text{dp}[i - 2]$$

### 3. dp 数组如何初始化 (Base Cases)
* **`dp[1] = 1`**：爬到第 1 阶只有 1 种方法（跨 1 步）。
* **`dp[2] = 2`**：爬到第 2 阶有 2 种方法（跨 1 步 + 跨 1 步，或者直接跨 2 步）。

### 4. 确定遍历顺序
* 从递推公式可以看出，`dp[i]` 依赖于 `dp[i-1]` 和 `dp[i-2]`，所以遍历顺序是从左到右，从前向后（即从 `i = 3` 遍历到 `n`）。

### 5. 举例推导 dp 数组 (以 $n = 5$ 为例)

| 阶数 $i$ | 1 | 2 | 3 | 4 | 5 |
| :--- | :---: | :---: | :---: | :---: | :---: |
| **`dp[i]`** | 1 | 2 | 3 | 5 | 8 |

* 本质上就是**斐波那契数列**（从 index 1 开始）。

## 💻 代码实现

### 方法一：标准 DP（空间复杂度 $O(n)$）

```python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2:
            return n
        
        # 1. 创建 dp 数组
        dp = [0] * (n + 1)
        
        # 2. 初始化 base case
        dp[1] = 1
        dp[2] = 2
        
        # 3. 递推计算
        for i in range(3, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]
            
        return dp[n]
```

---

# 🪜 LeetCode 746. 使用最小花费爬楼梯 (Min Cost Climbing Stairs)

> **题目链接**：[LeetCode 746. 使用最小花费爬楼梯](https://leetcode.cn/problems/min-cost-climbing-stairs/)  
> **难度**：🟢 简单（一维决策 DP 基础题）

## 📌 题目描述

给你一个整数数组 `cost` ，其中 `cost[i]` 是从楼梯第 `i` 个台阶向上爬需要支付的费用。一旦支付了费用，你可以选择向上爬 **1** 个或 **2** 个台阶。

你可以选择从下标为 `0` 或下标为 `1` 的台阶开始爬楼梯。

请你计算并返回到达楼梯顶部的最低花费。

> **注意**：楼顶（Top）指的是 `cost` 数组之外的下一个位置（即下标为 `n` 的位置，其中 `n = cost.length`）。

## 💡 状态推导：DP 破局五步法

### 🎟️ 关键概念：门票比喻
* **`dp[i]`**：你一路走来，**脚刚刚踏上第 `i` 级台阶时**，手上累积花掉的最少费用（此时还没支付第 `i` 级台阶的费用）。
* **`cost[i]`**：站在第 `i` 级台阶上，**准备离开它向上跳时**，看守员向你收取的“出门门票”。

### 1. 确定 dp 数组及其下标的含义
* **`dp[i]` 的含义**：到达第 `i` 阶台阶所需的**最小总花费**。
* **目标**：求到达楼顶（即 `dp[n]`，其中 `n = cost.length`）的最小花费。

### 2. 确定递推公式 / 状态转移方程
到达第 `i` 阶台阶只有两条路径：
1. **从第 `i - 1` 阶跨 1 步过来**：到达 `i - 1` 花了 `dp[i-1]`，离开 `i - 1` 需要交门票 `cost[i-1]`，总花费为 `dp[i-1] + cost[i-1]`。
2. **从第 `i - 2` 阶跨 2 步过来**：到达 `i - 2` 花了 `dp[i-2]`，离开 `i - 2` 需要交门票 `cost[i-2]`，总花费为 `dp[i-2] + cost[i-2]`。

取二者最小值：
$$\text{dp}[i] = \min(\text{dp}[i - 1] + \text{cost}[i - 1], \;\text{dp}[i - 2] + \text{cost}[i - 2])$$

### 3. dp 数组如何初始化 (Base Cases)
* 题目说明可以免费直接站在台阶 0 或台阶 1 上：
  * **`dp[0] = 0`**：站在第 0 阶台阶，累计花费为 0。
  * **`dp[1] = 0`**：站在第 1 阶台阶，累计花费为 0。

### 4. 确定遍历顺序
* `dp[i]` 依赖于 `dp[i-1]` 和 `dp[i-2]`，因此从前向后遍历（从 `i = 2` 到 `n`）。

### 5. 举例推导 dp 数组 (以 Example 1: `cost = [10, 15, 20]` 为例)

* 数组长度 $n = 3$，楼顶为 $i = 3$。

| 下标 $i$ | 0 | 1 | 2 | 3 (楼顶 Top) |
| :--- | :---: | :---: | :---: | :---: |
| **`cost[i]`** | 10 | 15 | 20 | - |
| **`dp[i]`** | 0 | 0 | $\min(0+10, 0+15) = \mathbf{10}$ | $\min(0+15, 10+20) = \mathbf{15}$ |

* **输出答案**：`dp[3] = 15`。

## 💻 代码实现

### 方法一：标准 DP（空间复杂度 $O(n)$）

```python
class Solution:
    def minCostClimbingStairs(self, cost: list[int]) -> int:
        n = len(cost)
        # 1. 创建 dp 数组，长度为 n + 1（表示 0 到 n 阶）
        dp = [0] * (n + 1)
        
        # 2. 初始化 base case
        dp[0] = 0
        dp[1] = 0
        
        # 3. 递推计算
        for i in range(2, n + 1):
            dp[i] = min(dp[i - 1] + cost[i - 1], dp[i - 2] + cost[i - 2])
            
        return dp[n]
```

### 方法二：滚动变量优化（空间复杂度 $O(1)$）

[cite_start]由于 `dp[i]` 只与前两个状态有关 [cite: 68][cite_start]，我们只需维护两个局部变量代表 `dp[i-2]` 和 `dp[i-1]` ：

```python
class Solution:
    def minCostClimbingStairs(self, cost: list[int]) -> int:
        # dp0 代表 dp[i-2]，dp1 代表 dp[i-1]
        dp0, dp1 = 0, 0
        
        for i in range(2, len(cost) + 1):
            next_dp = min(dp1 + cost[i - 1], dp0 + cost[i - 2])
            dp0 = dp1
            dp1 = next_dp
            
        return dp1
```

### ⏱️ 复杂度分析

| 维度 | 时间复杂度 | 空间复杂度 | 说明 |
| :--- | :---: | :---: | :--- |
| **标准 DP** | $O(n)$ | $O(n)$ | 需要一个长度为 $n+1$ 的 dp 数组 |
| **滚动变量优化** | $O(n)$ | $O(1)$ | 仅使用 2 个变量滚动记录前两阶状态 |

---

# 🪙 LeetCode 322. 零钱兑换 (Coin Change)

> [cite_start]**题目链接**：[LeetCode 322. 零钱兑换](https://leetcode.cn/problems/coin-change/) `[NeetCode / Blind 75]` [cite: 4, 28]  
> [cite_start]**难度**：🟡 中等（完全背包模型代表作） 

---

## 📌 题目描述

[cite_start]给你一个整数数组 `coins` ，表示不同面额的硬币；以及一个整数 `amount` ，表示总金额 [cite: 4]。

[cite_start]计算并返回可以凑成总金额所需的 **最少硬币个数** [cite: 4, 7, 8][cite_start]。如果没有任何一种硬币组合能凑成总金额，返回 `-1` [cite: 4]。

[cite_start]你可以认为每种硬币的数量是**无限的** [cite: 4]。

---

## 🎒 模型转换：为什么是完全背包？

[cite_start]我们可以把“零钱兑换”直接映射到背包问题 [cite: 4]：
* [cite_start]**背包容量**：目标金额 `amount` [cite: 4, 28]。
* [cite_start]**物品**：面额不同的硬币 `coins` [cite: 4]。
* [cite_start]**物品数量无限**：每种硬币可以重复无限次挑选 $\rightarrow$ **完全背包** [cite: 4, 28]。
* [cite_start]**求解目标**：填满容量为 `amount` 的背包所需的**最少物品数** [cite: 4, 7, 8]。

## [cite_start]💡 状态推导：DP 破局五步法 [cite: 1]

### [cite_start]1. 确定 dp 数组及其下标的含义 [cite: 1]
* [cite_start]**`dp[i]` 的含义**：凑齐金额 `i` 所需要的最少硬币数量 [cite: 5, 8, 9]。
* [cite_start]**最终答案**：`dp[amount]` [cite: 4, 28]。

### [cite_start]2. 确定递推公式 / 状态转移方程 [cite: 1]
[cite_start]假设我们当前要凑齐金额 `i`，遍历手里拥有的每一张硬币 `coin` [cite: 5, 9]：
* [cite_start]如果我们选择了这枚面值为 `coin` 的硬币，问题就转化成了“凑齐金额 `i - coin` 需要的最少硬币数，再加上当前这 1 枚硬币” [cite: 5, 9]。
* [cite_start]状态转移为：`dp[i - coin] + 1` [cite: 5, 9]。
* [cite_start]由于我们想找**最少**硬币数，对所有可能的硬币取最小值 [cite: 5, 7, 9]：
  
  [cite_start]$$\text{dp}[i] = \min(\text{dp}[i], \;\text{dp}[i - \text{coin}] + 1)$$ [cite: 5, 9, 28]

### [cite_start]3. dp 数组如何初始化 (Base Cases) [cite: 1]
* **`dp[0] = 0`**：凑齐金额 0 需要 0 枚硬币。
* **其他位置 `dp[i]` 初始化为无穷大（如 `amount + 1` 或 `float('inf')`）**：
  * [cite_start]因为转移方程求的是 $\min()$，如果初始化为 0，最小值会被覆盖掉 [cite: 7, 9]。
  * 设置为 `amount + 1` 是因为就算全用面额最小的 1 元硬币，最多也只需要 `amount` 枚，所以 `amount + 1` 相当于逻辑上的“正无穷”。

### [cite_start]4. 确定遍历顺序 [cite: 1]
* **求最值问题**：完全背包的“外层循环遍历金额、内层遍历硬币”或“外层遍历硬币、内层遍历金额”都可以。
* [cite_start]通常推荐**外层循环遍历金额 `i`（从 1 到 `amount`），内层循环遍历硬币 `coin`**：符合由浅入深的直觉 [cite: 25]。

### [cite_start]5. 举例推导 dp 数组 (以 `coins = [1, 2, 5], amount = 11` 为例) [cite: 1]

初始化：`dp = [0, inf, inf, inf, inf, inf, inf, inf, inf, inf, inf, inf]`

| 金额 $i$ | 算式推导 $\min(\dots)$ | `dp[i]` 最终值 |
| :--- | :--- | :---: |
| **0** | Base Case | **0** |
| **1** | $\min(\text{dp}[0] + 1) = 0 + 1$ | **1** |
| **2** | $\min(\text{dp}[1] + 1, \text{dp}[0] + 1) = \min(2, 1)$ | **1** |
| **3** | $\min(\text{dp}[2] + 1, \text{dp}[1] + 1) = \min(2, 2)$ | **2** |
| **4** | $\min(\text{dp}[3] + 1, \text{dp}[2] + 1) = \min(3, 2)$ | **2** |
| **5** | $\min(\text{dp}[4]+1, \text{dp}[3]+1, \text{dp}[0]+1) = \min(3, 3, 1)$ | **1** |
| ... | ... | ... |
| **11** | $\min(\text{dp}[10]+1, \text{dp}[9]+1, \text{dp}[6]+1) = \min(3, 3, 2+1)$ | **3** (即 $5 + 5 + 1$) |

## 💻 代码实现

### 标准 DP 实现 (Python)

```python
class Solution:
    def coinChange(self, coins: list[int], amount: int) -> int:
        # 1. 初始化 dp 数组，初始值为 amount + 1 (代表无穷大)
        dp = [amount + 1] * (amount + 1)
        
        # 2. Base case
        dp[0] = 0
        
        # 3. 递推填表：外层金额，内层硬币
        for i in range(1, amount + 1):
            for coin in coins:
                if i - coin >= 0: # 背包容量能够装下当前硬币
                    dp[i] = min(dp[i], dp[i - coin] + 1)
                    
        # 4. 判断结果：如果 dp[amount] 仍为初值，说明无法凑齐
        return dp[amount] if dp[amount] <= amount else -1
```

---
