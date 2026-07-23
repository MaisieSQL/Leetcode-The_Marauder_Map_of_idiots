# 贪心排序与区间类问题总结

## 一、 区间类问题的两大核心类别与排序流派

根据具体的处理方式与目标，区间类问题主要可以分为以下两大核心排序与处理流派：

* **按结束时间升序排序（End Time Sorting）流派**
  * **适用场景**：“无重叠区间”（Non-overlapping Intervals）、“用最少数量的箭引爆气球”（Minimum Number of Arrows to Burst Balloons）等。
  * **核心逻辑**：优先选择结束时间最早的活动，这样可以为后面的活动留出尽可能多的物理空间，从而实现局部最优到全局最多不重叠区间的转化。
* **按起始时间升序排序（Start Time Sorting）流派**
  * **适用场景**：“合并区间”（Merge Intervals）、“插入区间”（Insert Interval）、“会议室”（Meeting Schedule）等。
  * **核心逻辑**：将所有区间按照左端点从小到大排好序后，由于它们在数轴上有序展开，我们只需要依次向后扫描，并用一个当前的合并区间去和后续区间进行比对、融合即可。

## 二、 经典实战分类代表与解法概述

根据具体的处理方式与目标，这些类别在 LeetCode 题目中对应着不同的解法：

* **合并与插入类**（如 *Merge Intervals*, *Insert Interval*）
  * **特点**：通常采用 **左端点升序排序**。
  * **解法**：通过比较相邻区间的左右边界来决定是否融合或插入新区间。
* **覆盖与调度类**（如 *Non-overlapping Intervals*, *Minimum Number of Arrows to Burst Balloons*, *Meeting Schedule*）
  * **特点**：通常采用 **右端点升序排序** 或区间交集端点覆盖策略。
  * **解法**：利用贪心保证每个保留的区间尽早结束或尽可能覆盖更多目标。

## 三、 经典实战题目清单详细拆解

* **1. 合并区间 (Merge Intervals / LeetCode 56)**
  * **解法**：按照区间 **左端点升序排序**。遍历时，若当前区间的左端点 $\le$ 已经合并的最后一个区间的右端点，说明发生重叠，直接更新右端点（取两者最大值）；否则，将当前区间作为一个全新的独立区间加入结果集。
* **2. 插入区间 (Insert Interval / LeetCode 57)**
  * **解法**：可以先将新区间放入列表，或者直接利用已有区间有序的特性分三步走：先添加所有在新区间左侧且不重叠的区间 $\rightarrow$ 持续合并所有与新区间有重叠的区间 $\rightarrow$ 添加所有右侧剩余的区间。
* **3. 无重叠区间 (Non-overlapping Intervals / LeetCode 435)**
  * **解法**：按照区间 **右端点升序排序**。用贪心策略保证每个保留的区间尽早结束，从而移除最少数量的重叠区间。
* **4. 会议室 / 用最少数量的箭引爆气球 (Meeting Schedule / LeetCode 452)**
  * **解法**：本质上都是区间交集与端点覆盖问题，利用“结束时间排序 + 贪心扫描”可以在 $O(N \log N)$ 的时间复杂度内完美解决。

# 区间问题与贪心算法总结

## 一、 区间问题是否都使用贪心算法？

答案是：**不一定。**

* 虽然“排序 + 贪心”是解决许多区间调度、覆盖、合并问题的利器（因为预排序能消除混乱、建立秩序，揭示隐藏的决策单调性），但区间类问题同样会采用其他算法。
* 例如，涉及复杂状态转移、子问题重叠且无法简单通过局部最优推导全局最优的区间问题，往往需要使用**动态规划（Dynamic Programming）**来求解（例如矩阵连乘、区间DP等）。

## 二、 “排序 + 贪心”区间问题的解题模版

在处理常见的区间调度与合并问题时，通常遵循**“先排序、后扫描（贪心决策）”**的通用范式。以下是两大核心流派的代码模版：

### 1. 范式一：按结束时间排序（End Time Sorting）
* **适用场景**：无重叠区间（Non-overlapping Intervals）、用最少数量的箭引爆气球（Minimum Number of Arrows to Burst Balloons）等覆盖与调度类问题。
* **核心思想**：优先选择结束时间最早的活动，为后面的活动留出尽可能多的物理空间。

```python
def interval_schedule_by_end(intervals):
    if not intervals:
        return 0
    
    # 1. 核心第一步：按照区间右端点（结束时间）升序排序
    intervals.sort(key=lambda x: x[1])
    
    count = 0
    current_end = float('-inf')
    
    # 2. 核心第二步：线性扫描进行贪心决策
    for interval in intervals:
        start, end = interval[0], interval[1]
        
        # 如果当前区间的起始时间 >= 上一个保留区间的结束时间，说明不冲突
        if start >= current_end:
            count += 1
            current_end = end  # 更新当前边界
            
    return count
```

### 2. 范式二：按起始时间排序（Start Time Sorting）
* **适用场景**：合并区间（Merge Intervals）、插入区间（Insert Interval）、会议室（Meeting Schedule）等合并与插入类问题。

* **核心思想**：将区间按左端点从小到大排好序后在数轴上有序展开，通过维护一个当前的合并区间依次向后比对和融合。
