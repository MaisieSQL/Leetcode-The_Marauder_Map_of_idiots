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

