# 💡 双指针算法题型全景分类总结

双指针的核心思想：**通过控制两个指针的移动节奏，利用数据的“单调性”或“对称性”来减少不必要的搜索，将 $O(n^2)$ 时间复杂度优化至 $O(n)$。**

---

## 1. 相向双指针（两边向中间夹击）
* **指针方向：** 一左一右，相向而行 (`left ->  <- right`)
* **适用场景：** 数据有序（利用单调性）、求对称结构、或两端碰撞寻求最优解。
* **核心逻辑：** 每次根据当前两端状态，**确定性地排除掉一边**。

### 📌 经典代表题：
* **有序数组求和：** LeetCode 167. 两数之和 II (Two Sum II)
  * *逻辑：* 如果 `s[left] + s[right] > target`，说明右边的数太大了，只能 `right--`。
* **盛水/接水问题：** LeetCode 11. 盛最多水的容器 (Container With Most Water)
  * *逻辑：* 容器高度由短板决定，因此每次只能移动较短的那一根柱子。
* **三数之和：** LeetCode 15. 3Sum
  * *逻辑：* 先排序，固定第一个数，剩下的两个数用相向双指针夹击。
* **验证回文：** LeetCode 125. 验证回文串 (Valid Palindrome)

---

## 2. 同向双指针 A：快慢指针（追逐 / 原地修改）
* **指针方向：** 两个指针同向移动，但**步频不同**（比如一个一次走两步，一个走一步）或**触发条件不同**。
* **适用场景：** 链表找环、链表中点、数组原地去重或过滤。

### 📌 经典代表题：
* **链表找环 / 找入口：** LeetCode 141. 环形链表 (Has Cycle) / LeetCode 142
  * *逻辑：* 快指针一次走 2 步，慢指针一次走 1 步。如果有环，快指针必定会在环内追上慢指针（龟兔赛跑）。
* **寻找链表倒数第 K 个节点：** LeetCode 19. 删除链表的倒数第 N 个结点
  * *逻辑：* 快指针先走 K 步，然后快慢指针同步走。当快指针到头时，慢指针正好在目标位置。
* **数组原地去重 / 移除元素：** LeetCode 26. 删除有序数组中的重复项
  * *逻辑：* 快指针用于扫描新元素，慢指针用于记录有效数据的写入位置。

---

## 3. 同向双指针 B：滑动窗口（Sliding Window）
* **指针方向：** 两个指针同向移动，形成一个**动态可变大小的“区间” `[left, right]`**。
* **适用场景：** 连续子数组 / 子字符串的最值问题（如“最长...”、“最短...”、“包含...的最小子串”）。
* **核心逻辑：**
  1. `right` 不断右移**扩大窗口**，直到满足条件（或打破条件）。
  2. `left` 不断右移**缩小窗口**，以寻找最优解或重新满足条件。

### 📌 经典代表题：
* **无重复字符的最长子串：** LeetCode 3. Longest Substring Without Repeating Characters
* **长度最小的子数组：** LeetCode 209. Minimum Size Subarray Sum
* **最小覆盖子串：** LeetCode 76. Minimum Window Substring

---

## 4. 背向双指针（从内向外扩展）
* **指针方向：** 从同一个中心点（或相邻点）出发，向左右两边扩散 (`left <-  -> right`)。
* **适用场景：** 利用对称性寻找结构。

### 📌 经典代表题：
* **最长回文子串：** LeetCode 5. Longest Palindromic Substring
* **回文子串个数：** LeetCode 647. Palindromic Substrings

---

## 5. 双数组 / 多链表双指针（双路归并）
* **指针方向：** 指针分别位于**两个不同的数组或链表**上，各自同向移动。
* **适用场景：** 合并两个有序数据结构、计算交集/差集。

### 📌 经典代表题：
* **合并两个有序链表：** LeetCode 21. Merge Two Sorted Lists
* **合并两个有序数组：** LeetCode 88. Merge Sorted Array
* **两个数组的交集：** LeetCode 350. Intersection of Two Arrays II

---

## 📋 快速分类记忆卡片

| 题型分类 | 关键信号词 | 核心思维模型 |
| :--- | :--- | :--- |
| **相向双指针** | 有序数组、两数之和、盛水容器 | 利用单调性，一次排除一侧无用解 |
| **快慢指针** | 链表找环、倒数第 K 个、原地修改 | 速度差 / 步长差 |
| **滑动窗口** | 连续子数组、子字符串、最长/最短 | 右指针拉伸扩容，左指针收缩优化 |
| **背向扩展** | 回文串、镜像对称 | 选定轴心，向外校验对称性 |
| **双路归并** | 两个有序数组/链表 | 比较两队头，较小者先出列 |

---

# 🎯 背向双指针（中心扩散）刷题清单：Easy ➔ Medium ➔ Hard

背向双指针的核心逻辑：**选定对称轴/中心点，向左右两侧扩散 (`left <- -> right`)，利用对称性校验结构。**

---

## 1. Easy 基础入门

### 📌 LeetCode 125. Valid Palindrome (验证回文串)
* **题目链接：** [LeetCode 125](https://leetcode.com/problems/valid-palindrome/)
* **解题思路：** 清理非字母数字字符后，找到字符串中心（奇数长取正中间，偶数长取中间两个），用背向双指针向两边扩散校验。
* **训练目标：** 熟悉中心点的定义与边界控制。

---

## 2. Medium 核心主力（必刷重点）

### 📌 LeetCode 5. Longest Palindromic Substring (最长回文子串)
* **题目链接：** [LeetCode 5](https://leetcode.com/problems/longest-palindromic-substring/)
* **解题思路：** 遍历所有可能中心，分别以 `(i, i)`（奇数长）和 `(i, i+1)`（偶数长）展开扩散，实时更新最长回文串的边界。
* **训练目标：** 熟练手写背向双指针标准模版。

### 📌 LeetCode 647. Palindromic Substrings (回文子串个数)
* **题目链接：** [LeetCode 647](https://leetcode.com/problems/palindromic-substrings/)
* **解题思路：** 遍历 $2n-1$ 个中心点，只要 `s[left] == s[right]`，计数器就 $+1$，遇到不匹配立即停止扩散（高效剪枝）。
* **训练目标：** 掌握中心扩散过程中的累加机制。

### 📌 LeetCode 1750. Minimum Length of String After Deleting Similar Ends
* **题目链接：** [LeetCode 1750](https://leetcode.com/problems/minimum-length-of-string-after-deleting-similar-ends/)
* **解题思路：** 寻找两端删除相同字符后剩下的最短长度。可以尝试结合相向与背向扩散的双向思维。
* **训练目标：** 加深对区间对称收缩/扩展逻辑的理解。

---

## 3. Hard 进阶挑战

### 📌 LeetCode 214. Shortest Palindrome (最短回文串)
* **题目链接：** [LeetCode 214](https://leetcode.com/problems/shortest-palindrome/)
* **解题思路：** 寻找**从索引 0 开始的最长回文前缀**。从中心靠左的位置向外扩散，找左边界能触碰到 `0` 的最长回文串。
* **训练目标：** 学习在带“固定端点/前缀”约束下灵活变通中心扩展。

### 📌 LeetCode 336. Palindrome Pairs (回文对)
* **题目链接：** [LeetCode 336](https://leetcode.com/problems/palindrome-pairs/)
* **解题思路：** 结合哈希表，把单词拆解后将“判断前缀/后缀是否为回文”作为子模块（使用中心扩展），在字典里快速匹配另一半。
* **训练目标：** 将背向双指针作为子函数，嵌入到更复杂的复合检索逻辑中。

---

## 💡 背向双指针 Python 标准通用模板

```python
def expandFromCenter(s: str, left: int, right: int) -> int:
    """
    从中心向两边扩散，返回合法回文子串的长度
    """
    while left >= 0 and right < len(s) and s[left] == s[right]:
        left -= 1
        right += 1
    # 退出循环时，left 和 right 已经各多走了一步（处于无效位置）
    # 合法长度为：(right - 1) - (left + 1) + 1 = right - left - 1
    return right - left - 1

# 在主函数中遍历所有可能的中心点：
# 1. 奇数长度中心：expandFromCenter(s, i, i)
# 2. 偶数长度中心：expandFromCenter(s, i, i + 1)
```
