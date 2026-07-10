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

# 母题
# 📂 LeetCode 370. 区间加法 (Range Addition) ── 宗师级模板题解

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
