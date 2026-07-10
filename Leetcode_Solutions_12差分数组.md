# 📂 LeetCode 差分数组 (Difference Array) 全景题号清单

### 🟢 一、 一维标准差分（核心必刷）
> **特点：** 差分数组最纯粹的板子题，用来培养“区间加减、单遍还原”的条件反射。

*   **370** - 区间加法 (Range Addition) *(注：此题为 Plus 会员题，1109 题是它的完美免费替代版)*
*   **1109** - 航班预订统计 (Corporate Flight Bookings)
*   **1094** - 拼车 (Car Pooling)
*   **1854** - 人口最多的年份 (Maximum Population Year)

### 🟡 二、 扫描线与事件型差分（区间重叠/会议室）
> **特点：** 坐标范围通常很大或不连续，无法直接开固定大小的数组。需要用 **`Map/Dict`（哈希表）** 或 **将区间拆成 [进入, 离开] 事件排序** 来做离散化差分。

*   **252** - 会议室 (Meeting Rooms)
*   **253** - 会议室 II (Meeting Rooms II)
*   **731** - 我的日程安排表 II (My Calendar II)
*   **732** - 我的日程安排表 III (My Calendar III)
*   **218** - 天际线问题 (The Skyline Problem) *(高阶扫描线 + 差分)*

### 🟠 三、 二维/矩阵差分（Grid 变形）
> **特点：** 差分从“一条线”变成“一个网格（二维矩阵）”，需要在矩形的四个关键角进行加减标记。

*   **2132** - 用邮票贴满网格图 (Stamp Grid)
*   **2536** - 子矩阵元素加 1 (Increment Submatrices by One)

### 🔴 四、 高阶缝合题（二分答案 + 贪心 + 差分）
> **特点：** 大厂 Hard 面试杀招。不能一眼看出是差分，必须通过二分或贪心将问题转化后，在内层使用差分数组作为**优化时间复杂度的辅助工具**。

*   **1526** - 让目标数组形成的最少行动次数 (Minimum Number of Increments on Subarrays to Form a Target Array)
*   **1674** - 使数组互补的最少操作次数 (Minimum Moves to Make Array Complementary)
*   **2251** - 花期内花的数目 (Number of Flowers in Full Bloom)
*   **2528** - 最大化城市的最小电量 (Maximize the Minimum Game Power)
*   **2772** - 使数组中的所有元素都等于零 (Apply Operations to Make All Array Elements Equal to Zero)

---

# 母题📂 LeetCode 370. 区间加法 (Range Addition) ── 宗师级模板题解

## 📖 1. 题目描述 (人话直译版)
假设你有一个长度为 `length` 的数组，初始时**所有元素都为 0**。
现在给你一个操作二维数组 `updates`，其中每个操作表示为：`updates[i] = [start, end, inc]`。
这个操作的意思是：将数组中从索引 `start` 到 `end`（**包含两端**）的所有元素都加上 `inc`。

请输出执行完所有 `updates` 操作后的**最终数组**。

### 示例：
*   **输入:** `length = 5`, `updates = [[1, 3, 2], [2, 4, 3], [0, 2, -2]]`
*   **输出:** `[-2, 0, 3, 5, 3]`

## 💡 2. 核心解题思路：差分金字塔
如果我们用传统的暴力循环（`for` 遍历 `start` 到 `end`），每次操作的时间复杂度是 $O(N)$，总时间复杂度会飙升到 $O(M \times N)$（$M$ 为操作次数），直接超时。

作为差分数组的鼻祖题，本题利用了**“起点记账、终点+1销账”**的终极奥义：
1. **快速标记：** 面对一个指令 `[start, end, inc]`，我们不需要修改区间内的每一个数，只需要在差分数组 `diff` 的两头做两个 $O(1)$ 的记号：
   * `diff[start] += inc` （红利开始）
   * `diff[end + 1] -= inc` （红利期结束，吐出增量）
2. **前缀和还原：** 所有的操作记录完毕后，从左到右做一次**前缀和（累加）**，就能像推倒多米诺骨牌一样，一气呵成还原出最终的物理数组。总时间复杂度完美降到 $O(M + N)$。

## 💻 3. 核心代码实现

### Python3 实现
```python
class Solution:
    def getModifiedArray(self, length: int, updates: list[list[int]]) -> list[int]:
        # 1. 初始化差分数组，大小和原数组一致
        diff = [0] * length
        
        # 2. 遍历所有指令，进行 O(1) 的轻量级标记
        for start, end, inc in updates:
            diff[start] += inc
            # 注意：只有当 end + 1 还在数组范围内时，才需要做减法减去红利
            if end + 1 < length:
                diff[end + 1] -= inc
                
        # 3. 从左到右推骨牌（求前缀和），直接在 diff 数组上原地复原出答案
        for i in range(1, length):
            diff[i] = diff[i - 1] + diff[i]
            
        return diff
```

## ⏱️ 4. 复杂度分析

*   **时间复杂度：$O(M + N)$**
    *   **构建标记：** 遍历了 $M$ 次 `updates` 指令，每次操作都是 $O(1)$，耗时 $O(M)$。
    *   **恢复原样：** 遍历了一遍长度为 $N$ 的数组求前缀和，耗时 $O(N)$。
    *   相比暴力的 $O(M \times N)$，这是完美的**降维打击**。
*   **空间复杂度：$O(1)$ 或 $O(N)$**
    *   如果我们直接利用返回的答案数组（如代码中的 `diff`）原地进行前缀和累加，除了系统必要的返回空间外，没有开辟任何额外的辅助空间，纯算法层面的辅助空间复杂度为 $O(1)$。

---

# 📂 LeetCode 1109. 航班预订统计 (Corporate Flight Bookings) ── 一维标准差分实战

## 📖 1. 题目描述
这里有 `n` 个航班，标签从 `1` 到 `n`（**注意：索引是从 1 开始的**）。
给你一个预订记录列表 `bookings`，其中 `bookings[i] = [first, last, seats]`。
该操作的意思是：从航班 `first` 到航班 `last`（**包含 `first` 和 `last`**）的每个航班上，都预订了 `seats` 个座位。

请返回一个长度为 `n` 的数组，表示每个航班预订的座位总数。

### 示例：
*   **输入：** `n = 5`, `bookings = [[1, 2, 10], [2, 3, 20], [2, 5, 25]]`
*   **输出：** `[10, 55, 45, 25, 25]`

## 💡 2. 破局关键：识破“从 1 开始”的索引陷阱
这道题的代码骨架与宗师题 370 题一模一样，但唯一的区别就在于它的航班号是从 `1` 到 `n`。
为了让代码写起来最爽、最不容易出错，我们可以使用 **空间微调，强行对齐** 的绝招：

*   直接把差分数组的大小开成 **`n + 2`**。
*   这样，航班 `1` 直接对应 `diff[1]`，航班 `5` 直接对应 `diff[5]`。
*   当我们要执行“终点 $+1$ 销账”（即 `last + 1`）时，哪怕 `last` 是最后一个航班 `n`，`last + 1 = n + 1` 也在我们 `n + 2` 的安全范围内，绝对不会越界！
*   最后返回答案时，直接切片取 `diff[1 : n + 1]` 即可。

## 💻 3. 核心代码实现

### Python3 实现
```python
class Solution:
    def corpFlightBookings(self, bookings: list[list[int]], n: int) -> list[int]:
        # 开辟 n + 2 的大小，完美避开从 1 开始的边界乱象
        diff = [0] * (n + 2)
        
        # 1. 鼻祖流派：起点记账，终点 + 1 销账
        for first, last, seats in bookings:
            diff[first] += seats
            diff[last + 1] -= seats
            
        # 2. 从左到右推骨牌（求前缀和）
        for i in range(1, n + 1):
            diff[i] = diff[i - 1] + diff[i]
            
        # 3. 丢弃第 0 位和最后一位，只取 1 到 n 的真实航班数据
        return diff[1:n + 1]
```

## ⏱️ 4. 复杂度分析

*   **Time (时间复杂度)：$O(M + N)$** ── 其中 $M$ 是 `bookings` 的记录数，$N$ 是航班数 `n`。打标记耗时 $O(M)$，求和还原耗时 $O(N)$。
*   **Space (空间复杂度)：$O(N)$** ── 需要一个长度为 $N + 2$ 的差分数组来辅助计算。

### 💡 为什么是 `n + 2` 而不是 `n + 1`？
这是一个极其经典的细节：

*   如果最大航班号是 `n`，那么 `end` 最大可以等于 `n`。
*   差分数组的公式要求我们操作 `end + 1`，也就是说我们最大会访问到 `n + 1` 的位置。
*   在 Python 中，如果你想让数组拥有 `n + 1` 这个索引，那么整个数组的长度就必须是 **`n + 2`**（因为索引是从 0 开始算的，长度为 `n + 2` 的数组，最大下标刚好是 `n + 1`）。

改完 `seats = [0] * (n + 2)` 之后，你直接去提交，就能看到绿色的 **Accepted** 了！这就是鼻祖模板的威力，只要把地基（空间大小）垫结实了，里面的核心逻辑雷打不动。

---

# 📂 LeetCode 1094. 拼车 (Car Pooling) ── 区间边界微调实战

## 📖 1. 题目描述
车上最初是空的，总共有 `capacity` 个座位。
给你一个数组 `trips`，其中 `trips[i] = [num_passengers, from, to]`。
这表示第 `i` 趟行程有 `num_passengers` 个人从 `from` 站上车，在 `to` 站下车。

请问：这辆车能不能一路上把所有人顺利接送完？（即：在任何时刻车上的人数都不能超过 `capacity`）。

### 示例：
*   **输入：** `trips = [[2, 1, 5], [3, 3, 7]]`, `capacity = 4`
*   **om 输出：** `false` （因为在第 3 站到第 5 站之间，车上会有 2+3=5 个人，超过了 4 个座位）

---

## 💡 2. 破局关键：左闭右开区间 `[from, to)`
这道题看似和前两题一样，但隐藏着一个关于“区间定义”的微妙变化：
*   在 `from` 站，乘客上车，车上人数**增加**。
*   在 `to` 站，这批乘客下车。这意味着**在 `to` 站这一时刻，车上已经不包含这批人了**。

**关键点：** 这属于典型的**左闭右开区间** `[from, to)`。因此，我们在销账时**不需要 `end + 1`**！
*   起点记账：`diff[from] += num`
*   终点销账：`diff[to] -= num` （因为在 `to` 站这一瞬间，他们刚好离开）

题目明确说明了车站范围在 `0` 到 `1000` 之间，因此我们可以直接开一个固定大小为 `1001` 的差分数组。

---

## 💻 3. 核心代码实现

### Python3 实现
```python
class Solution:
    def carPooling(self, trips: list[list[int]], capacity: int) -> bool:
        # 题目给出站点范围在 0-1000 之间，开辟 1001 大小的固定数组
        diff = [0] * 1001
        
        # 1. 处理左闭右开区间 [from, to)
        for num, start, end in trips:
            diff[start] += num
            diff[end] -= num  # 人下车的那一刻就离开了，所以直接在 end 处销账
            
        # 2. 从左到右推骨牌（求前缀和），同时动态监控是否超载
        current_passengers = 0
        for i in range(1001):
            current_passengers += diff[i]
            if current_passengers > capacity:
                return False  # 一旦超过座位数，直接返回失败
                
        return True
```

---

