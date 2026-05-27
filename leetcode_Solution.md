# 05/08/2026 今天我们来做Array的题，利用双指针，Dict等办法
## 1. Two Sum
Given an array of integers nums and an integer target, return indices of the two numbers such that they add up to target. You may assume that each input would have exactly one solution, and you may not use the same element twice. You can return the answer in any order.

Example 1:
Input: nums = [2,7,11,15], target = 9
Output: [0,1]
Explanation: Because nums[0] + nums[1] == 9, we return [0, 1].

Example 2:
Input: nums = [3,2,4], target = 6
Output: [1,2]

Example 3:
Input: nums = [3,3], target = 6
Output: [0,1]
 
Constraints:

2 <= nums.length <= 104
-109 <= nums[i] <= 109
-109 <= target <= 109
Only one valid answer exists.
Follow-up: Can you come up with an algorithm that is less than O(n2) time complexity?

```Python
class Solution(object):
    def twoSum(self, nums, target):
        """
        :type nums: List[int]
        :type target: int
        :rtype: List[int]
        """
        mydict = {}
        for i in range(len(nums)):
            if target - nums[i] in mydict:
                return [mydict[target - nums[i]], i]
            mydict[nums[i]] = i
```
Note: 这里有一个时间换空间的概念

---

## LeetCode 3. Longest Substring Without Repeating Characters
Given a string s, find the length of the longest substring without repeating characters. A substring is a contiguous sequence of characters within a string.

Example 1:
Input: s = "abcabcbb"
Output: 3Explanation: The answer is "abc", with the length of 

Example 2:
Input: s = "bbbbb"
Output: 1

Explanation: The answer is "b", with the length of 1.

Example 3:
Input: s = "pwwkew"
Output: 3

Explanation: The answer is "wke", with the length of 3. Note that the answer must be a substring, "pwke" is a subsequence and not a substring.

```Python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        left = 0
        max_length = 0
        mydict = {}

        for i in range(len(s)):
            if s[i] in mydict and mydict[s[i]] >= left:
                left = mydict[s[i]] + 1
            current_len = i - left + 1
            if current_len > max_length:
                max_length = current_len
            mydict[s[i]] = i

        return max_length 
```
Note1: 这里为什么会有mydict[s[i]] >= left而不是mydict[s[i]] > left？
Answer: 这个问题问得极度刁钻，说明你已经抠到滑动窗口最核心的边界细节里去了！简单粗暴的回答是：为了防止 left 往回跳（倒退）。

咱们用一个最极端的例子：s = "abba"。拆解一下没有 >= 会发生什么：
走到第一个 a (i=0)：mydict = {'a': 0}, left = 0。
走到第一个 b (i=1)：mydict = {'a': 0, 'b': 1}, left = 0。
走到第二个 b (i=2)：
发现 b 在字典里了，且 mydict['b'] 是 1。
1 >= 0 成立，所以 left 跳到 1 + 1 = 2。

现在的窗口是 [b]，left = 2。此时 mydict = {'a': 0, 'b': 2}。

走到第二个 a (i=3)：
关键点来了！ 此时 a 确实在字典里（上次出现在位置 0）。
但是！我们的 left 已经在 2 了（窗口里只有 b）。
如果你没有 mydict['a'] >= left 这个判断：
代码会执行 left = mydict['a'] + 1，也就是 left = 0 + 1 = 1。
天呐！left 从 2 退回到 1 了！

为什么不能“倒退”？
如果 left 往回跳，意味着你把早就踢出窗口的脏数据（那个旧的 a）又给捡回来了。这会导致你的窗口里又出现了重复字符（窗口变成了 bba），逻辑彻底崩盘。

加上 >= left 的潜台词是：“虽然我在字典里见过你，但如果你在 left 左边，说明你已经被我之前的操作‘开除’出窗口了，你已经不是威胁了，我可以假装没看见你，继续往右走。”

总结
s[i] in mydict：确认这个字符曾经出现过。mydict[s[i]] >= left：确认这个字符现在还在我的有效窗口里。这个细节是面试中最容易掉进去的坑，你能敏锐地察觉到这个“等于号”，说明你的代码敏感度已经上来了！

Note2: 为什么刚才那个第三题，复杂度不是O(n^2)，而是O(n)?
Answer: 表面上看，你有一个 for 循环，里面还套了一个 if（或者别人解法里的 while），直觉上很容易觉得是 $O(n^2)$。但实际上，它是标准的 $O(n)$。咱们用两个维度来拆解原因：1. 指针的“单向性” (The One-Way Rule)在 $O(n^2)$ 的算法（比如暴力双重循环）中，外层循环每走一步，内层循环都要回过头去完整地扫一遍。但在你的这个滑动窗口里：右指针 i：从头到尾走了一遍，一共 $n$ 步。左指针 left：虽然它会“跳跃”，但它永远只往右走，从来不回头。想象你在拉一个拉链：右手一直在往前拉，左手偶尔跟上来缩短一下。拉链的每一寸地方，左手和右手都分别只经过了“一次”。右手进场：$n$ 次。左手离场：最多 $n$ 次。总动作数：$n + n = 2n$ 次。在算法复杂度计算中，$O(2n)$ 也就是 $O(n)$。2. 字典查找的“超能力”在暴力解法里，为了检查“有没有重复”，你可能需要遍历一遍之前的子串，那是 $O(k)$ 的开销。但在你的代码里，你用了 哈希表 (mydict)：if s[i] in mydict：这是哈希查找，耗时是 $O(1)$。mydict[s[i]] = i：这是哈希写入，耗时也是 $O(1)$。因为查找和记录都是“瞬间”完成的，所以它没有增加额外的阶乘级负担。一个形象的例子：$O(n^2)$ (暴力法)：你每多看一只猫，都要把之前看过的所有猫再从头数一遍，确认有没有重样的。$O(n)$ (你的算法)：你手里拿着一张名单（字典）。你每看到一只新猫，只需要在名单上查一下名字（$O(1)$），如果重了，就把左边的围栏往右挪一挪。你和围栏都只是从头走到了尾，没有人在原地打转。这就是为什么这道题虽然看起来有嵌套逻辑，但在性能上却是飞速的。

---

# 05/08/2026 今天我们来做Array的题，利用双指针，Dict等办法
## LeetCode 438. Find All Anagrams in a String1. 

Given two strings s and p, return an array of all the start indices of p's anagrams in s. You may return the answer in any order. An Anagram is a word or phrase formed by rearranging the letters of a different word or phrase, typically using all the original letters exactly once. 
简单来说：在 s 中寻找所有长度和 p 一样，且字符组成也完全一样的子串，返回它们的起始下标。

Example 1:
Input: s = "cbaebabacd", p = "abc"
Output: [0, 6]
Explanation:Index 0 is "cba", which is an anagram of "abc".Index 6 is "bac", which is an anagram of "abc".

Example 2:
Input: s = "abab", p = "ab"
Output: [0, 1, 2]
Explanation:Index 0 is "ab", which is an anagram of "ab".Index 1 is "ba", which is an anagram of "ab".Index 2 is "ab", which is an anagram of "ab".

2. 核心算法：固定窗口滑动 (Fixed-Size Sliding Window)窗口大小：恒定为 len(p)。数据结构：使用 collections.Counter (哈希表) 实时维护窗口内的字符频率。关键动作：进场：右指针 i 移入新字符，count_s[s[i]] += 1。出场：当窗口长度超过 len(p)，左边界字符 s[i - len_p] 移出，并从字典中维护计数。对比：直接对比两个字典是否相等。3. Python 实现 (Accepted)Pythonfrom collections import Counter

```Python

class Solution:
    def findAnagrams(self, s: str, p: str) -> List[int]:
        """
        :type s: str
        :type p: str
        :rtype: List[int]
        """
        len_s, len_p = len(s), len(p)
        # 特判：如果 s 比 p 还短，直接回绝
        if len_s < len_p:
            return []
        
        count_s = Counter()     # 动态窗口的账本
        count_p = Counter(p)    # 目标的标准清单
        myresult = []

        for i in range(len_s):
            # 1. 右侧新员工进场
            count_s[s[i]] += 1

            # 2. 窗口满了，左侧老员工离场
            if i >= len_p:
                left_char = s[i - len_p]  # 找到要被踢出的字符
                if count_s[left_char] == 1:
                    del count_s[left_char] # 数量归零必须彻底删除 Key，否则对比会失败
                else:
                    count_s[left_char] -= 1
            
            # 3. 检查当前账本是否符合标准
            if count_s == count_p:
                myresult.append(i - len_p + 1)
        
        return myresult
```

### 复杂度分析 (Complexity)
#### 时间复杂度：$O(n)$。虽然有字典对比，但由于字符集仅限 26 个小写字母，对比代价是 $O(26)$，即常数级。
#### 空间复杂度：$O(1)$。字典最多存储 26 个键值对，不随输入字符串长度 $n$ 增长。

---

## LeetCode 11. Container With Most Water

题目描述给定一个长度为 n 的整数数组 height。有 n 条垂直线，第 i 条线的两个端点是 (i, 0) 和 (i, height[i])。找出两条线，使得它们与 x 轴共同构成的容器可以容纳最多的水。目标：最大化 $Area = (right - left) \times \min(height[left], height[right])$约束：不能倾斜容器，且 $n \ge 2$。2. 算法核心：对撞指针 (Two Pointers)为什么用对撞指针？初始状态：左指针 l 指向索引 0，右指针 r 指向索引 n-1。此时宽度最大。收缩逻辑：由于宽度在收缩过程中注定减小，我们必须通过寻找更高的柱子来弥补宽度的损失。贪心策略（移动哪一边？）容器的水位高度取决于较短的那根柱子（即著名的“木桶效应”）：如果移动高柱子：宽度变小了，但由于高度依然被那根保留着的“短柱子”限制，面积只会变小，绝不可能变大。如果移动矮柱子：虽然宽度变小了，但我们有可能遇到一根更高的柱子，从而拉高整个容器的水位，面积有可能增大。结论：每次都移动较矮的那根柱子，只有这样才有可能在宽度损失的情况下，获得更大的面积收益。3. 复杂度分析时间复杂度：$O(n)$。左右指针合力遍历了整个数组一遍，每个元素仅被访问一次。空间复杂度：$O(1)$。只使用了常数个额外变量用于存储指针和最大面积。

```Python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        # 对撞指针
        myresult = 0
        left = 0
        right = len(height) - 1

        while left < right:
            area = (right - left) * min(height[left], height[right])

            myresult = max(myresult, area)

            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
        
        return myresult
```

---

## LeetCode 15. 3Sum (三数之和)

题目描述给你一个整数数组 nums，判断是否存在三元组 [nums[i], nums[j], nums[k]] 满足 i != j, i != k, j != k ，且 nums[i] + nums[j] + nums[k] == 0 。要求：答案中不可以包含重复的三元组。示例：nums = [-1,0,1,2,-1,-4] -> 输出：[[-1,-1,2],[-1,0,1]]2. 算法核心：排序 + 固定一数 + 双指针对撞这道题的暴力解法是 $O(n^3)$，必超时。我们用 $O(n^2)$ 的策略：排序 (Sort)：这是灵魂。排序后，相同的数会挨在一起（方便去重），且我们可以利用大小关系来移动指针。固定一个数 (i)：遍历数组，每次锁定一个 nums[i] 作为三元组的第一个数。双指针寻找另外两个数 (L 和 R)：在 i 之后的区间内，设置 L = i + 1 和 R = len(nums) - 1。计算 Sum = nums[i] + nums[L] + nums[R]。如果 Sum < 0：说明太小了，L 往右走。如果 Sum > 0：说明太大了，R 往左走。如果 Sum == 0：记录结果，并跳过重复元素。3. 为什么这题是“细节狂魔”？（去重逻辑）这道题最容易错的地方就是去重。你需要处理两个位置的重复：固定位置 i 的去重：如果 nums[i] == nums[i-1]，说明以这个数为开头的组合已经找过了，直接 continue。双指针 L 和 R 的去重：找到一个答案后，如果 L 下一个数和当前一样，或者 R 下一个数和当前一样，要跳过它们。

```Python
class Solution:
    def threeSum(self, nums: List[int]) -> List[List[int]]:
        nums.sort()  # 第一步：排序
        res = []
        n = len(nums)
        
        for i in range(n):
            # 如果当前的数已经大于0，后面三个数之和必然大于0，直接结束
            if nums[i] > 0:
                break
            
            # 去重：如果这个数和上一个数一样，跳过
            if i > 0 and nums[i] == nums[i-1]:
                continue
            
            # 双指针对撞
            l, r = i + 1, n - 1
            while l < r:
                total = nums[i] + nums[l] + nums[r]
                
                if total < 0:
                    l += 1
                elif total > 0:
                    r -= 1
                else:
                    # 找到一组解
                    res.append([nums[i], nums[l], nums[r]])
                    
                    # 关键：在找到解后，也要进行去重
                    while l < r and nums[l] == nums[l+1]:
                        l += 1
                    while l < r and nums[r] == nums[r-1]:
                        r -= 1
                    
                    # 找到解并去重后，两端同时向内收缩
                    l += 1
                    r -= 1
                    
        return res
```

---

## LeetCode 42. Trapping Rain Water (接雨水) 

不仅可以用指针，而且它的双指针解法被公认为是最优雅、空间复杂度最低（$O(1)$）的“神仙解法”。这道题其实是 LeetCode 11 (盛水容器) 的进阶魔改版。11题是求“最大的那个框”，而42题是求“所有小框里的水加起来是多少”。1. 核心逻辑：从“木桶效应”出发在第11题里，我们知道水的高度取决于短板。在第42题里，某一个位置 i 能接多少水，取决于它左边最高的柱子和右边最高的柱子中较短的那一个。公式是：$$Water[i] = \max(0, \min(\text{left\_max}, \text{right\_max}) - height[i])$$2. 为什么能用对撞指针？通常我们会想：为了算第 i 个位置的水，我得先把左右两边的最大值都算出来（这通常需要 $O(n)$ 的空间）。但用对撞指针，我们可以边走边算：设置 left = 0, right = n - 1。维护两个变量：left_max（左边见过的最高高度）和 right_max（右边见过的最高高度）。关键直觉：如果你发现 left_max < right_max，那么对于 left 指针指向的位置来说，它能接多少水已经确定了！虽然我们不知道 left 右边真正的最大值是多少，但我们知道 right_max 已经比 left_max 大了。根据“木桶效应”，决定水位的只能是那个较小的 left_max。所以，我们直接计算 left 处的水，然后让 left += 1。反之亦然。
在 LeetCode 42 题这种二维模型里，$x$ 轴其实被简化成了“单位宽度”。1. $x$ 轴是什么？在题目中，$x$ 轴代表的是数组的下标索引（Index）。每个下标 $i$ 对应的柱子，其宽度固定为 1。因为宽度是 1，所以我们算“体积”的时候，其实就是在算那个长方形的“面积”。$$体积 = 宽度(1) \times 高度(\text{积水深度})$$这就是为什么我们在公式里只看高度差，因为乘个 1 对数值没有影响。2. 为什么二维图能代表三维的“接雨水”？你可以把它想象成一个侧视图。每一根柱子都是一个单位体积的方块。积水是存在这些“坑”里的。我们计算的是这一排建筑在侧面投影下能承载的总水量。3. $x$ 轴在算法中扮演的角色虽然我们在算水量时无视了 $x$ 轴（因为它横向贡献永远是 1），但在双指针移动时，$x$ 轴就是我们的“路标”：left 和 right 指针在 $x$ 轴上对向而行。right - left 决定了中间还有多少个“潜在的坑”没被计算。4. 重新看那个公式如果我们严格按照物理定义来写，公式应该是：$$\text{总水量} = \sum_{i=0}^{n-1} \left( \text{单位宽度} \times \text{积水深度}_i \right)$$因为 $\text{单位宽度} = 1$，所以简化成了：$$\text{总水量} = \sum_{i=0}^{n-1} \left( \min(\text{left\_max}_i, \text{right\_max}_i) - \text{height}_i \right)$$

### 坐标系建模分析
* **Y 轴 (Height)**: 代表地形的海拔高度，是变量，决定了水位上限。
* **X 轴 (Index)**: 代表地理位置。在本项目中，步长（宽度）恒定为 1。
* **计算逻辑**: 
  由于 $\Delta X = 1$，体积计算退化为一维的高度求和。
  这种建模方式将复杂的空间蓄水问题转化为了简单的线性序列处理。

```Python
class Solution:
    def trap(self, height: List[int]) -> int:
        if not height: return 0
        left, right = 0, len(height) - 1
        max_left, max_right = 0, 0
        myresult = 0

        while left < right:
            max_left = max(max_left, height[left])
            max_right = max(max_right, height[right])
            if max_left < max_right:
                myresult += max_left - height[left]
                left += 1
            else:
                myresult += max_right - height[right]
                right -= 1
                
        return myresult
```
---

## LeetCode 141. Linked List Cycle（环形链表） 是链表题型中最经典、面试出镜率最高的一道入门题。它完美的展现了快慢指针（Fast & Slow Pointers） 的优雅。

题目描述:
给你一个链表的头节点 head，判断链表中是否有环。
有环：链表中的某个节点，可以通过连续跟踪 next 指针再次到达。
目标：如果链表中有环，返回 true ；否则，返回 false 。

核心算法：快慢指针（又称“乌龟与兔子”算法）
想象一下，两个人在操场跑道上跑步：
慢指针（Slow）：每次走 1 步（乌龟）。
快指针（Fast）：每次走 2 步（兔子）。

关键直觉：
如果没有环：兔子跑得快，会先到达终点（遇到 null），乌龟永远追不上兔子。
如果有环：兔子和乌龟都会进入环内。因为兔子比乌龟快，它们在环内不断循环，最终兔子一定会从后面“套圈”乌龟。

只要快慢指针相遇（slow == fast），就证明一定有环。

```Python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def hasCycle(self, head: Optional[ListNode]) -> bool:
        fast, slow = head, head
        while fast and fast.next:
            fast = fast.next.next
            slow = slow.next
            if fast == slow:
                return True 
        return False
```

---

## 283. Move Zeroes (移动零) 就太容易了！

虽然 141 题是在“套圈”，但 283 题的快慢指针更像是一个 “质检员与搬运工” 的组合。

1. 核心逻辑：原地整理
题目要求：把所有 0 挪到末尾，同时保持非零元素的相对顺序，且必须在 原地 (In-place) 操作。
我们将快慢指针定义如下：
快指针 (fast)：质检员。它会跑在前面，检查每一个元素。如果是 0，它直接跳过；如果不是 0，它就告诉慢指针。
慢指针 (slow)：搬运工/位置占位符。它指向“下一个应该存放非零元素”的位置。

2. 执行过程
fast 不断向后移动。
每当 fast 发现一个 非零元素 时：
把它和 slow 指向的位置交换。
slow 向后移一步（因为这个坑填好了）。
如果 fast 发现的是 0，它就自己走，slow 原地不动，等着非零元素来填坑。

```Python
class Solution:
    def moveZeroes(self, nums: List[int]) -> None:
        """
        Do not return anything, modify nums in-place instead.
        """
        slow = 0

        for fast in range(len(nums)):
            if nums[fast] != 0:
                nums[fast], nums[slow] = nums[slow], nums[fast]
                slow += 1
        return nums
```
---

## LeetCode 167 两数之和 II 

输入有序数组📝 题目描述给你一个下标从 1 开始的整数数组 numbers ，该数组已按 非递减顺序 排列，请你从数组中找出满足相加之和等于目标数 target 的两个数。如果这两个数分别是 numbers[index1] 和 numbers[index2] ，则 1 <= index1 < index2 <= numbers.length 。以长度为 2 的整数数组 [index1, index2] 的形式返回这两个整数的下标 index1 和 index2。你可以假设每个输入 正好有一个解决方案 ，且你 不能 使用相同的元素两次。你的解决方案必须只使用 $O(1)$ 的额外空间。

📥 输入输出示例

示例 1：输入： numbers = [2, 7, 11, 15], target = 9输出： [1, 2]解释： 2 与 7 之和等于目标数 9 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。

示例 2：输入： numbers = [2, 3, 4], target = 6输出： [1, 3]解释： 2 与 4 之和等于目标数 6 。因此 index1 = 1, index2 = 3 。返回 [1, 3] 。示例 3：输入： numbers = [-1, 0], target = -1输出： [1, 2]解释： -1 与 0 之和等于目标数 -1 。因此 index1 = 1, index2 = 2 。返回 [1, 2] 。

💡 FDE/工程思维提示有序性利用：作为准 FDE，要敏锐察觉到“数组有序”这个约束。这意味着你可以使用双指针从两端向中间逼近，而不需要暴力遍历 。1-Indexed：在将算法封装为 API 或 MCP 工具时，必须严格遵守接口文档的下标规范（本题为从 1 开始计）。空间复杂度：限制 $O(1)$ 额外空间意味着你不能使用额外的哈希表，双指针是唯一解 。

```Python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left, right = 0, len(numbers) - 1
        
        while left < right:
            current_sum = numbers[left] + numbers[right]
            if current_sum == target:
                return [left + 1, right + 1]
            elif current_sum < target:
                left += 1
            else:
                right -= 1
        return []
```

---

# 05/18/2026 
### 看来你之前的底子比想象中还要扎实！既然双指针的两种核心形态（快慢指针、对撞/相向指针）你都已经接触过了，而二分查找和滑动窗口也分别有了一定积累，那我们今天的策略就得从“纯新手入门”升级为“巩固+高阶拓展”。

## 总题数我们依然保持在 8 道题，但结构调整为：
* 3道经典复习（查漏补缺，稳固地基）
* 2道进阶挑战（双指针与滑动窗口的综合变形题）
* 3道全新突破（彻底攻克单调栈这个新大山）

## 重新为你规划的今日刷题清单如下：
* 阶段一：温故与深化（5 道题）
  
  1. 二分查找（从 1 道变 2 道，补齐边界核心）
     *【复习题 1】LeetCode 704. 二分查找 (Binary Search)目的：快速热身。确保你对基础二分（left <= right 还是 left < right）的闭区间/半开区间写法绝对熟练。
     *【进阶题 2】LeetCode 34. 在排序数组中查找元素的第一个和最后一个位置 目的：二分真正的难点在边界控制 。这道题要求你分别去寻找左边界和右边界，是检验二分功底的试金石 。
     
  3. 滑动窗口与双指针（融合进阶，挑战中高难度）
     *【复习题 3】LeetCode 3. 无重复字符的最长子串 目的：最经典的不定长滑动窗口 。复习 right 负责扩张、left 负责收缩的对撞/同向指针节奏 。
     *【复习题 4】LeetCode 167. 两数之和 II - 输入有序数组 目的：复习你的对撞指针技巧 。利用有序性，两头往中间靠拢，秒杀原本需要 $O(n^2)$ 的问题 。
     *【进阶题 5】LeetCode 11. 盛最多水的容器 目的：对撞指针的贪心思考 。为什么每次只能移动较短的那根柱子 ？这道中等题能帮你彻底吃透双指针的移动逻辑。

* 阶段二：知新（3 道题）
  
  3. 全新算法：单调栈 (Monotonic Stack)一句话大白话：它是一个特制的栈，里面的元素要么单调递增，要么单调递减 。每当新来的元素破坏了单调性，就要开始“弹栈”并触发结算 。专门用来搞定**“寻找下一个更大/更小元素”**的 $O(n)$ 神器 。
     
     *【新题 1】LeetCode 739. 每日温度 (Daily Temperatures) 核心考点：单调栈的教科书级母题 。寻找“下一个比今天高的温度在几天后” 。维持一个单调递减栈，新元素大就弹栈算差值 。
     *【新题 2】LeetCode 496. 下一个更大元素 I 核心考点：单调栈 + 哈希表 。用单调栈预处理出右边第一个更大元素，存进 Map 里供随时查询 。
     *【新题 3】LeetCode 503. 下一个更大元素 II核心考点：循环数组的处理。如果数组是环形的（末尾的下一个是开头）该怎么办？利用“模拟遍历两次数组”或取模运算（i % n），配合单调栈解决。

## 704

704. Binary Search

Given an array of integers nums which is sorted in ascending order, and an integer target, write a function to search target in nums. If target exists, then return its index. Otherwise, return -1.

You must write an algorithm with O(log n) runtime complexity.

Example 1:

Input: nums = [-1,0,3,5,9,12], target = 9
Output: 4
Explanation: 9 exists in nums and its index is 4
Example 2:

Input: nums = [-1,0,3,5,9,12], target = 2
Output: -1
Explanation: 2 does not exist in nums so return -1
 

Constraints:

1 <= nums.length <= 104
-104 < nums[i], target < 104
All the integers in nums are unique.
nums is sorted in ascending order.

```Python
class Solution:
    def search(self, nums: List[int], target: int) -> int:
        left, right = 0, len(nums) - 1

        while left <= right:
            mid = left + (right - left) // 2
            
            if nums[mid] > target:
                right = mid - 1
            elif nums[mid] < target:
                left = mid + 1
            else:
                return mid 
        return -1
```

#### 二分查找底层细节：为什么是 `left + (right - left) // 2`？

这个问题问到了二分查找中非常经典的一个**底层细节**！

简单来说，写成 `mid = left + (right - left) // 2` 而不是直觉上的 `mid = (left + right) // 2`，最核心的原因是为了**防止数值溢出（Overflow）**，同时在 Python 中它能完美处理整除。

我们可以把这个问题拆成三点来看：
* 1. 致命的“整数溢出”问题

   在绝大多数编程语言（如 C++, Java, Go 等）中，整型变量（`int`）都是有最大范围限制的（例如 32 位有符号整数的最大值是 $2147483647$）。
   
   假设我们有以下极端情况：
   * 数组非常大，我们要查找的范围在数组的后半段。
   * 此时 `left = 2000000000`（20亿）
   * `right = 2100000000`（21亿）

   如果你使用直觉上的公式：
   $$\text{mid} = \frac{\text{left} + \text{right}}{2}$$
   
   在计算 `left + right` 的瞬间，结果是 **41亿**。这个数字已经远远超过了 32 位 `int` 的最大承受范围（21.47亿）。在 C++ 或 Java 中，这会导致**严重的整数溢出**，使它变成一个负数，随后除以 2 得到一个错误的负数下标，直接引发程序崩溃（数组越界）。

* 2. 为什么变形成 `left + (right - left) // 2` 就能解决？

   我们从数学逻辑上来看，这两个公式在数学上是完全等价的：
   $$\text{left} + \frac{\text{right} - \text{left}}{2} = \frac{2 \times \text{left} + \text{right} - \text{left}}{2} = \frac{\text{left} + \text{right}}{2}$$
   
   但是计算机在执行 `left + (right - left) // 2` 时的步骤是：
   1. **先算 `right - left`**：用 21亿 减去 20亿，得到 **1亿**（这个数字很小，绝对不会溢出）。
   2. **再算 `1亿 // 2`**：得到 **5000万**。
   3. **最后算 `left + 5000万`**：20亿 加上 5000万，得到 **20.5亿**（依然在 21.47亿 的安全范围内）。

> 💡 **通俗点说**：
> * `(left + right) // 2` 的逻辑是：**先把两个人的家产全部加在一起，再平分。**（如果两人都很有钱，加在一起可能就超过了国家的最高限额）。
> * `left + (right - left) // 2` 的逻辑是：**我站在左边（`left`），然后向右走我们两人距离的一半。**（自始至终没有产生任何超越边界的超级大数）。

* 3. 那为什么 Python 也要这么写？

   你可能会问：“Python 的整数不是支持无限大（自动扩容）吗？它又不会溢出，为什么在 Python 题解里大家也这么写？”
   
   原因有两个：
   * **养成良好的算法习惯**：面试官在面试时，往往看重的是你是否有跨语言的底层安全意识。如果你在写 Python 时能写出 `left + (right - left) // 2`，面试官会觉得你对计算机底层原理很了解。
   * **符号与工程规范**：统一的模板利于记忆，这种写法在任何语言下都是 100% 绝对安全的。

--

## 34

# 34. Find First and Last Position of Element in Sorted Array

`Medium` `Topics` `Companies`

## 📝 Description

Given an array of integers `nums` sorted in non-decreasing order, find the starting and ending position of a given `target` value.

If `target` is not found in the array, return `[-1, -1]`.

You must write an algorithm with $O(\log n)$ runtime complexity.

### Example 1:
> **Input:** nums = [5,7,7,8,8,10], target = 8
> **Output:** [3,4]

### Example 2:
> **Input:** nums = [5,7,7,8,8,10], target = 6
> **Output:** [-1,-1]

---

## 💡 Core Strategy (Two Binary Searches)

We split the problem into two distinct binary searches using the **Closed Interval `[left, right]`** approach:
1. **Find Left Bound**: When `nums[mid] == target`, instead of returning immediately, we restrict our search to the left half (`right = mid - 1`) to find the first occurrence.
2. **Find Right Bound**: When `nums[mid] == target`, we restrict our search to the right half (`left = mid + 1`) to find the last occurrence.

---

## 💻 Python3 Solution

```python
class Solution:
    def searchRange(self, nums: List[int], target: int) -> List[int]:
        
        # Helper function to find either the left or right boundary
        def findBound(isLeft: bool) -> int:
            left, right = 0, len(nums) - 1
            bound = -1
            
            while left <= right:
                mid = left + (right - left) // 2
                
                if nums[mid] < target:
                    left = mid + 1
                elif nums[mid] > target:
                    right = mid - 1
                else:
                    # Found target! Record the position
                    bound = mid
                    if isLeft:
                        right = mid - 1  # Keep looking left
                    else:
                        left = mid + 1   # Keep looking right
            return bound

        left_idx = findBound(isLeft=True)
        right_idx = findBound(isLeft=False)
        
        return [left_idx, right_idx]
```
--

## 3. Longest Substring Without Repeating Characters

## 📝 Description

Given a string `s`, find the length of the **longest substring** without repeating characters.

### Example 1:
> **Input:** s = "abcabcbb"
> **Output:** 3
> **Explanation:** The answer is "abc", with the length of 3.

### Example 2:
> **Input:** s = "bbbbb"
> **Output:** 1
> **Explanation:** The answer is "b", with the length of 1.

### Example 3:
> **Input:** s = "pwwkew"
> **Output:** 3
> **Explanation:** The answer is "wke", with the length of 3. 
> Notice that the answer must be a substring, "pwke" is a subsequence and not a substring.

### Constraints:
* $0 \le \text{s.length} \le 5 \times 10^4$
* `s` consists of English letters, digits, symbols and spaces.

---

## 💡 Core Strategy (Sliding Window - Map Optimize)

We use a sliding window with two pointers `left` and `i` (as right pointer), combined with a hash map (`mydict`) to optimize the window shrinking process:

1. **Check First**: Before updating the map, check if the current character `s[i]` already exists in `mydict` and its recorded index is within the current window (`mydict[s[i]] >= left`). If so, we catch a real duplicate, and `left` immediately "jumps" to `mydict[s[i]] + 1`.
2. **Update Map**: After the check, we update or register the current character's latest index: `mydict[s[i]] = i`. **The order is crucial!** Checking must happen before updating to prevent the pointer from colliding with itself.
3. **Update Max**: At each step, calculate the current valid window length using `i - left + 1` and update `max_length`.

---

## 💻 Python3 Solution

```python
class Solution:
    def lengthOfLongestSubstring(self, s: str) -> int:
        left = 0
        mydict = {}
        max_length = 0

        for i in range(len(s)):
            # 1. Check first: if the character is a true duplicate within the current window
            if s[i] in mydict and mydict[s[i]] >= left:
                # Left pointer jumps to the next position of the last occurrence
                left = mydict[s[i]] + 1
            
            # 2. Update the ledger with the character's latest index
            mydict[s[i]] = i
            
            # 3. Calculate and update the maximum length
            max_length = max(max_length, i - left + 1)
            
        return max_length
```
--

## 167 
# 167. Two Sum II - Input Array Is Sorted

`Medium` `Topics` `Companies`

## 📝 Description

Given a **1-indexed** array of integers `numbers` that is already **sorted in non-decreasing order**, find two numbers such that they add up to a specific `target` number. Let these two numbers be `numbers[index1]` and `numbers[index2]` where $1 \le \text{index1} < \text{index2} \le \text{numbers.length}$.

Return the indices of the two numbers, `index1` and `index2`, **added by one** as an integer array `[index1, index2]` of length 2.

The tests are generated such that there is **exactly one solution**. You may not use the same element twice.

Your algorithm must use only constant extra space.

### Example 1:
> **Input:** numbers = [2,7,11,15], target = 9
> **Output:** [1,2]
> **Explanation:** The sum of 2 and 7 is 9. Therefore, index1 = 1, index2 = 2. We return [1, 2].

### Constraints:
* $2 \le \text{numbers.length} \le 3 \times 10^4$
* $-1000 \le \text{numbers}[i] \le 1000$
* `numbers` is sorted in **non-decreasing order**.
* $-1000 \le \text{target} \le 1000$
* The tests are generated such that there is **exactly one solution**.

---

## 💡 Core Strategy (Two Pointers / Two-Way Collision)

Since the array is already sorted, we can use two pointers starting from both ends moving towards each other:
1. **Initialize**: `left` at index `0` (smallest element) and `right` at the last index (largest element).
2. **Compare**: Calculate `current_sum = numbers[left] + numbers[right]`.
   * If `current_sum == target`, we found the answer! Return `[left + 1, right + 1]` (because the problem asks for 1-based indexing).
   * If `current_sum < target`, we need a larger value, so we advance `left += 1`.
   * If `current_sum > target`, we need a smaller value, so we retreat `right -= 1`.

---

## 💻 Python3 Solution

```python
class Solution:
    def twoSum(self, numbers: List[int], target: int) -> List[int]:
        left = 0
        right = len(numbers) - 1
        
        while left < right:
            current_sum = numbers[left] + numbers[right]
            
            if current_sum == target:
                # 题目要求返回的是 1-based 的下标，所以都要加 1
                return [left + 1, right + 1]
            elif current_sum < target:
                left += 1   # 和太小了，左指针右移换个大数
            else:
                right -= 1  # 和太大了，右指针左移换个小数
                
        return []
```
LeetCode 167 题，用的是标准的、最纯正的双指针（Two Pointers）解法——具体来说，叫“相向双指针”（或者叫“对撞指针”） 。

虽然滑动窗口本质上也是双指针的一种，但它们在解题的核心目的和指针移动规则上，有着非常微妙且关键的差别。我们花 1 分钟把这个彻底理清，以后你看一眼题目就能瞬间分类！

🆚 167 题（双指针）与 滑动窗口 的核心区别
1. 指针的移动方向不同

滑动窗口（如 LeetCode 3）：属于同向双指针 。left 和 right 一前一后，都从数组的左边同向往右边冲 。两个指针中间围起来的区域，就像一个在向前滑动的“窗口” 。


167 两数之和 II：属于相向双指针（对撞指针） 。left 站在数组的最左端（开头），right 站在数组的最右端（末尾） 。它们两个面对面，向中间靠拢 。

2. 核心目的不同

滑动窗口：通常是为了在全场找一个“连续的区间/子串”，这个区间要满足某种条件（比如不能有重复字符） 。


167 题双指针：完全不是为了找连续区间，而是利用数组已经排好序（升序）的黄金特性，通过两头包抄，去揪出某两个特定的数字 。

💡 167 题是怎么用“对撞指针”秒杀的？
既然数组是有序的，我们让 left 指向最小的值，right 指向最大的值 。
每次把它们两个指着的数字加起来（sum = nums[left] + nums[right]） ：

如果 sum 刚好等于 target：恭喜你，直接抓到答案！


如果 sum < target（小了）：说明现在的和不够大。怎么变大？因为数组是升序的，我们要让 left 往右走一步（left += 1），换一个大一点的数再试 。


如果 sum > target（大了）：说明现在的和太大了。怎么变小？我们要让 right 往左走一步（right -= 1），换一个小一点的数再试 。

这就是利用相向双指针在 O(n) 时间内搞定问题的全过程，中间没有任何“窗口”的概念 。

#### 不理解two sum和two sum ii为什么方法不一样？看起来很像的两道题

* 这个问题直接戳中了算法面试里最经典的一个“孪生兄弟”陷阱。这两道题不仅长得像，连名字都几乎一样，但解法却天差地别（Two Sum 用哈希表 ，Two Sum II 用双指针 ）。它们之所以走上两条不同的路，最核心的变量只有四个字：数组有序 。我们不聊复杂的公式，用最接地气的例子来看看这哥俩的底层差别：🆚 核心区别：有没有“引路导航”1. Two Sum II（有导航：直接两头包抄）在 Two Sum II 中，题目明确给了一个黄金条件：数组已经是升序排列的 。
这就好比你在一个从小到大排好队的队伍里找两个人，让他们俩的身高加起来刚好等于一个目标值 。你让最矮的（left）和最高的（right）站出来加一下 。如果发现矮+高 < 目标值 ，你根本不需要再去试别的人了，因为 right 已经是全场最高了，他跟最矮的加起来都小，说明最矮的彻底没戏。你直接让 left 往右走一步，换个稍高一点的来试（left += 1） 。同理，如果矮+高 > 目标值，说明最高的那个太高了，必须换个稍矮一点的来试（right -= 1） 。💡 总结：因为数组有序，每一次相加都在给你“指路”（告诉你该调大还是调小），所以你可以用对撞双指针在 $O(n)$ 时间内搞定 。2. 第一题 Two Sum（无导航：只能四处打听）而第 1 题 Two Sum 坏就坏在数组是乱序的。比如 nums = [3, 1, 4, 2]，你想找目标值 5。如果你还想用双指针，left 指向 3，right 指向 2，相加等于 5 纯属运气好。如果是别的目标值呢？因为数组没有顺序：当 nums[left] + nums[right] < target 时，你根本不知道该把 left 右移还是把 right 左移，因为右移指不定遇到一个更小的数，左移指不定遇到一个更大的数。双指针失去了“导航”，瞎走只会漏掉正确答案。那第一题怎么破？——用“备忘录（哈希表）”既然不能两头包抄，数字们又是乱坐的，那我们就只能边走边记。当你遇到 3 时，你在心里（哈希表里）记账：{3: 0}，代表“3 来过了，在 0 号位”。同时你在想：“要是后面来个 2 就完美了（$5 - 3 = 2$）。”接着你走到 1，一查账本，里面没有 4，于是把 1 记上：{3: 0, 1: 1}。直到你走到 2，你拿 $5 - 2 = 3$ 去账本里一翻：“哈哈，3 之前来过，在 0 号位！” 瞬间凑成一对。📊 一张表彻底看懂两兄弟题目数组状态核心目的绝配算法时间复杂度空间复杂度Two Sum (第1题)乱序（Unsorted）边走边查，看谁和自己能凑成一对哈希表 (Hash Map)$O(n)$$O(n)$（需要开辟空间记账）Two Sum II (167题)升序（Sorted） 利用有序性，两头夹击逼近目标 双指针 (Two Pointers) $O(n)$ $O(1)$（省空间，不需要记账） 🎯 面试官的隐藏考点如果面试官在面试时出了第 1 题 Two Sum，然后问你：“能不能不用额外空间（空间复杂度 $O(1)$）来做？” 这时候他其实就是在疯狂暗示你：“你先去把数组用 sort() 排个序，不就变成 Two Sum II 了吗？然后你就可以用双指针把空间优化到 $O(1)$ 了！” 

--

## 11. Container With Most Water

`Medium`

## 📝 Description

You are given an integer array `height` of length `n`. There are `n` vertical lines drawn such that the two endpoints of the $i^{th}$ line are `(i, 0)` and `(i, height[i])`.

Find two lines that together with the x-axis form a container, such that the container contains the most water.

Return the maximum amount of water a container can store.

**Notice** that you may not slant the container.

### Example 1:
> **Input:** height = [1,8,6,2,5,4,8,3,7]
> **Output:** 49
> **Explanation:** The vertical lines are represented by array [1,8,6,2,5,4,8,3,7]. In this case, the max area of water the container can contain is 49.

### Example 2:
> **Input:** height = [1,1]
> **Output:** 1

### Constraints:
* $n == \text{height.length}$
* $2 \le n \le 10^5$
* $0 \le \text{height}[i] \le 10^4$

## 💡 Core Strategy (Two Pointers + Greedy)

We use a two-pointer approach starting from both ends (`left = 0` and `right = len(height) - 1`) and move toward each other:

1. **Geometry Calculation**: The water volume is determined by `width * current_height`, where `width = right - left` (no `+ 1` because it represents geometric distance) and `current_height = min(height[left], height[right])` (limited by the shorter board).
2. **Greedy Move**: At each step, we **always move the pointer pointing to the shorter line inward**.
   * *Why?* Shifting the longer line inwards will only decrease the width while the height remains bottlenecked by the shorter line, ensuring a smaller area. 
   * Only by moving the shorter line do we stand a chance of finding a taller boundary that might compensate for the loss in width.

## 💻 Python3 Solution

```python
class Solution:
    def maxArea(self, height: List[int]) -> int:
        # Initialize two pointers at both ends
        left, right = 0, len(height) - 1
        max_water = 0
        
        while left < right:
            # Calculate geometric width
            width = right - left
            # Height is bottlenecked by the shorter line (木桶效应)
            current_height = min(height[left], height[right])
            
            # Update the maximum water volume
            max_water = max(max_water, width * current_height)
            
            # Greedy Strategy: Move the shorter pointer inward
            if height[left] < height[right]:
                left += 1
            else:
                right -= 1
                
        return max_water
```

--

## 739. Daily Temperatures

`Medium`

## 📝 Description

Given an array of integers `temperatures` represents the daily temperatures, return an array `answer` such that `answer[i]` is the number of days you have to wait after the $i^{th}$ day to get a warmer temperature. If there is no future day for which this is possible, keep `answer[i] == 0` instead.

### Example 1:
> **Input:** temperatures = [73,74,75,71,69,72,76,73]
> **Output:** [1,1,4,2,1,1,0,0]

### Example 2:
> **Input:** temperatures = [30,40,50,60]
> **Output:** [1,1,1,0]

### Example 3:
> **Input:** temperatures = [30,30,30]
> **Output:** [0,0,0]

### Constraints:
* $1 \le \text{temperatures.length} \le 10^5$
* $30 \le \text{temperatures}[i] \le 100$

## 💡 Core Strategy (Monotonic Decreasing Stack)

Whenever you see problems asking for **"finding the next greater/smaller element"**, your brain should instantly think of a **Monotonic Stack**!

1. **The Ledger**: We maintain a stack that stores the **indices** of the days, and we ensure the corresponding temperatures of these indices inside the stack are always in **strictly decreasing order**.
2. **The Encounter**: We iterate through the temperatures day by day. For the current day `i`:
   * As long as today's temperature `temperatures[i]` is **warmer** than the temperature at the top of our stack (`temperatures[stack[-1]]`), it means the day at the top of the stack has finally found its "warmer day"!
   * We pop that day out (`prev_index = stack.pop()`) and calculate the day difference: `answer[prev_index] = i - prev_index`.
3. **The Push**: After popping all colder days, we push today's index `i` onto the stack to wait for its own warmer day in the future.


## 💻 Python3 Solution

```python
class Solution:
    def dailyTemperatures(self, temperatures: List[int]) -> List[int]:
        n = len(temperatures)
        ans = [0] * n
        stack = []  # Monotonic decreasing stack storing indices
        
        for i in range(n):
            # While stack is not empty and today's temp is hotter than the stack top temp
            while stack and temperatures[i] > temperatures[stack[-1]]:
                prev_index = stack.pop()
                # Calculate how many days you had to wait
                ans[prev_index] = i - prev_index
                
            # Push the current day's index into the stack
            stack.append(i)
            
        return ans
```

--

## 496 Next Greater Element I

* 这玩意儿在 LeetCode 上挂着 Easy 的标签，简直就是“杀猪盘”！其实，这道题之所以被归为 Easy，纯粹是因为它的数据规模非常小（你看它的 Constraints 约束条件：数组长度最大才 1000）。这意味着，如果你用暴力解法（双重循环）去写，LeetCode 的后台测试也能大摇大摆地让你通过（Accepted）。❌ 面试官眼中的“假 Easy”（暴力解法）如果不用单调栈，这题确实是 Easy：遍历 nums1 中的每一个数，比如 4。去 nums2 里先肉眼找到 4 的位置。然后从 4 的位置往右一个一个数，直到撞见第一个比 4 大的数。这种做法的时间复杂度是 $O(m \times n)$。因为这道题上限才 1000，计算机顶得住。但是！如果面试官在面试里把这道题掏出来，他绝对不是想看你写这个暴力解法的。🔥 为什么大厂面试管它叫“单调栈”？一旦面试官把数据规模加到 $10^5$（10万），你的暴力解法当场就会超时崩掉。这时候，它就暴露出它本质上是一道中等（Medium）难度的单调栈隐蔽题。它之所以是单调栈，是因为它完美符合单调栈的核心暗号：“为一个数组里的每一个数，寻找它右边第一个比它大的元素。”只要你用单调栈预处理了 nums2，你就能在 $O(n)$ 的时间内把 nums2 全场的“右边大佬”全揪出来。💡 一个让你彻底释怀的行业真相在 LeetCode 里，有大量早期题目（比如前 500 题）的难度分级是有点失准的。第 1 题 Two Sum：挂着 Easy，但如果你没学过哈希表，根本想不出来。第 496 题：挂着 Easy，但如果不走暴力，用最优解写，它就是正宗的单调栈 Medium 题。

老铁，既然要讲，咱们就彻底抛弃官方那段绕口令一样的英文描述，直接用最接地气的例子，把这道题的单调栈内核给它拆得一丝不挂！

这道题的恶心之处在于，它给了你两个数组（nums1 和 nums2），还说 nums1 是 nums2 的子集。很多人一上来就想同时盯着两个数组看，结果越看越乱。

咱们的破局核心就两个字：解耦（拆开来看）。

💡 第一步：忘掉 nums1，把它当成 739 题来做（批发商模式）
我们先假装 nums1 根本不存在，全场只看大数组 nums2。
假设 nums2 = [1, 3, 4, 2]。

我们要找的是：nums2 里每一个数字，它右边第一个比它大的数是谁。
这不就是 739 题排队进屋轰人的游戏吗？而且这次更爽，因为每个数字都是独一无二的，我们连下标都不用存了，直接把数字本身扔进单调栈里排队！

我们拿一个账本（哈希表 res_map）在旁边记账：

数字 1 进场：屋里没人，1 老实待着。

栈内：[1]

数字 3 进场：3 比栈顶的 1 大！触发轰人！

1 被轰出栈。我们在账本上记下：res_map[1] = 3（意思就是 1 的下一个更大元素是 3）。

屋里空了，3 坐下。

栈内：[3]

数字 4 进场：4 比栈顶的 3 大！继续轰人！

3 被轰出栈。我们在账本上记下：res_map[3] = 4。

屋里空了，4 坐下。

栈内：[4]

数字 2 进场：2 比栈顶的 4 矮。相安无事，2 老实待着。

栈内：[4, 2]

数组遍历完了。留在栈里的 [4, 2] 到死都没等到比它们大的数字。

此时，我们的批发账本 res_map 完美诞生：

Python
res_map = {
    1: 3,
    3: 4
}
💡 第二步：请出 nums1，对号入座（零售商模式）
大数组 nums2 全场的答案都被我们用单调栈“批发”算出来了。现在，题目问我们什么，我们去账本里查就行了。

题目给的 nums1 = [4, 1, 2]。我们拿一个空答案本 ans = []：

查第一个数 4：翻开账本 res_map，里面没有 4（因为 4 没被轰走过）。说明 4 右边没有比它大的。答案给 -1。

ans = [-1]

查第二个数 1：翻开账本，赫然写着 1: 3。哈哈，找到了，答案给 3。

ans = [-1, 3]

查第三个数 2：翻开账本，里面没有 2。答案给 -1。

ans = [-1, 3, -1]

查找结束，[-1, 3, -1] 就是最终答案！这跟官方示例 1 的输出完全一模一样！

💻 带着这个画面，重新审视代码
现在你再来看这段 Python 代码，是不是每一个逻辑都变得极其顺理成章了？

```Python
class Solution:
    def nextGreaterElement(self, nums1: List[int], nums2: List[int]) -> List[int]:
        res_map = {}
        stack = []  # 维持单调递减的栈，直接存数值
        
        # ─── 第一步：在 nums2 里玩大个子轰人游戏（批发答案） ───
        for num in nums2:
            # 新来的 num 比栈顶大，说明栈顶找到了它的“右边大佬”
            while stack and num > stack[-1]:
                popped_val = stack.pop()
                res_map[popped_val] = num  # 记账
                
            stack.append(num)  # 轰完人后自己老实入栈排队
            
        # ─── 第二步：拿着 nums1 去账本里对号入座（零售查询） ───
        ans = []
        for num in nums1:
            # 查账：如果 num 在账本里，返回对应的最大值；不在，说明是没被轰走的傲骨，给 -1
            ans.append(res_map.get(num, -1))
            
        return ans
```

### “除了单调栈，还有别的栈吗？”

* 答案是：太多了！在算法和计算机的世界里，“栈”（后进先出）是一个非常抽象的底层容器，就像一个塑料桶，只要你往里面叠盘子，它就叫栈。而“单调栈”只是在这个桶里加了一条“必须保持大小顺序”的铁律而已。除了单调栈，面试和工程里最常考、最经典的还有以下三大类栈。它们不需要保持大小单调，但各自拥有神奇的超能力：

1. 括号匹配栈（对称消消乐栈）这是最经典的普通栈应用。代表题目：LeetCode 20. 有效的括号。它的规矩：不需要管大小顺序，它玩的是“消消乐”。生活场景：吃汉堡 🍔。你必须先叠下半块面包、再叠生菜、再叠牛肉，最后盖上上半块面包。吃的时候，最先咬到的（最上面的）上半块面包，必须和最底下的下半块面包完美对称。算法怎么玩：遇到左括号 (, [, { 就无脑压入栈；遇到右括号 ), ], }，就让它跟栈顶的左括号碰一下，如果它们是一对（比如 ( 和 )），就双双消掉（弹栈）。如果最后全消光了，说明括号完全合法。

2. 表达式计算栈（逆波兰表达式栈）计算机在底层计算我们写的复杂数学公式（比如 3 + 5 * (2 - 4)）时，其实根本看不懂，它就是靠两个栈来配合完成的。代表题目：LeetCode 150. 逆波兰表达式求值。数字栈：专门用来存数字。符号栈：专门用来存 +、-、*、/ 和括号。核心逻辑：计算机从左往右读公式，遇到数字进数字栈。遇到符号时，如果新符号的优先级比符号栈顶的符号高（比如 * 遇到了 +），就继续压栈；如果新符号优先级低，就从数字栈里弹出两个数字，把栈顶的符号拿出来做运算，算完的结果再压回数字栈。

3. 浏览器前进后退栈（双栈互倒流）你天天在用的浏览器（或者编辑器的 Ctrl+Z 撤销和 Ctrl+Y 反撤销），底层就是用两个普通的栈实现的。栈 A（后退栈）：你每点开一个新网页（网页1 $\rightarrow$ 网页2 $\rightarrow$ 网页3），它们就依次压入栈 A。栈 B（前进栈）：一开始是空的。当你点“后退”时：网页3 从 栈 A 弹出，退回到网页2。但网页3 并没有死，它被压入了 栈 B。当你点“前进”时：网页3 又从 栈 B 弹出来，重新压回 栈 A。两个小桶把数据倒来倒去，就完美实现了极其丝滑的前进和后退。

---

## 503. Next Greater Element II

`Medium` `Topics` `Companies`

## 📝 Description

Given a circular integer array `nums` (i.e., the next element of `nums[nums.length - 1]` is `nums[0]`), return *the **next greater number** for every element in `nums`*.

The **next greater number** of a number `x` is the first greater number to its traversing-order next in the array, which means you could search circularly to find its next greater number. If it doesn't exist, return `-1` for this number.

### Example 1:
> **Input:** nums = [1,2,1]
> **Output:** [2,-1,2]
> **Explanation:** 
> - The first 1's next greater number is 2.
> - The number 2 can't find next greater number.
> - The second 1's next greater number needs to search circularly, which is also 2.

### Example 2:
> **Input:** nums = [1,2,3,4,3]
> **Output:** [2,3,4,-1,4]

### Constraints:
* $1 \le \text{nums.length} \le 10^4$
* $-10^9 \le \text{nums}[i] \le 10^9$

---

## 💡 Core Strategy (Monotonic Stack + Circular Array Trick)

This is the ultimate evolution of the Next Greater Element series. The only twist is the **circular array**. 

### The Naive Way vs The Smart Way
* **Naive**: Literally duplicate the array `nums = nums + nums` to simulate the circle. This works but wastes extra space.
* **Smart (The Modulo Trick)**: We pretend the array is doubled ($2n$ in length), and use the **modulo operator (`% n`)** to map back to the real indices. This simulates walking through the array twice without using extra memory!

### Algorithmic Steps:
1. **Initialize**: Create a result array `ans` initialized with `-1`. Since we are calculating index-based differences or looking up by positions, we should store **indices** in our monotonic stack, just like LeetCode 739.
2. **Double Traverse**: Run a `for` loop from `0` to `2n - 1`.
3. **Real Index Mapping**: The current virtual index is `i`, but the real index in `nums` is `real_i = i % n`.
4. **Monotonic Pop**: As long as `nums[real_i]` is greater than `nums[stack[-1]]`, it means the element at `stack[-1]` has found its next greater element! We pop it out and record: `ans[stack.pop()] = nums[real_i]`.
5. **Conditional Push**: We only push `real_i` onto the stack during the first pass (`i < n`) or if it still needs to be evaluated, but pushing all `real_i` up to `2n` and letting the `while` clear them out naturally is the cleanest template.

---

## 💻 Python3 Solution

```python
class Solution:
    def nextGreaterElement(self, nums: List[int]) -> List[int]:
        n = len(nums)
        ans = [-1] * n
        stack = []  # Monotonic decreasing stack storing indices
        
        # Traverse the array virtually twice
        for i in range(2 * n):
            real_i = i % n  # Map virtual index back to actual array bounds
            
            # Standard Monotonic Stack pop logic
            while stack and nums[real_i] > nums[stack[-1]]:
                prev_index = stack.pop()
                ans[prev_index] = nums[real_i]
                
            # We only need to push indices into the stack during the first traversal traversal pass
            if i < n:
                stack.append(real_i)
                
        return ans
```

---

## 05/22/2026

### 复习模块：

# 🏆 LeetCode 算法通关秘籍与历史战果汇总 (截至 2026-05-22)

| 阶段 | 题号与名称 | 核心算法分类 | 核心通关秘籍（一句话直击灵魂） |
| :--- | :--- | :--- | :--- |
| **温故** | **1. Two Sum** | 哈希表 / 空间换时间 | 乱序数组的“四处打听”流，用字典充当备忘录 $O(n)$ 秒杀。 |
| **温故** | **3. Longest Substring Without Repeating Characters** | 不定长滑动窗口 | `left` 指针绝不倒退的“拉链法则”，`>= left` 护体防止撞见已被淘汰的脏数据。 |
| **温故** | **438. Find All Anagrams in a String** | 固定长度滑动窗口 | 恒定为 `len(p)` 的窗口，用 `Counter` 实时记账。数量归零时必须彻底 `del` 对应的键，否则字典对比会直接翻车。 |
| **温故** | **11. Container With Most Water** | 相向双指针 / 贪心 | 木桶效应决定蓄水高度。由于向内收缩宽度注定减小，每次必须无脑移动较矮的柱子，去赌一个更高的未来。 |
| **温故** | **15. 3Sum** | 排序 + 相向双指针 | 细节狂魔。排序是去重的灵魂，固定第一个数后用对撞指针两头包抄。固定位 `i`、左指针 `L`、右指针 `R` 三个位置全都要极限跳过重复值。 |
| **温故** | **42. Trapping Rain Water** | 相向双指针 / 空间极限优化 | 木桶效应的二维终极魔改。不需要提前开辟 $O(n)$ 空间，利用对撞指针，哪边矮就先结算哪边，用 `left_max` 和 `right_max` 边走边算。 |
| **温故** | **141. Linked List Cycle** | 快慢指针（同向） | 操场套圈理论。兔子（快指针步长 2）和乌龟（慢指针步长 1）同向奔跑，一旦没有环，兔子直奔终点；一旦有环，兔子必定在环内追尾并套圈乌龟。 |
| **温故** | **283. Move Zeroes** | 快慢指针 / 原地交换 | 质检员与搬运工的完美配合。快指针（`fast`）跑前面当质检员，抓到非 0 元素就告诉慢指针（`slow`）原地控坑交换，做到完美的 $O(1)$ 空间整理。 |
| **温故** | **167. Two Sum II - Input Array Is Sorted** | 相向双指针（对撞） | 有序数组附带黄金导航。最矮和最高对撞，矮+高太小则 `left += 1` 换大数，太大则 `right -= 1` 换小数。注意题目要求返回 1-based 下标。 |
| **温故** | **704. Binary Search** | 二分查找 / 基础模板 | 闭区间 `[left, right]` 的死律，进入循环条件必须是 `while left <= right`。利用 `left + (right - left) // 2` 彻底封死多语言下的数值溢出毒瘤。 |
| **温故** | **34. Find First and Last Position of Element in Sorted Array** | 二分查找 / 边界控制 | 二分功底的试金石。撞见 `target` 绝对不要收手：找左边界就让右边界左缩（`right = mid - 1`）继续向左死磕；找右边界就让左边界右缩（`left = mid + 1`）继续向右死磕。 |
| **知新** | **739. Daily Temperatures** | 单调递减栈 / 下一个更大元素 | 单调栈教科书级母题。小黑屋里必须严格保持从大到小排队。栈里死死存**天数下标**，新来大个子弹栈的瞬间，用“今天下标 - 历史下标”顺手把天数差结算掉。 |
| **知新** | **496. Next Greater Element I** | 单调栈 + 哈希表字典 | 经典的工程解耦破局思维。把恶心的双数组强行拆开：大数组 `nums2` 用单调栈做“批发加工”把老大关系存入字典，小数组 `nums1` 直接去字典做“零售查询” $O(1)$ 对号入座。 |
| **知新** | **503. Next Greater Element II** | 单调栈 + 循环数组取模 | 环形数组的终极奥义。不需要真的去成倍扩容数组浪费内存，在脑脑里把数组“拉直”虚拟转两圈（`2 * n`），在访问元素时用 `i % n` 配合结果数组下标，一步到位暴力清空库存。 |

今天是 2026年5月22日，咱们废话不多说，直接开始咱们的“承前启后”大练兵。按照你的要求，今天我们总共安排 10 道题：

* 5 道复习巩固题：使用全新题目，来深度轰炸和检验你之前学过的双指针/滑动窗口/哈希表/单调栈。
* 5 道全新算法题：正式吹响挺进新大陆——二叉树与递归（Binary Tree & Recursion）的冲锋号！

首先，我把你之前死磕过的所有战果用一张干净利落的表格为你汇总，方便你对自己的武器库了如指掌。

# 🎯 2026-05-22 “老带新” 10 道必刷题单全景规划

## ⚔️ 阶段一：旧算法·新战场 (5 道全新盲测题)

| 题号与名称 | 难度 | 核心算法分类 | 今日盲测突破核心（为什么 Pick 它？） |
| :--- | :--- | :--- | :--- |
| **202. Happy Number**<br>(快乐数) | `Easy` | 快慢指针 / 哈希集合 | 别被数学名字唬住！它表面上是纯数学计算，底层本质上是不带指针的 **LeetCode 141 快慢指针（链表环）** 的魔改变种。如果计算陷入死循环，就代表有环！用它来复习乌龟与兔子流，好玩又绝妙。 |
| **209. Minimum Size Subarray Sum**<br>(长度最小的子数组) | `Medium` | 不定长滑动窗口 | 用来高强度检验你对 **LeetCode 3（最长子串）** 的滑动窗口掌控力。这次反过来了，目标是寻找满足条件的连续区间的**最小值**，极致考验你右指针扩张、左指针收缩的拉链节奏。 |
| **567. Permutation in String**<br>(字符串的排列) | `Medium` | 固定长度滑动窗口 | 它是你做过的 **LeetCode 438（字母异位词）** 的孪生亲兄弟！同样是固定长度为 `len(p)` 的窗口在全场滑动，完美用来复习和巩固 `Counter` 动态更新与清理账本的细节。 |
| **735. Asteroid Collision**<br>(小行星碰撞) | `Medium` | 普通栈 / 对称消除 | 这道题完全不考查大小单调性，纯纯高空轰炸你在笔记里总结的 **“对称消消乐栈”（括号匹配的变形）** 的工程思维。正负数相撞、大小对冲，写起来非常爽快，解耦逻辑很硬核。 |
| **456. 132 Pattern**<br>(132 模式) | `Medium` | 单调栈终极变型 | **单调栈的终极梦魇题**。这道题极其隐蔽且烧脑，用来彻底压榨和检验你对单调栈的理解上限。只要你能独立拿下这道 Medium，你的单调栈内功直接宣布毕业！ |

---

## 🌿 阶段二：全新突破口——二叉树与递归 (5 道新算法题)

| 题号与名称 | 难度 | 核心算法分类 | 今日新知开辟核心（为什么 Pick 它？） |
| :--- | :--- | :--- | :--- |
| **104. Maximum Depth of Binary Tree**<br>(二叉树的最大深度) | `Easy` | 二叉树递归 / DFS | **二叉树和递归板块的绝对母题**！这道题将带你第一次深刻体会什么是高效的“甩手掌柜递归流”——把工作无脑甩给左、右两个副经理，自己最后只加个 1 坐享其成。 |
| **226. Invert Binary Tree**<br>(翻转二叉树) | `Easy` | 二叉树操作 / 递归 | 算法界大名鼎鼎的“面霸梗题”（当年 Homebrew 的作者就是因为白板写不出这道题被大厂挂掉的）。学完甩手掌柜流，我们用几行代码直接一枪秒了它，打破神话！ |
| **100. Same Tree**<br>(相同的树) | `Easy` | 多路同步递归 | 从这道题开始，我们将升级递归形态，教你掌握二叉树的**多路同步递归**。看计算机怎么同时驱使两个指针在两棵不同的树上“神同步”判断它们是否完全对称。 |
| **101. Symmetric Tree**<br>(对称二叉树) | `Easy` | 镜像递归 | **LeetCode 100 题的完美进阶版**。这道题是让一棵树自己跟自己“照镜子”，看看左子树的左边和右子树的右边能不能完美对齐，是二叉树镜像递归的必修课。 |
| **102. Binary Tree Level Order Traversal**<br>(二叉树的层序遍历) | `Medium` | 队列 / 广度优先 (BFS) | 此时此刻，你在笔记里抄录的 **队列 (Queue) 先进先出 (FIFO)** 将迎来它的终极用武之地！带你见识横向“剥洋葱”的广度优先搜索，用双端队列层层通关。 |

### 刷题模块开启

#### 202. Happy Number

`Easy` `Topics` `Companies`

##### 📝 Description

Write an algorithm to determine if a number `n` is happy.

A **happy number** is a number defined by the following process:
1. Starting with any positive integer, replace the number by the sum of the squares of its digits.
2. Repeat the process until the number equals 1 (where it will stay), or it **loops endlessly in a cycle** which does not include 1.
3. Those numbers for which this process **ends in 1** are happy.

Return `true` *if `n` is a happy number, and `false` if not*.

#####  Example 1:
> **Input:** n = 19
> **Output:** true
> **Explanation:**
> - $1^2 + 9^2 = 82$
> - $8^2 + 2^2 = 68$
> - $6^2 + 8^2 = 100$
> - $1^2 + 0^2 + 0^2 = 1$ (Stays at 1, Happy Number!)

#####  Example 2:
> **Input:** n = 2
> **Output:** false
> **Explanation:** The process will loop endlessly into a known sequence: `2 → 4 → 16 → 37 → 58 → 89 → 145 → 42 → 20 → 4...` (Stuck in a cycle without 1)

#####  Constraints:
* $1 \le n \le 2^{31} - 1$

---

#####  💡 Core Strategy (Implicit Fast & Slow Pointers)

Although there are no actual linked list nodes or pointers in the input, the **runtime trajectory** of this problem perfectly mirrors **LeetCode 141 (Linked List Cycle)**.

### The Underlying Graph Theory:
* **The "Next" Pointer**: We treat the process of "calculating the sum of squares of digits" as the `.next` operation.
* **Two Outcomes**: 
  1. **Linear Path**: Eventually hits `1` and stays there (`1 → 1 → 1`).
  2. **Circular Path**: Falls into a dead-end cycle without ever hitting `1` (e.g., the cycle of `4`).

#####  The Double-Speed Race (Floyd's Cycle Detection):
Instead of allocating extra memory using a Hash Set to memorize visited numbers, we can deploy a **Slow Pointer (Turtle)** and a **Fast Pointer (Hare)**:
* **Slow** runs 1 step per turn: `slow = get_next(slow)`
* **Fast** runs 2 steps per turn: `fast = get_next(get_next(fast))`

If there is no cycle, the `fast` pointer will confidently smash the finish line first and become `1`. If there is a hidden cycle, the `fast` pointer will eventually track down and crash into the `slow` pointer from behind (`slow == fast`), triggering our cycle alert.

#####  💻 Python3 Solution

```python
class Solution:
    def isHappy(self, n: int) -> bool:
        # Helper function: acts as the '.next' transition in an implicit linked list
        def get_next(number: int) -> int:
            total_sum = 0
            while number > 0:
                # divmod(x, 10) returns (x // 10, x % 10) elegantly in one shot
                number, digit = divmod(number, 10)
                total_sum += digit ** 2  # ⚠️ Use ** for exponentiation (Python ^ means XOR)
            return total_sum
        
        # Initialize: Slow moves 1 step, Fast moves 2 steps ahead
        slow = n
        fast = get_next(n)
        
        # Keep racing until Fast hits 1 (Happy) OR Fast catches up to Slow (Cycle detected)
        while fast != 1 and slow != fast:
            slow = get_next(slow)            # Turtle takes 1 step
            fast = get_next(get_next(fast))  # Hare takes 2 steps
            
        # If the loop breaks because fast == 1, it's a happy number!
        return fast == 1
```
---

#### 209. Minimum Size Subarray Sum

`Medium` `Topics` `Companies`

##### 📝 Description

Given an array of positive integers `nums` and a positive integer `target`, return *the **minimal length** of a subarray whose sum is greater than or equal to* `target`. If there is no such subarray, return `0` instead.

##### Example 1:
> **Input:** target = 7, nums = [2,3,1,2,4,3]
> **Output:** 2
> **Explanation:** The subarray `[4,3]` has the minimal length under the problem constraint.

##### Example 2:
> **Input:** target = 4, nums = [1,4,4]
> **Output:** 1

##### Example 3:
> **Input:** target = 11, nums = [1,1,1,1,1,1,1,1]
> **Output:** 0

##### Constraints:
* $1 \le \text{target} \le 10^9$
* $1 \le \text{nums.length} \le 10^5$
* $1 \le \text{nums}[i] \le 10^4$

##### 💡 Core Strategy (Adaptive Shrinking Sliding Window)

This problem is the exact inverse of LeetCode 3. Instead of searching for the *maximum* window length under constraint, we are hunting for the **minimum valid window length** where the sum of elements is greater than or equal to `target`.

##### The Greedy "Bulk & Cut" Strategy:
1. **Right Pointer (`right`) Explores (Bulk Up)**: The out loop uses a standard `for` loop to advance the right pointer index. It blindly absorbs new numbers into the window (`currentsum += nums[right]`) until the sum hits or exceeds the target.
2. **Left Pointer (`left`) Shrinks (Weight Loss)**: The moment `currentsum >= target` is satisfied, we officially have a valid window! Since we want the *smallest* length, we immediately trigger a `while` loop to try and shrink the window from the left (`left += 1`).
3. **Record Global Limits**: During the shrinking phase, we continuously record the minimum window size found using `min(min_len, right - left + 1)`. We stop shrinking only when the window sum drops below the `target`.

##### 🚨 Common Pitfalls & Code Mechanics:
* **Loop Control**: Do not use `while right in range(...)` because `while` does not auto-increment indices and will lead to `NameError` or infinite loops. Always stick to `for right in range(len(nums))`.
* **Value vs. Index**: When throwing elements out of the window, ensure you deduct the **actual numerical value** `nums[left]` from `currentsum`, not the index value `left`.

##### 💻 Python3 Solution

```python
class Solution:
    def minSubArrayLen(self, target: int, nums: List[int]) -> int:
        left = 0
        currentsum = 0
        # Initialize with infinity so any valid window size can easily override it
        min_len = float('inf')
        
        # 1. Right pointer expands the window from the right bound
        for right in range(len(nums)):
            currentsum += nums[right]
            
            # 2. As long as the current window satisfies the condition, try to shrink it
            while currentsum >= target:
                # Refresh the record with the smaller length
                min_len = min(min_len, right - left + 1)

                # Evict the leftmost element by value, then advance the left boundary
                currentsum -= nums[left]
                left += 1
        
        # If min_len remains infinity, no valid subarray was found -> return 0
        return min_len if min_len != float('inf') else 0
```

📊 Complexity Analysis

* Time Complexity: $O(n)$Even though there is a while loop nested inside a for loop, look closely at the behavior of the pointers. The right pointer right visits each index exactly once. The left pointer left only moves forward and never backtracks, meaning it also visits each index at most once. The elements are added to the window once and removed at most once, bounding the total operations perfectly to $2n$, which simplifies to linear time $O(n)$.

* Space Complexity: $O(1)$The window states are tracked purely using sliding dynamic pointer indices and a couple of numerical scalar variables (currentsum, min_len). No extra hash arrays or data stores are allocated.

---

# 567. Permutation in String

`Medium` `Topics` `Companies`

## 📝 Description

Given two strings `s1` and `s2`, return `true` *if* `s2` *contains a permutation of* `s1`*, or* `false` *otherwise*.

In other words, return `true` if one of `s1`'s permutations is the substring of `s2`.

### Example 1:
> **Input:** s1 = "ab", s2 = "eidbaooo"
> **Output:** true
> **Explanation:** s2 contains one permutation of s1 ("ba").

### Example 2:
> **Input:** s1 = "ab", s2 = "eidboaoo"
> **Output:** false

### Constraints:
* $1 \le \text{s1.length, s2.length} \le 10^4$
* `s1` and `s2` consist of lowercase English letters.

---

## 💡 Core Strategy (Fixed-Size Sliding Window)

This problem is the exact structural twin of **LeetCode 438 (Find All Anagrams)**. A permutation of `s1` means a contiguous substring in `s2` that contains the exact same characters with the exact same frequencies as `s1`.

### Key Mechanics:
1. **Fixed Window**: The size of our active search window in `s2` must always be equal to `len(s1)`.
2. **Frequency Ledger**: We use `collections.Counter` to maintain character counts.
3. **Eviction Rule (Crucial)**: As the window moves right, we increment the count of the incoming character. When the loop index `i >= len(s1)`, it means the window is overflowing, and the character at `s2[i - len(s1)]` must be evicted. **If its count drops to 0, it must be completely deleted (`del`) from the map**, otherwise dictionary comparison (`count_s2 == count_s1`) will fail due to leftover dummy keys with 0 values.
4. **Early Exit**: The moment `count_s2 == count_s1`, we return `True` immediately without wasting time scanning the rest of the string.

---

## 💻 Python3 Solution

```python
from collections import Counter

class Solution:
    def checkInclusion(self, s1: str, s2: str) -> bool:
        len_1, len_2 = len(s1), len(s2)
        
        if len_1 > len_2:
            return False
            
        count_s1 = Counter(s1)
        count_s2 = Counter()
        
        for i in range(len_2):
            # 1. Slide right: absorb new character
            count_s2[s2[i]] += 1
            
            # 2. Slide left: evict old character when window exceeds fixed bounds
            if i >= len_1:
                left_char = s2[i - len_1]
                if count_s2[left_char] == 1:
                    del count_s2[left_char]  # Clean eviction to pass dictionary equality
                else:
                    count_s2[left_char] -= 1
                    
            # 3. Match Check
            if count_s2 == count_s1:
                return True
                
        return False
```

🎨 核心心法：固定长度滑动窗口（进场与开除）为了不重复计算，我们用一个动态账本（哈希表）来记录窗口里现在都有哪些字母。整个过程就像一个“严格限员的公司”：标准清单（count_p）：老板给的招聘标准（比如 p="abc"，标准就是 $a:1, b:1, c:1$）。新员工进场：右指针 i 每往右走一步，新字母进场，我们在动态账本里给它的数量 +1。老员工被开除（关键点！）：因为窗口长度是固定的。当 i >= len(p) 时，说明公司满员了！每进来一个新员工，最左边那个最早进来的老员工（下标是 i - len(p)）就必须被无情开除，数量 -1。彻底抹除（你的独门细节）：如果被开除的老员工数量减到 0 了，必须用 del 键把它的名字从账本里彻底擦掉！否则字典对比（count_s == count_p）会因为带着一堆 0 从而宣告失败。

---

# 735. Asteroid Collision

`Medium` `Topics` `Companies`

## 📝 Description

We are given an array `asteroids` of integers representing asteroids in a row.

For each asteroid, the absolute value represents its size, and the sign represents its direction (positive meaning right, negative meaning left). Each asteroid moves at the same speed.

Find out the state of the asteroids after all collisions. If two asteroids meet, the smaller one will explode. If both are the same size, both will explode. Two asteroids moving in the same direction will never meet.

### Example 1:
> **Input:** asteroids = [5,10,-5]
> **Output:** [5,10]
> **Explanation:** The 10 and -5 collide resulting in 10. The 5 and 10 never collide.

### Example 2:
> **Input:** asteroids = [5,2,-5]
> **Output:** []
> **Explanation:** The 2 and -5 collide resulting in -5. The 5 and -5 collide resulting in [].

### Example 3:
> **Input:** asteroids = [10,-5]
> **Output:** [10]
> **Explanation:** The 10 and -5 collide resulting in 10.

### Constraints:
* $2 \le \text{asteroids.length} \le 10^4$
* $-1000 \le \text{asteroids}[i] \le 1000$
* $\text{asteroids}[i] \ne 0$

---

## 💡 Core Strategy (The "Pop-Candy" Stack / 消消乐栈)

This problem doesn't require maintaining a sorted monotonic sequence. Instead, it perfectly extends the **"Pop-Candy/Matching" engine** used in standard parenthesis matching (like LeetCode 20).

### The 3 Law of Cosmic Collisions:
1. **Peaceful Cruising**: If asteroids move in the same direction (`+` and `+`, or `-` and `-`), or move away from each other (`-` on left, `+` on right), they will **never** collide.
2. **The Danger Zone**: A collision **only** occurs when a right-moving asteroid (`+`) is inside the stack, and a newly arriving asteroid is moving left (`-`).
3. **Survival of the Fittest**:
   * If the incoming `|left| > |right|`, the stack top explodes (`stack.pop()`). The incoming asteroid keeps crushing previous ones until it meets a larger one, a same-sized one, or the stack empties.
   * If `|left| == |right|`, both annihilate each other.
   * If `|left| < |right|`, the incoming asteroid explodes instantly.

---

## 💻 Python3 Solution

```python
class Solution:
    def asteroidCollision(self, asteroids: List[int]) -> List[int]:
        stack = []
        
        for ast in asteroids:
            # We only trigger a collision loop if the incoming asteroid goes LEFT (-)
            # and the current survivor at the stack top goes RIGHT (+)
            while stack and ast < 0 < stack[-1]:
                # Case 1: Incoming asteroid is LARGER than the stack top
                if abs(ast) > stack[-1]:
                    stack.pop()  # The stack top explodes, loop continues to check next item
                    continue
                
                # Case 2: Both asteroids are EQUAL in size
                elif abs(ast) == stack[-1]:
                    stack.pop()  # Both annihilate each other
                
                # Case 3: Incoming asteroid is SMALLER than the stack top
                # (In both Case 2 and Case 3, the incoming asteroid gets destroyed, stopping the loop)
                break
                
            else:
                # The 'else' block associated with a 'while' loop executes 
                # ONLY if the while loop terminates naturally without hitting a 'break'.
                # This means the incoming asteroid survived all clashes or faced no opposition!
                stack.append(ast)
                
        return stack
```

---

# 456. 132 Pattern

`Medium` `Topics` `Companies`

你这真是直接一脚踩进了单调栈的终极大 Boss 房间！LeetCode 456. 132 模式 (132 Pattern) 被公认为单调栈题型里最绕、最烧脑、也最隐蔽的一道中等题。很多大厂面试官特别喜欢拿它来当压轴题，去测验候选人的算法极限。只要你能把这道题的骨架给拆明白，你的单调栈内功就可以直接原地宣布大圆满毕业！

## 📝 Description

Given an array of `n` integers `nums`, return `true` *if there is a **132 pattern** in* `nums`*, otherwise, return* `false`.

A **132 pattern** is a subsequence of three integers `nums[i]`, `nums[j]` and `nums[k]` such that `i < j < k` and `nums[i] < nums[k] < nums[j]`.

### Example 1:
> **Input:** nums = [1,2,3,4]
| **Output:** false
> **Explanation:** There is no 132 pattern in the sequence.

### Example 2:
> **Input:** nums = [3,1,4,2]
> **Output:** true
> **Explanation:** There is a 132 pattern in the sequence: [1, 4, 2].

### Example 3:
> **Input:** nums = [-1,3,2,0]
> **Output:** true
> **Explanation:** There are three 132 patterns in the sequence: [-1, 3, 2], [-1, 3, 0] and [-1, 2, 0].

### Constraints:
* $n == \text{nums.length}$
* $1 \le n \le 2 \times 10^5$
* $-10^9 \le \text{nums}[i] \le 10^9$

---

## 💡 Core Strategy (倒序单调栈 - 寻找卧底 "2")

This problem asks us to find three numbers such that $i < j < k$ and $\text{nums}[i] < \text{nums}[k] < \text{nums}[j]$. 
Let's rename them to make it simple: `1` is the smallest, `3` is the largest, and `2` is the middleman.

If we look for `1`, `3`, `2` from left to right, the logic gets extremely messy. The golden trick is to **traverse from right to left** and use a **Monotonic Increasing Stack** to maintain candidate values for `3` and `2`.

### The "Spy & Boss" Analogy:
1. **The Middleman `2` (`ak_2`)**: We maintain a variable `ak_2` initialized to $-\infty$. This represents the largest possible value for the "2" position we have encountered so far from the right side.
2. **The Big Boss `3` (Stack Top)**: The stack maintains potential candidates for the "3" position in a strictly decreasing order (from bottom to top).
3. **The Clash**: As we scan from right to left, if the current number `nums[i]` is **larger** than the stack top, it means this new number wants to be the Big Boss `3`. 
   * It will fiercely kick out (pop) all smaller elements from the stack.
   * **Crucial Twist**: The elements being kicked out are smaller than the new `3`, but they came from the right side of `3`! This makes them perfect candidates for position `2`! We update `ak_2` with the largest popped value.
4. **The Victory `1`**: After updating `ak_2`, if we ever find a current number `nums[i]` that is **strictly smaller** than `ak_2`, we instantly win! Because `nums[i]` (which is `1`) $< ak_2$ (which is `2`) $< \text{stack top}$ (which is `3`). A perfect 132 pattern is found!

💡 核心通关秘籍（倒序单调栈 - 极限寻找卧底 “2”）

如果顺着从左往右找，你会陷入“既要盯着小的，又要找大的，还要防着后面不高不矮的”死胡同，脑浆子都会看出来。

这道题的黄金解法是：**从右往左（倒着）遍历数组**，并维护一个**单调递增栈**。

### 🎬 极其生动的“抓内鬼”大白话情景剧：
1. **中介 `2` 号位（用变量 `ak_2` 表示）**：我们初始化它为 `-无限大`。它代表我们从右边一路走过来，能抓到的**最大的“中介/二号位”**。
2. **老大 `3` 号位（栈顶元素）**：单调栈里从栈顶到栈底，死死存着从大到小的“老大候选人”。
3. **疯狂踢人与抬高门槛**：我们倒着往左走，每遇到一个新数字：
   * 如果这个新数字比栈顶元素还要**大**，说明这家伙有实力当“老大 `3` 号位”！
   * 他会一脚把栈里比他小的元素全轰出去（`stack.pop()`）。
   * **灵魂魔改点**：被他轰出去的这些小弟，因为本来就待在右边，现在又比新老大矮，这不就是天然的**“中介 `2` 号位”**吗？！
   * 我们让 `ak_2` 继承这些被轰走的人里最大的那个。`ak_2` 被抬得越高，后面的数字想要小于它，就越容易！
4. **一枪秒杀**：在我们更新完 `ak_2` 之后，只要发现当前左边的数字 `nums[i]` **严格小于 `ak_2`**，直接宣告通关！因为这一瞬间：
   $$\text{当前数字 (1号位)} < ak\_2 \text{ (2号位)} < \text{栈顶元素 (3号位)}$$
   连坐关系完美成立！

---

## 💻 Python3 Solution

```python
class Solution:
    def find132pattern(self, nums: List[int]) -> bool:
        n = len(nums)
        if n < 3:
            return False
            
        stack = []         # Monotonic stack storing candidates for '3'
        ak_2 = float('-inf')  # The value of '2', maximized from popped elements
        
        # Traverse the array backwards (from right to left)
        for i in range(n - 1, -1, -1):
            # 1. Check if we found a valid '1'
            # If the current number is smaller than our established '2', 
            # and since '2' only exists if it was popped by a larger '3', 
            # we automatically satisfy 1 < 2 < 3!
            if nums[i] < ak_2:
                return True
                
            # 2. Maintain Monotonic Stack: if current number is larger than stack top,
            # it qualifies as a better/larger '3'.
            while stack and nums[i] > stack[-1]:
                # The elements kicked out by '3' become candidates for '2'.
                # We take the maximum to make it easier for future numbers to be smaller than '2'.
                ak_2 = stack.pop()
                
            # 3. Push the current number as a candidate for '3'
            stack.append(nums[i])
            
        return False
```

📊 Complexity Analysis

* Time Complexity: $O(n)$ — We traverse the array of length $n$ exactly once from right to left. Each element is pushed onto the stack once and popped at most once, which bounds the inner while loop to a cumulative amortized cost of $O(n)$.

* Space Complexity: $O(n)$ — In the worst-case scenario (e.g., a strictly decreasing array), no elements will be popped, and the stack will grow to store all $n$ elements.

---

# 456. 132 模式

`中等` `单调栈` `经典大厂压轴题`

## 📝 题目描述

给你一个整数数组 `nums` ，请你判断这里面是否存在一个 **132 模式** 的子序列。

**大白话解释：** 让你在数组里找出三个数，它们的下标满足 `i < j < k`（从左到右的相对顺序不变），但是它们的值要满足：**第一个数 < 第三个数 < 第二个数**。

为了好记，我们直接给这三个位置起个外号：
* **`1` 号位**：全场最小的弟弟（在最左边）。
* **`3` 号位**：全场最大的老大（在中间）。
* **`2` 号位**：不高不矮的中介/卧底（在最右边）。

符合 132 模式就意味着我们要找到一组关系：**`弟弟 (1) < 中介 (2) < 老大 (3)`**。

### 示例 1：
> **输入：** nums = [1, 2, 3, 4]
> **输出：** false
> **解释：** 数组全是升序的，根本凑不出先变大再变小的 132 组合。

### 示例 2：
> **输入：** nums = [3, 1, 4, 2]
> **输出：** true
> **解释：** 找到了一个完美的 132 模式子序列：`[1, 4, 2]`。（1 是弟弟，4 是老大，2 是中介）。

## 💡 核心通关秘籍（倒序单调栈 - 极限寻找中介 “2”）

如果顺着从左往右找，你会陷入“既要盯着小的，又要找大的，还要防着后面不高不矮的”死胡同。
这道题的黄金解法是：**从右往左（倒着）遍历数组**，并维护一个**单调递减栈**（从栈底到栈顶由大到小）。

### 🎬 单调栈内的“权力更迭”情景剧：
1. **中介 `2` 号位（用变量 `ak_2` 表示）**：我们初始化它为 `-float('inf')`。它代表我们从右边一路走过来，能抓到的**最强、个头最高的“中介 2 号位”**。
2. **老大 `3` 号位（栈顶元素 `stack[-1]`）**：单调栈里存着目前从右往左看，所有有潜力当老大的候选人。
3. **恶霸进门与抬高门槛：**
   * 我们倒着往左走，新来的数字 `nums[i]` 如果比栈顶元素 `stack[-1]` 还要大，说明它比现任老大更猛，触发 `while` 循环开始轰人。
   * 它会一脚把栈里比它矮的元素全弹出去（`stack.pop()`）。
   * **核心逆转：** 被它轰出去的这些前任老大，因为本来就待在右边，现在个头又被新老大踩在脚底下，这不就是天然的**“中介 2 号位”**吗？！
   * 我们让 `ak_2` 继承这些被轰走的人里最大的那个。`ak_2` 作为门槛被抬得越高，后面左边的数字想要小于它（当弟弟 1 号位），就越容易！
4. **一枪绝杀：**
   * 轰完人后，新恶霸作为最新最强老大高调入栈（`stack.append(nums[i])`）。
   * 此时，只要发现当前最新的左边数字 `nums[i]` **严格小于 `ak_2`**，直接宣告通关！因为这一瞬间：
     $$\text{当前数字 (1号位弟弟)} < ak\_2 \text{ (2号位中介)} < \text{栈顶元素 (3号位老大)}$$
     连坐关系完美成立，躺赢！

## 💻 完美 Python3 中文通关代码

```python
class Solution:
    def find132pattern(self, nums: List[int]) -> bool:
        # 这道题很有意思，如果想起出逻辑了其实并不难
        n = len(nums)
        
        # 1. 进场先防御，长度不够 3 直接回绝
        if n < 3:
            return False
        
        # 2. 规范初始化：统一定义在顶层，彻底杜绝 NameError 作用域报错
        stack = []             # 单调栈：用来存“老大 3 号位”的候选人（从底到顶递减）
        ak_2 = float('-inf')   # 变量：用来存全场最优秀的“中介 2 号位”
        
        # 3. 从后向前索引（倒序遍历）
        for i in range(n - 1, -1, -1):

            # 绝杀瞬间：如果发现当前最左边的弟弟（1号位）比中介（2号位）还要小，直接躺赢！
            if nums[i] < ak_2:
                return True
            
            # 维护单调栈：如果当前数字比【栈顶元素】还要大，说明它更适合当老大（3号位）
            while stack and nums[i] > stack[-1]:
                # 老大进门轰人，被轰出来的小弟降级成了“中介 2 号位”的完美替补
                # 我们用 max 拼命抬高 ak_2 的门槛水位，给左边的 1 号位弟弟创造机会
                ak_2 = stack.pop()
            
            # 把当前最新的老大候选人（3号位）压进 stack 顶端老实排队
            stack.append(nums[i])
            
        return False
```
📊 复杂度深度分析

* 时间复杂度：$O(n)$虽然代码结构上是 for 循环里嵌套了 while 循环，但仔细观察就会发现，整个数组里的每个数字最多只会进栈一次（append），也最多只会被轰出来一次（pop）。全场总计的栈操作上限是 $2n$ 次，平摊到每一步后，是一个货真价实、速度极快的线性时间复杂度 $O(n)$。

* 空间复杂度：$O(n)$在最极端恶劣的无碰撞情况下（例如输入数组本来就是严格递减的 [4, 3, 2, 1]），由于新来的数总比栈顶小，谁也轰不走谁，单调栈会一路憋到最大，装满全部 $n$ 个元素。

---

# 二叉树的新大陆地图开启！！！
恭喜你正式跨入算法面试的半壁江山——二叉树（Binary Tree）的新大陆！如果说数组和链表考的是你的“手速和细节”（各种指针挪来挪去、缩进错位），那二叉树考的就是你的“大局观和工程思维”。搞定二叉树，核心就在于掌握一套全新的内功心法：“甩手掌柜递归流”。

# # 104. 二叉树的最大深度

`简单` `二叉树` `递归` `精选基础题`

## 📝 题目描述

给定一个二叉树的根节点 `root`，找出并返回其 **最大深度**。

二叉树的 **最大深度** 是指从根节点到最远叶子节点的最长路径上的节点数。

### 示例 1:
> **输入：** root = [3, 9, 20, null, null, 15, 7]
> **输出：** 3

### 示例 2:
> **输入：** root = [1, null, 2]
> **输出：** 2

### 提示:
* 树中节点的数量在 $[0, 10^4]$ 区间内。
* $-100 \le \text{Node.val} \le 100$

---

## 💡 核心心法：甩手掌柜递归流（自底向上）

面对二叉树，千万不要试图用大脑去模拟每一层的复杂跳跃，那样极其容易 CPU 烧完。我们要学会当一个高明的 **“甩手掌柜”**：

假设你是 **大老板（根节点 `root`）**，你想知道以你为首的这棵大树有多深：
1. **向下派发任务（递归子问题）**：你把左副总（`root.left`）和右副总（`root.right`）叫到办公室，跟他们说：“你们各自去统计一下你们管辖的那半边树的最大深度。”
2. **副总们汇报结果（等待递归返回）**：左副总回来汇报说他的深度是 `L`，右副总汇报说是 `R`。
3. **老板做决策（总结归纳）**：作为大老板，你只需要挑一个最深的半边，然后**加上你坐在办公室里的这 1 层**，就是全场的最高纪录！
   * **决策公式：**
     $$\text{最大深度} = \max(L, R) + 1$$
4. **防御线（递归出口）**：如果一个副总手底下根本没人（空节点 `None`），那他管辖的深度自然就是 `0`。

你看，每个节点做的事情一模一样：**问左右要结果 $\rightarrow$ 挑个大的 $\rightarrow$ 加个 1 往上交**。这就是递归的魅力！

---

## 💻 干净漂亮的 Python3 通关代码

二叉树的递归代码往往短得惊人，核心逻辑其实只要三行：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def maxDepth(self, root: Optional[TreeNode]) -> int:
        # 1. 防御线：如果碰到了空节点，说明到头了，深度为 0
        if not root:
            return 0
        
        # 2. 甩手掌柜：派发任务给左右子树，各自去算深度
        left_depth = self.maxDepth(root.left)
        right_depth = self.maxDepth(root.right)
        
        # 3. 老板决策：挑个最深的，加上当前自己这一层（+1），得意地往上汇报
        return max(left_depth, right_depth) + 1
```
# 💡 编程心法：递归 (Recursion) vs. 嵌套 (Nesting)

> **核心结论：** 它们描述的是两个不同维度的概念。大白话来说——**它是通过“函数的嵌套调用”，来实现“算法的递归思想”。**

---

## 1. 为什么它叫“递归 (Recursion)”？（—— 算法思想）

**递归是一种“大局观”的编程技巧。** 它的核心特征是：**函数自己调用自己**。
它把一个庞大的惊天大问题，拆解成一个个“长得一模一样，但规模更小”的子问题。

* **在 LeetCode 104 题（最大深度）里**：
  你想求整棵大树的深度（`self.maxDepth(root)`），你发现这个大问题可以拆成求左子树深度（`self.maxDepth(root.left)`）和右子树深度（`self.maxDepth(root.right)`）。
* **套娃分发模式**：
  这种大老板把任务套娃式地分发给小老板，小老板又分发给组长，直到基层员工（空节点 `None`）的模式，就叫**递归**。

---

## 2. 为什么它也是“嵌套 (Nesting)”？（—— 执行机制）

**嵌套是指一个东西里面包含着另一个东西。** 在计算机执行这段代码的底层，它采用的是**“函数嵌套调用”**。

* **卡住进度，层层深入**：
  当你调用 `maxDepth(root)` 时，这个函数还没有运行完，它内部就又调用了 `maxDepth(root.left)`。此时，计算机必须把第一层函数的进度“卡住”，先去执行第二层函数；第二层函数还没完，可能又卡住去执行第三层……
* **系统调用栈 (Call Stack)**：
  这就叫嵌套。在计算机里，这种嵌套调用是由一个专门叫 **“系统调用栈”** 的容器在内存里默默管理的。

---

## 🧱 形象的小比喻：俄罗斯套娃 vs. 剥洋葱

| 概念 | 形象比喻 | 动态拆解 |
| :--- | :--- | :--- |
| **嵌套 (Nesting)** | **俄罗斯套娃** | 你看到一个巨大的木偶，打开它，里面嵌套着一个一模一样的中木偶；再打开，里面又嵌套着一个小木偶。这种**空间上的层层包裹、大套小**，就是嵌套。 |
| **递归 (Recursion)** | **剥洋葱 / 探险** | 为了拿到洋葱心里的秘密，你必须剥开外面这一层（**递**：向深处走）；剥到最核心、没有皮的时候（**递归出口**），你拿到了秘密，然后你要一层一层把皮捡回来重新包好（**归**：原路返回，把结果一层层往上交）。这种**有去有回、自己解决自己**的动态过程，就是递归。 |

---

## 🎯 总结

* **嵌套**：是它的**外表和动作**（函数一层套一层地卡在内存里）。
* **递归**：是它的**灵魂和目的**（自己调用自己，用套娃的方式把大树给数完了）。

这完全是一段**利用了函数嵌套执行的经典递归代码**！

---

# 226. 翻转二叉树

`简单` `二叉树` `递归` `谷歌面试名梗题`

## 📝 题目描述

给你一棵二叉树的根节点 `root` ，翻转这棵二叉树，并返回其根节点。

**大白话解释：** 所谓的翻转二叉树，其实就是给整棵树**照镜子（镜像对称）**。把所有节点的左子树和右子树全部对调。

### 示例 1：
> **输入：** root = [4, 2, 7, 1, 3, 6, 9]
> **输出：** [4, 7, 2, 9, 6, 3, 1]
> **解释：**
> 原树：
>       4
>     /   \
>    2     7
>   / \   / \
>  1   3 6   9
>
> 镜像翻转后：
>       4
>     /   \
>    7     2
>   / \   / \
>  9   6 3   1

---

## 💡 核心通关秘籍（甩手掌柜递归流 - 自顶向下交换）

有了 104 题的基础，咱们继续把**“甩手掌柜”**的格局打开！

假设你是**大老板（根节点 4）**，你想把整棵大树全部镜像翻转：
1. **向下派发任务（递归子问题）**：你把左副总（2 号节点）和右副总（7 号节点）叫到办公室，跟他们说：“你们各自去把你们管辖的那半边树，内部先全部翻转好。”
2. **副总们执行完毕（等待递归返回）**：左副总和右副总执行力惊人，很快把它们各自手底下的人全翻转完了。
3. **老板做决策（主逻辑交换）**：作为大老板，你看到两边内部都翻转好了，你最后只需要做一件事——**伸出双手，把左副总和右副总的职位（指针位置）互换一下！**
4. **防御线（递归出口）**：如果你手底下根本没人（空节点 `None`），直接返回 `None` 结束。

你看，每个节点做的事情一模一样：**把左边交出去翻转 $\rightarrow$ 把右边交出去翻转 $\rightarrow$ 交换左右子树 $\rightarrow$ 返回自己**。

---

## 💻 完美 Python3 中文通关代码

在 Python 中，交换两个变量甚至不需要第三个临时变量，一句 `a, b = b, a` 优雅搞定，所以代码短得不可思议：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right
class Solution:
    def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
        if not root:
            return None
        root.left, root.right = self.invertTree(root.right), self.invertTree(root.left)
        return root
```
Question:

class Solution:
def invertTree(self, root: Optional[TreeNode]) -> Optional[TreeNode]:
if not root:
 return None
root.left = self.invertTree(root.right)
root.right = self.invertTree(root.left)
return root
哪错了。。。 

🚨 现场还原：大老板在第 2 行“失忆”了
我们直接锁定你引发逻辑风暴的这两行代码：

```Python
        # 🚨 第一步：把右边翻转后的结果，直接赋给了左边
        root.left = self.invertTree(root.right)
        
        # 🚨 第二步：你想把左边翻转后的结果，赋给右边
        root.right = self.invertTree(root.left) 
```

发生了什么？：
当程序运行完第一行 root.left = self.invertTree(root.right) 时，原来的老左树（root.left）已经被你用新右树的结果给无情覆盖掉了！

致命的第二步：
紧接着，你执行第二行去算 self.invertTree(root.left)。此时，计算机去抓 root.left，抓到的其实是刚刚在第一行被你换过来的、已经翻转好的新右树！

可怕的后果：
你相当于把右树翻转了整整两次，并把它同时装在了左边和右边。而你真正的老左树，在第一步执行完的一瞬间，就彻底在内存里蒸发消失了！

---

# 100. 相同的树

`简单` `二叉树` `递归` `精选高频题`

## 📝 题目描述

给你两棵二叉树的根节点 `p` 和 `q` ，编写一个函数来检验这两棵树是否相同。

如果两个二叉树在结构上相同，并且节点具有相同的值，则认为它们是相同的。

### 示例 1：
> **输入：** p = [1,2,3], q = [1,2,3]
> **输出：** true
> **解释：** 两棵树的长相和对应节点的值完全一致。

### 示例 2：
> **输入：** p = [1,2], q = [1,null,2]
> **输出：** false
> **解释：** 结构对不上，一棵树的 2 在左边，另一棵在右边。

---

## 💡 核心通关秘籍（双线安检递归流 - 同步对比）

这次我们要同时对比两棵树，这就好比海关有两个安检通道（`p` 和 `q`），我们要保证每一级进来的人（节点）都完全对得上。

依然由大老板（当前节点）出面，同时检查 `p` 和 `q`：

### 🚨 严丝合缝的 3 道安检防御线：
1. **两边都是空的**（`not p and not q`）：两边都没人，这叫“完美的空虚”，说明这一路比对到头了都完全一样，**安全放行，返回 `True`**。
2. **一边空，一边不空**（`not p or not q`）：如果走到某一步，`p` 通道有人，`q` 通道居然是个空位（或者反过来）。结构当场对不上，**立马逮捕，返回 `False`**。
3. **两边都有人，但长相不同**（`p.val != q.val`）：虽然两个节点都存在，但是里面的数值不一样（比如一个装 1，一个装 2），**立马逮捕，返回 `False`**。

### 👔 甩手掌柜的分发：
如果上面 3 道安检都顺利通过，说明当前这一层的两个节点是完全合格且相等的！
接下来大老板继续甩手：
* 让左副总去同步比对两棵树的左边：`self.isSameTree(p.left, q.left)`
* 让右副总去同步比对两棵树的右边：`self.isSameTree(p.right, q.right)`
* **全场通关条件：** 必须左边完全相同，**且（`and`）** 右边也完全相同，整棵树才算真正相同！

---

## 💻 完美 Python3 中文通关代码

二叉树的递归防御一旦写工整，逻辑简直赏心悦目：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isSameTree(self, p: Optional[TreeNode], q: Optional[TreeNode]) -> bool:
        # 🚨 第一道防御线：如果两棵树都走到了空节点，说明一路比对过来完全一样，返回 True
        if not p and not q:
            return True
            
        # 🚨 第二道防御线：如果其中一棵树为空，另一棵不为空，结构对不上，返回 False
        if not p or not q:
            return False
            
        # 🚨 第三道防御线：如果两个节点都有值，但值不相等，返回 False
        if p.val != q.val:
            return False
            
        # ⚙️ 甩手掌柜分发：当前层合格！同时把左边交给左副总，右边交给右副总，两边都必须为 True
        return self.isSameTree(p.left, q.left) and self.isSameTree(p.right, q.right)
```

---

# 101. 对称二叉树

`简单` `二叉树` `递归` `行业高频必刷题`

## 📝 题目描述

给你一个二叉树的根节点 `root` ，检查它是否轴对称（即：**互为镜像**）。

### 示例 1：
> **输入：** root = [1, 2, 2, 3, 4, 4, 3]
> **输出：** true
> **解释：** 
>        1
>      /   \
>     2     2
>    / \   / \
>   3   4 4   3
> 从中间画一条轴，左边和右边像照镜子一样完全对称。

### 示例 2：
> **输入：** root = [1, 2, 2, null, 3, null, 3]
> **输出：** false
> **解释：** 结构对不上，不是完美的镜像。

---

## 💡 核心通关秘籍（双指针探戈递归流 - 外侧对外侧，内侧对内侧）

这道题如果只给一个指针，你根本没办法同时看左边和右边。
所以，黄金突破口是：**大老板（根节点）以自身为中心，派出两个分身（左副总 `left` 和 右副总 `right`），让他们开始同步“照镜子”比对。**

既然是照镜子（镜像），那这两个分身在往下走的时候，舞步必须是**相反且对称**的，就像在跳一场严丝合缝的探戈：

### 🚨 3 道严密的镜像安检防线：
1. **两边都走到了空**（`not left and not right`）：两边都没人，完美对称，放行返回 `True`。
2. **一边空，一边不空**（`not left or not right`）：两边结构对不上，立马逮捕返回 `False`。
3. **两边都有人，但长相不同**（`left.val != right.val`）：数值对不上，立马逮捕返回 `False`。

### 💃 镜像分发的灵魂舞步（注意看指针搭配！）：
当前层通过安检后，大老板指挥两个分身继续往深处走。因为要互为镜像，所以：
* **外侧比外侧：** 左分身的左手边，必须等于右分身的右手边！$\rightarrow$ `check(left.left, right.right)`
* **内侧比内侧：** 左分身的右手边，必须等于右分身的左手边！$\rightarrow$ `check(left.right, right.left)`
* **通关条件：** 必须外侧对称，**并且（`and`，吸取上一题的教训！）** 内侧也对称，整棵树才是对称的！

---

## 💻 完美 Python3 中文通关代码

我们在主函数里先做个防御，然后通过一个辅助函数 `check` 展开双线比对：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        # 如果整棵树本来就是空的，它自己对自己当然也是对称的
        if not root:
            return True
            
        # 大老板派出左、右两个分身，开始跳“镜像探戈”
        return self.check(root.left, root.right)

    def check(self, left: Optional[TreeNode], right: Optional[TreeNode]) -> bool:
        # 🚨 防线 1：两个分身同时走到空，说明这一路比对都完美对称
        if not left and not right:
            return True
            
        # 🚨 防线 2：一个空一个不空，结构不对称，枪毙
        if not left or not right:
            return False
            
        # 🚨 防线 3：两个都有值，但值不相等，不是镜像，枪毙
        if left.val != right.val:
            return False
            
        # ⚙️ 镜像分发：外侧对外侧（left.left vs right.right） 
        #              AND 内侧对内侧（left.right vs right.left）
        return self.check(left.left, right.right) and self.check(left.right, right.left)
```

📊 复杂度深度分析

* 时间复杂度：O(n)
整棵树一共有 n 个节点，我们的两个分身会以同步的速度把全场每个节点刚好访问一遍，耗时和节点总数呈完美线性关系。

* 空间复杂度：O(h)
h 是树的高度。底层系统的嵌套递归调用栈占用的深度取决于树的高度。在最坏情况下（树退化成了一条长长的链表），空间复杂度为 O(n)。

Question: 这里为什么还要定义一个新的function？嵌套本身不好吗

Answer: 不是嵌套调用不好，而是主函数 isSymmetric(self, root) 原始带的参数太少了，它只有一个 root。单凭一个指针，在树里根本无法施展“左右分头行动”的镜像对比！

我们必须定义一个新函数（比如 check(left, right)），是为了强行给它扩充参数，开辟双线作战的战场。

咱们把这里面的工程细节彻底揉碎了看：

🚨 1. 为什么不能直接嵌套调用主函数自己？
我们来尝试一下，如果像前几道题一样，不定义新函数，直接强行在主函数里自己嵌套自己：

```python
class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
```
        # 🚨 灾难发生了：你想让左边和右边去比对
        # 但是 isSymmetric 规定了只能传【一个】参数！
        # 你没办法写成 self.isSymmetric(root.left, root.right) ❌（会爆参数个数不匹配报错）
        
        # 如果你写成这样：
        left_res = self.isSymmetric(root.left)
        right_res = self.isSymmetric(root.right)
        # 🚨 这意味着左子树自己跟自己比，右子树自己跟自己比。
        # 它们各自内部是对称的，不代表它们【互为镜像】啊！比如左边是[3,4]，右边也是[3,4]，各自都不对称，但整体是对称的。
看到了吧！因为主函数的接口是 LeetCode 死死固定好的，它只接收一个 root。当你需要让左边和右边这两个不同的节点同步结对往下走的时候，一个参数的函数根本接不住这两个变量。

🛠️ 2. 新定义一个函数，到底爽在哪里？
新定义的 check(left, right) 函数，本质上是一个“参数改装器”。

主函数 isSymmetric 的职责：它是个大门迎宾员。它只负责接住最初的那一个大根节点 root。如果根节点不为空，它就把 root.left 和 root.right 两个打包好，一把扔进改装后的双参数战场。

新函数 check 的职责：它是真正的双线安检员。它有两只手（left 和 right），左手抓着左子树，右手抓着右子树，从而可以完美实现：
self.check(left.left, right.right)（外侧对比）
self.check(left.right, right.left)（内侧对比）

💡 3. 江湖高级流派：如果不喜欢多写个函数，能不能玩真正的“纯嵌套”？
老铁，既然你提到了“嵌套本身不好吗”，Python 还真有一种超级炫酷、极为闷骚的写法——“闭包（内嵌函数）”！

Q: 哪错了？

```Python
# Definition for a binary tree node.
# class TreeNode:
# def __init__(self, val=0, left=None, right=None):
# self.val = val
# self.left = left
# self.right = right

class Solution:

def isSymmetric(self, root: Optional[TreeNode]) -> bool:
if not root:
return True
return check(self.root.left, self.root.right)

def check(self, left, right):
if not left and not right:
return True
if not left or not right:
return False
if left.val != right.val:
return False
return check(left.left, right.right) and check(left.right, right.left)
```


A: Python 肯定无情地抛出了两个报错：一个是 AttributeError: 'Solution' object has no attribute 'root'，另一个是 NameError: name 'check' is not defined。

这两处卡壳不是因为你算法错了，而是因为在面大厂时最容易手抖敲错的两个 Python 关键字（self）的遗漏。

咱们直接来把这两个小漏洞当场揪出来：

🚨 错位点一：误加了 self.root
```Python
return check(self.root.left, self.root.right)
```

# 🚨 报错：'Solution' 没有 root
为什么错？：主函数传进来的参数名字直接叫 root。它是一个局部变量。而当你写成 self.root 时，Python 会以为你想去访问整个 Solution 类里面自带的全局属性，但这个类里根本没有定义过叫 root 的全局变量。

怎么改？：把前面的 self. 删掉，直接用传入的参数：check(root.left, root.right)。

🚨 错位点二：调用类方法时漏掉了 self.
```Python
return check(root.left, root.right)  # 🚨 报错：name 'check' is not defined
# ...
return check(left.left, right.right) and check(left.right, right.left)
```

# 🚨 同样报错
为什么错？：因为你的 check 函数是和 isSymmetric 平级定义在 Solution 类底下的。在 Python 的面向对象语法里，类里面的亲兄弟函数想要互相打招呼，必须抬出 self. 大旗！如果你不加 self.，Python 会在全场最外层去找一个叫 check 的普通独立函数，找不到就会报 NameError。

怎么改？：只要是在类里调用 check，前面一律老老实实加上 self.，也就是：self.check(...)。

🛠️ 完美的修复版（补齐 self 召唤术）
我们只需要把那几处 self. 归位，并修正 root 的访问方式，你的这段双函数流代码就彻底完美无缺了：

```Python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def isSymmetric(self, root: Optional[TreeNode]) -> bool:
        if not root:
            return True
        # ✅ 修复：直接用参数 root（去掉 self.），但调用 check 必须加 self.
        return self.check(root.left, root.right)
    
    # 因为是类的方法，记得把 self 传进来（你这里已经写对了，很棒！）
    def check(self, left, right):
        if not left and not right:
            return True
        if not left or not right:
            return False
        if left.val != right.val:
            return False
        # ✅ 修复：类内互相嵌套调用，前面必须加 self.
        return self.check(left.left, right.right) and self.check(left.right, right.left)
```
老铁，Python 类里这个 self. 确实像个黏人的小尾巴，漏掉任何一个都会引发全场报错。现在把这几个 self. 补齐，再次点击提交，绿色的 Accepted 绝对老老实实交出来！

101 题对称树关卡也彻底无伤通过了。

---

# 广度优先算法BFS

# 💡 算法核心：什么是 BFS（广度优先搜索）？

> **一句话总结：** 广度优先搜索（Breadth-First Search，简称 BFS），用四个字概括就是 **“平推扫描”**。
> 它的核心思想是：**从起点开始，先把它周围最近的邻居全部访问一遍，然后再去访问邻居的邻居，以此类推，像波纹一样一圈一圈地向外扩散。**

---

## 🎬 三个极其生动的生活大比喻

* 🌊 **丢石头（水波纹效应）**：
  你往平静的湖里丢一颗石头（起点），水波纹并不是直接扎进水底，而是**一圈一圈、均匀地、由近及远**地向四周扩散。这就是标准的 BFS。
* 🔍 **地毯式搜救（扇形推进）**：
  搜救队在树林里找人，大家排成一排（横向一整层），齐头并进向前推进。只有把眼前的这一横排树林搜完了，才会向前迈出下一步。绝不会有一个人自己先单独冲到树林最深处。
* 👑 **古代皇帝选妃（按关系远近）**：
  先考察身边关系最近的（第一圈：皇后、贵妃），再考察稍微远一点的（第二圈：常在、答应），最后才轮到宫外的。

---

## 🛠️ BFS 的终极神器：队列（Queue）

想要完美实现“地毯式平推”，大脑必须记住“接下来该轮到谁了”。BFS 绑定使用的底层核心武器就是 **队列（Queue）**。



队列的铁律是：**先进先出（FIFO - First In, First Out）**，就像我们在食堂排队打饭一样，先排队的人先吃，新来的人只能老老实实排在队尾。

### 🏃‍♂️ BFS 的标准运作剧本
1. **开局**：准备一个队列，把 **起点** 扔进去排队。
2. **循环平推（只要队列里有人，就一直重复）**：
   * 从队头 **弹出一个老哥**，表示正式访问他。
   * 看看这个老哥 **身边有哪些还没访问过的亲戚邻居**？
   * 把这些亲戚邻居一股脑全抓过来，**送到队尾去老老实实排队**。
3. **收工**：当队列变空时，说明全场所有能走到的人全被“排队消消乐”消耗光了，平推结束！

---

## 💻 BFS 标准通关模板（以二叉树层序遍历为例）

老铁，记住这个模板！以后不管遇到什么 BFS 题目，90% 的核心骨架都是直接套用这段代码：

```python
from collections import deque

class Solution:
    def levelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        # 1. 进来先防御，如果是空树，直接返回空列表
        if not root:
            return []
        
        result = []
        queue = deque([root])  # 初始化队列，让根节点率先入队

        # 2. 只要队列不空，平推扫描就不停
        while queue:
            # 💡 核心精髓：提前拍照，锁死当前这一层一共有多少个节点
            queue_size = len(queue)
            current_level = []

            # 3. 严格只执行 queue_size 次，保证只消耗当前层的员工
            for _ in range(queue_size):
                # 🚨 避坑指南：popleft 是个函数，必须加小括号 () 才能把节点真正弹出来！
                node = queue.popleft()
                current_level.append(node.val)

                # 顺藤摸瓜：把他的下一代家属（左、右孩子）送到队尾去排队
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)

            # 4. 当前层全员消耗完毕，小账本打包送入最终大账本
            result.append(current_level)
            
        return result
```

---

# 2026/05/26

# 🗺️ 算法通关大盘点：已锁定的神兵与未知的世界版图

> **复盘寄语：** 纵观你笔记里这洋洋洒洒十几道硬核经典题，你其实已经在不知不觉中，把算法面试中出镜率最高、最核心的**五大基本流派**全部通关了！从手速细节到大局观思维，你的兵器库已经非常全面。

## 🏆 阶段一：你的算法战绩全景大盘点（已精通流派）

### 1. 哈希表与空间换时间流（—— 备忘录思维）
* **经典代表题**：`1. Two Sum`、`496. 下一个更大元素 I`（解耦查询流）
* **你掌握的核心**：乱序数组无法双指针包抄时，用字典当“记账本”，把 $O(n^2)$ 的暴力查找用 $O(1)$ 的哈希查找瞬间干掉。

### 2. 滑动窗口流（—— 连续区间扫描器）
* **固定窗口**：`438. 字母异位词`、`567. 字符串排列`
  * **你的独门细节**：窗口限员，左边开除老员工时数量归零**必须用 `del` 彻底擦除键**，否则带着值为 `0` 的 Dummy Key 对比字典必翻车。
* **不定长窗口**：`3. 最长无重复子串`、`209. 长度最小的子数组`
  * **你的独门细节**：掌握了“单向拉链法则”，用 `>= left` 护体死死锁住左指针，**绝不让 `left` 往回跳**去捡已被淘汰的脏数据。

### 3. 快慢与相向双指针流（—— 线性时空收割机）
* **快慢指针（同向）**：`141. 环形链表`、`202. 快乐数`（隐式套圈）、`283. 移动零`（质检员与搬运工）
  * **你掌握的核心**：用速度差在操场玩“套圈消消乐”，有环必相遇，无环直奔终点。原地整理时，快指针当质检员，慢指针控坑交换。
* **对撞指针（相向）**：`167. 两数之和 II`、`11. 盛最多水的容器`、`15. 三数之和`、`42. 接雨水`
  * **你掌握的核心**：利用“数组有序性”或“木桶效应（短板限制）”进行贪心收缩。宽度注定减小时，**每次必须无脑移动较矮的柱子**，才有可能赌到一个更高的未来。

### 4. 栈与队列两大底层容器（—— 时空操纵者）
* **普通栈/消消乐栈**：`735. 小行星碰撞`、以及你总结的括号匹配和表达式计算。
  * **你掌握的核心**：利用“碰撞/消消乐引擎”，处理正负对冲与对称消除。
* **单调栈（右边大佬轰人流）**：`739. 每日温度`、`503. 循环数组取模`、`456. 132模式`（倒序单调栈大 Boss）
  * **你掌握的核心**：小黑屋严格保持大小排队，大个子进门轰人并顺手结算。`132模式` 采用倒序遍历，用变量 `ak_2` 继承被弹小弟来极限抬高门槛。
* **队列/广度优先搜索 (BFS)**：`102. 二叉树的层序遍历`
  * **你的独门细节**：横向水波纹平推。在 `while` 循环里**必须提前拍照锁死 `queue_size` 发放限定数量的入场券**，把当前层和下一层家属彻底隔离；同时死死记住了 `popleft()` **必须加小括号**的 Python 铁律。

### 5. 二叉树与 DFS 递归（—— 大局观甩手掌柜）
* **经典代表题**：`104. 最大深度`、`226. 翻转二叉树`、`100. 相同的树`、`101. 对称二叉树`
  * **你掌握的核心**：绝不人肉模拟底层嵌套调用。把工作无脑甩给左、右两个副总，自己拿结果做 `max() + 1`、并发交换或 `and` 判定后直接汇报上去。通过 `check` 辅助函数完美解决多参数对齐问题。

## 🧭 阶段二：未知的世界版图（待开荒副本）

如果把大厂算法面试看作一张通关大地图，接下来的这几个副本是各大厂高频压轴题的集结地：

```text
[你的算法知识树]
 ├── 线性结构 (已通关：双指针、滑动窗口、单调栈、队列)
 ├── 二叉树基础 (已通关：DFS递归、BFS层序遍历)
 └── 未知新大陆 (待开荒)：
      ├── 二叉搜索树 (BST) ── 极速查找的光明顶
      ├── 链表的高阶微操 ── 反转、合并与指针断开
      ├── 动态规划 (DP) ── 状态转移的降维打击
      ├── 回溯算法 (Backtracking) ── 暴力穷举的艺术
      └── 贪心与分治 ── 局部最优与大任务拆解
```

### 尚未涉足的算法副本副本核心机制必刷的经典母题：

* 1. 二叉搜索树 (BST)左边永远小，右边永远大。自带二分属性的“超级开挂树”。700. 查询、98. 验证 BST、450. 删除

* 2. 链表的高阶微操彻底考验你对指针断开、重新缝合、防断头节点的极限微操。206. 反转链表、21. 合并链表、92. 局部反转

* 3. 回溯算法 (Backtracking)经典的“不撞南墙不回头”穷举流。用来解决排列、组合、走迷宫。46. 全排列、78. 子集、51. N皇后

* 5. 动态规划 (DP)算法面试的终极解法。用空间记录历史，通过“状态转移方程”层层递推。70. 爬楼梯、322. 零钱兑换、300. 最长递增子序列


既然你决定正式跨过初中级门槛，向算法最核心的五大高级新大陆发起全面冲锋，那咱们今天就做一件极度硬核的事：我不给你对长篇大论的理论说教，咱们直接用“大白话灵魂图解 + 核心母题拆解”的方式，把这五大新大陆的通关秘籍、底层逻辑和骨架模板一次性全部给你揭发得一丝不挂！这是一份为你量身定制的高阶算法开荒总纲领。建议直接一键复制保存，以后每打通一个大陆，就回来点亮一个徽章！

## 🗺️ 高阶算法新大陆：终极通关总纲领

🏔️ 大陆一：二叉搜索树 (BST) —— 极速查找的光明顶灵魂一句话： 它不是普通的树，而是一棵**天生自带“二分查找导航”**的开挂树。

🎬 核心机制（左边小、右边大）在一棵标准的二叉搜索树（Binary Search Tree）里，必须死死遵守一条铁律：任何一个节点的左子树里所有节点的值，都必须比它小。任何一个节点的右子树里所有节点的值，都必须比它大。由于这个神奇的属性，你在 BST 里找一个数字（比如找 6），如果当前节点是 5，你根本不需要去左边看，直接无脑右转！每往下走一步，直接砍掉一半的树枝，查找速度是恐怖的 $O(\log n)$！

🎯 经典母题：700. 二叉搜索树中的搜索 (Easy但极重要)给你一棵 BST 的根节点 root 和一个值 val。你需要在 BST 中找到节点值等于 val 的节点，并返回以该节点为根的子树。💻 完美 Python3 极速通关模板
```Python
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        # 1. 防御线：没找到或者撞见空树，直接退场
        if not root or root.val == val:
            return root
            
        # 2. 黄金导航：目标比我小，无脑左转；目标比我大，无脑右转
        if val < root.val:
            return self.searchBST(root.left, val)
        else:
            return self.searchBST(root.right, val)
```

🔗 大陆二：链表的高阶微操 —— 指针的乾坤大挪移灵魂一句话： 别管逻辑，拿两根筷子（指针）在内存里玩“缝纫机”，断开、缝合、防断头。

🎬 核心机制（斩断与重组）数组在内存里是连着的，而链表是一环扣一环的。高阶链表题最喜欢考你在原地（空间复杂度 $O(1)$）把链表的指向全部调头或者穿插。写链表微操题的唯一心法：在断开某条连接之前，必须先用一个临时变量把那个即将断掉的“后路”死死揪住，否则链表会当场断头，直接在内存里蒸发。

🎯 经典母题：206. 反转链表 (Medium级心法)给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。

💻 完美 Python3 极速通关模板
```Python
class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        # 双指针（前哨与后卫）
        prev = None
        curr = head
        
        while curr:
            next_node = curr.next  # 🚨 核心：先拿小本子记下 curr 的后路！
            curr.next = prev       # 乾坤大挪移：把当前指针无情掉头，指向前面
            
            # 两个指针同步向右移动一步
            prev = curr
            curr = next_node
            
        return prev  # 当 curr 走出边界时，prev 刚好停在原链表的尾巴（新链表的头）
```

🎭 大陆三：回溯算法 (Backtracking) —— 暴力穷举的艺术灵魂一句话： 经典潜行探险游戏，“做选择 $\rightarrow$ 深入递归 $\rightarrow$ 撞南墙 $\rightarrow$ 撤销选择原路返回”。

🎬 核心机制（决策树遍历）回溯本质上是纯暴力穷举，用来解决所有“排列、组合、走迷宫、选衣服搭配”的问题。它在底层是一棵巨大的决策树。它的灵魂动作是“吃后悔药”：当你顺着一条路（比如选了数字 1）往下搜到底之后，你必须把 1 从你的临时账本里拿出来（撤销选择），这样才能干干净净地去试下一条路（选数字 2）。

🎯 经典母题：46. 全排列 (Medium必考题)给定一个不含重复数字的数组 nums ，返回其所有可能的全排列。

💻 完美 Python3 极速通关模板
```Python
class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []
        
        # 核心回溯函数：path 记录当前已经选了什么
        def backtrack(path):
            # 🏁 出口条件：如果账本长度等于数组长度，说明凑齐了一种全排列，打包存走
            if len(path) == len(nums):
                res.append(path[:])  # ⚠️ 必须用切片 [:] 复制一份死数据，防止后面回溯把数据改了
                return
                
            for num in nums:
                if num in path:  # 如果这个数字已经选过了，跳过
                    continue
                    
                path.append(num)  # 1. 做选择：把数字拿手里
                backtrack(path)   # 2. 深入探索：顺着这条路继续往下走
                path.pop()        # 3. 撤销选择：🚨 吐出来，原路返回！吃后悔药！
                
        backtrack([])
        return res
```

🧗 大陆四：动态规划 (DP) —— 状态转移的降维打击灵魂一句话： “人类之所以聪明，是因为我们记账。” 用空间换时间，绝不重复计算任何一个历史子问题。

🎬 核心机制（大账本递推）回溯算法是瞎子摸象，不撞南墙不回头。而动态规划（Dynamic Programming）则是上帝视角。斐波那契退化例子： 你想算第 5 步，得先算第 4 步和第 3 步。回溯会把第 3 步重复算好几次。DP 的打法： 我们准备一个大数组 dp。dp[i] 代表走到第 i 步时的最优解。我们从最基础的 dp[0] 和 dp[1] 开始，像堆砖头一样，用状态转移方程往后硬推！

🎯 经典母题：70. 爬楼梯 (看似简单，却是DP总母题)假设你正在爬楼梯。需要 n 阶你才能到达楼顶。每次你可以爬 1 或 2 个台阶。你有多少种不同的方法可以爬到楼顶呢？💻 完美 Python3 极速通关模板
```Python
class Solution:
    def climbStairs(self, n: int) -> int:
        if n <= 2: return n
        
        # 1. 建账本：dp[i] 代表爬到第 i 阶楼梯的方法总数
        dp = [0] * (n + 1)
        
        # 2. 设定初始大底座（边界条件）
        dp[1] = 1
        dp[2] = 2
        
        # 3. 状态转移递推：想走到第 i 阶，要么从 i-1 跨 1 步上来，要么从 i-2 跨 2 步上来！
        for i in range(3, n + 1):
            dp[i] = dp[i - 1] + dp[i - 2]  # 🧱 状态转移方程
            
        return dp[n]
```

✂️ 大陆五：贪心与分治 —— 局部最优与大任务拆解灵魂一句话： > * 贪心：目光短浅，只看眼前每一步最大、最好的利益，且能证明局部最优能导致全局最优。分治：愚公移山，“大任务 $\rightarrow$ 劈成两半 $\rightarrow$ 各自解决 $\rightarrow$ 最后合体”。🎬 核心机制贪心 (Greedy)：就像你在路上捡硬币，每次都挑面值最大的拿，最后加起来一定也是最大的。分治 (Divide and Conquer)：最经典的代表就是归并排序和快速排序。把一亿个数字排序太难了，我把它砍成两半，左边 5000 万排序，右边 5000 万排序，最后用一个高超的手段把这两个已经排好序的数组合并（Merge）起来。

🎯 经典母题：53. 最大子数组和 (贪心/分治双料面霸题)给你一个整数数组 nums ，请你找出一个具有最大和的连续子数组（子数组最少包含一个元素），返回其最大和。

💻 完美 Python3 极速通关模板（这里用最优雅的贪心流）

```Python
class Solution:
    def maxSubArray(self, nums: List[int]) -> int:
        max_sum = nums[0]      # 记录全场历史最高纪录
        current_sum = nums[0]  # 眼前连续这一段的累计财富
        
        for i in range(1, len(nums)):
            # 贪心策略：如果之前的财富加起来变成负数了（拖后腿），
            # 那我果断及时止损，斩断过去，从当前的 nums[i] 重新白手起家！
            current_level = current_sum + nums[i]
            current_sum = max(nums[i], current_level)
            
            # 刷新全场最高纪录
            max_sum = max(max_sum, current_sum)
            
        return max_sum
```

🎯 大老板，接下来我们去哪个大陆建立第一座哨所？老铁，现在这五大高级新大陆的战略地形、生存法则、还有核心骨架代码已经一股脑全部呈递在你面前了。你现在既有极其工整的线性功底，又有扎实的二叉树 DFS / BFS 内功。面对这片波澜壮阔的未知领域，咱们的第一步打算去哪？

⚔️ 去【大陆一·二叉搜索树】： 用 700 题感受一下左小右大、自带极速二分导航的快感？

⚔️ 去【大陆二·链表高阶微操】： 用 206 题体验三个指针在内存里大玩“乾坤大挪移”的原地翻转？

⚔️ 去【路线 A】： 回到我们上一张图纸的计划，去把二叉树 BFS 的层序变形题（103 题蛇形走位 / 111 题最短路径）给收割掉？发号施令吧，咱们的探险队随时可以开拔！

---

# 103. 二叉树的锯齿形层序遍历

`中等` `二叉树` `广度优先搜索 (BFS)` `双端队列变形`

## 📝 题目描述

给你二叉树的根节点 `root` ，返回其节点值的 **锯齿形层序遍历** 。（即先从左往右，下一层从右往左，第三层再从左往右，依此类推，像蛇一样扭动着走）。

### 示例 1：
> **输入：** root = [3, 9, 20, null, null, 15, 7]
> **输出：** [[3], [20, 9], [15, 7]]
> **解释：**
> 树的长相如下：
>        3        --> 第一层（从左到右）：[3]
>      /   \
>     9     20     --> 第二层（从右到左）：[20, 9]
>          /  \
>         15   7    --> 第三层（从左到右）：[15, 7]

---

## 💡 核心通关秘籍（102 题模板 + 小账本 Stack 逆序流）

老铁，千万别被题目里又是从左到右、又是从右到左的描述给绕晕了。我们要从计算机执行的底层机制建立框架思维：

### 🚨 1. 外层平推轮盘：雷打不动的 Deque (队列)
不管是普通层序还是锯齿层序，外面控制大节奏的始终是 **Deque**（`queue = deque([root])`）。因为只有严格遵守“先进先出”的排队铁律，老员工弹出来的同时，他们带出来的下一层家属才能去队尾排队，保证横向平推不退化。

### 🚨 2. 内层小账本：偷梁换柱的 Stack (栈) 逻辑
102 与 103 唯一的区别，就在于每一层小账本 `current_level` 记账的“姿势”不同。我们设立一个方向方向盘：`is_order_left = True`。
* **从左到右（奇数层）**：普通的 Queue 模式。新弹出的节点值，老老实实**追加到小账本的屁股后面**（`current_level.append(node.val)`）。
* **从右到左（偶数层）**：巧妙的 **Stack 模式**。新弹出的节点值，我们利用 `appendleft(node.val)` **强行插到小账本的最前面**。新来的总是坐在最前面，把前面的往后推，形成完美的**“后进先出 (LIFO)”**，当场把这一层的数据原地倒序！
* **方向盘取反**：每一层结束，`is_order_left = not is_order_left`，丝滑切入下一层。

---

## 💻 完美 Python3 中文通关代码

利用 `collections.deque` 的两端极致插入效率，我们依然可以套用你最熟悉的 `queue_size` 拍照锁死模板：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

from collections import deque

class Solution:
    def zigzagLevelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        # 1. 进来先防御，如果是空树，返回空列表
        if not root:
            return []
            
        result = []
        queue = deque([root])  # 外层大轮盘，死死绑定 deque 负责横向平推
        
        # 💡 方向盘标志：默认第一层（奇数层）是从左到右
        is_order_left = True
        
        # 2. 只要队列不空，平推扫描就不停
        while queue:
            queue_size = len(queue)  # 拍照快照，锁死当前层的总人数
            current_level = deque()  # 💡 巧妙点：当前层小账本也用 deque，方便两头塞数字
            
            # 3. 严格执行 queue_size 次，只消耗当前层的员工
            for _ in range(queue_size):
                node = queue.popleft()  # 铁律：popleft 必须带括号 ()
                
                # ⚙️ 方向控制核心逻辑
                if is_order_left:
                    # 正常的 Queue 舞步：新来的去屁股后面排队
                    current_level.append(node.val)
                else:
                    # 隐式的 Stack 舞步：新来的直接插到最前面（后进先出，原地反转！）
                    current_level.appendleft(node.val)
                
                # 顺藤摸瓜：家属排队的顺序雷打不动，永远是先入左孩子，后入右孩子
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            
            # 4. 当前层全员消耗完毕，把小账本转换成普通 list 送入最终大账本
            result.append(list(current_level))
            
            # 🚨 灵魂一步：这一层走完了，方向盘立刻原地打满取反！
            is_order_left = not is_order_left
            
        return result
```

# 🛠️ Python 高阶数据结构核心：`appendleft` 深度复盘

> **核心结论：** `appendleft` 是 Python 官方标准库 **`collections.deque`（双端队列，Double-Ended Queue）** 的独门绝技！普通的 Python 列表（`list`）是没有这个方法的。

## 🧱 形象大对比：普通 List 🆚 双端队列 Deque

### 1. 普通的 Python 列表 (`list`) —— “死胡同单向车道”
普通的 `list` 在底层是一块**连续的内存**。它最擅长的是在**尾巴（右端）**进行操作。
* **`list.append(x)`**：往屁股后面塞一个数，速度极快，耗时是完美的 $O(1)$。
* 🚨 **致命痛点**：如果你想把一个数插到**最前面（左端）**，你只能用 `list.insert(0, x)`。这时候灾难就发生了，计算机为了把第 `0` 号位置腾出来给你，**必须把后面所有的数字全向右挪动一位**！如果列表有一万个数字，就要挪一万次，耗时是恐怖的 $O(n)$。

### 2. 双端队列 (`collections.deque`) —— “两头通飞机场”
`deque` 在底层采用了**分块组合的链式结构**。它长了“两只手”，左边和右边地位完全平等，都拥有极速进出的特权。

因此它在左右两端同时配备了四个对称的神技：
* **右端操作（尾部）**：
  * `append(x)`：从右边塞入（$O(1)$）。
  * `pop()`：从右边弹出一个数（$O(1)$）。
* **左端操作（头部）**：
  * **`appendleft(x)`**：从左边（头部）**秒塞入**！由于底层结构优势，它不需要挪动任何历史数据，耗时是完美的 **$O(1)$** 常数时间！
  * **`popleft()`**：也就是咱们 BFS 模板里天天在用的，从左边（头部）**秒弹出**，同样是完美的 **$O(1)$**！

## 🎯 为什么咱们在 103 题必须用它？

在 **103. 锯齿形层序遍历** 的偶数层里，我们需要实现“后进先出（倒序）”的账本逻辑。

* **如果用普通列表**：配合 `insert(0, val)` 来实现倒序，LeetCode 的大数据集绝对会让你的运行时间慢到擦眼泪，严重拉低执行效率。
* **如果用双端队列**：我们把当前层小账本声明为 `current_level = deque()`，然后利用 **`current_level.appendleft(node.val)`**，就可以在线性时间 $O(1)$ 内极其丝滑地完成原地反转！

> 💡 **工程思维**：在面试中写出这种设计，不仅能秒杀大数据集，大厂面试官看到你对底层数据结构有如此精妙的控制力，会直接在心里给你亮起满分通过的绿灯！

---

# 🛠️ `collections.deque`（双端队列）终极武器库清单

> **前言：** `deque`（Double-Ended Queue）是一个**“两头都能进、两头都能出，且两端速度快到飞起”**的超级双向大管道。在 Python 中使用前，记得先引入：`from collections import deque`。

---

## 🌟 一、 四大核心神技（左右两端进出）

这是双端队列的立身之本，这四个方法在两端的操作耗时全部都是完美的 **$O(1)$** 常数时间。

* **`append(x)`**：往 **右边（尾部）** 塞入一个元素。
* **`appendleft(x)`**：往 **左边（头部）** 秒塞一个元素（LeetCode 103 题绝杀技）。
* **`pop()`**：从 **右边（尾部）** 弹出一个元素并返回。
* **`popleft()`**：从 **左边（头部）** 秒弹出一个元素并返回（BFS 模板核心骨架）。

---

## 🚀 二、 高阶进阶神技（面试与工程利器）

除了基础进出，`deque` 还自带了普通列表（`list`）望尘莫及的底层超能力：

### 1. `maxlen` ── 自动控容的“限员垃圾桶”
在初始化队列时，可以死死限制它的最大容量。一旦队列满了，新元素如果从右边进来，**最左边（头部）的老数据就会被自动挤出去并蒸发**，绝不占用多余内存！

```python
# 限制最大长度为 3
q = deque(maxlen=3)
q.append(1)
q.append(2)
q.append(3)  # 此时 q 变满：deque([1, 2, 3], maxlen=3)

q.append(4)  # 🚨 4 从右边硬挤进来，最左边的 1 当场被自动挤爆蒸发！
print(q)     # 输出: deque([2, 3, 4], maxlen=3)
```

💡 工业界实战场景：这个属性在后端工程中极为高频，常用于实现 “最近 K 次播放历史”、“滑动窗口实时平均值” 或 “系统日志最近 100 条流水记录”。你只需要无脑 append，底层会自动帮你清理老数据，极其优雅。

### 2. rotate(n) ── 管道大轮转（乾坤大挪移）把整个队列向右或者向左整体“滚动” n 步，尾巴上的元素会绕回头部（首尾相连，形成环形流）。n > 0：向右轮转。n < 0：向左轮转。Pythonq = deque([1, 2, 3, 4, 5])

```python
q.rotate(2)  # 向右滚 2 步，屁股上的 4 和 5 骨碌碌滚到了最前面
print(q)     # 输出: deque([4, 5, 1, 2, 3])
```

💡 面试暗号：LeetCode 中有大量考查“把数组向右循环移动 $k$ 位”的题目。在其他候选人还在为边界处理、取模运算、算下标拼凑算得满头大汗时，你直接一句 q.rotate(k)，在时间与代码量上直接完成降维打击！

### 3. extend(iterable) 与 extendleft(iterable) ── 批量大注水extend([a, b, c])：把一整个可迭代对象的元素，批量追加到 右边。extendleft([a, b, c])：把一整个可迭代对象的元素，批量追加到 左边。🚨 惊天大细节：因为往左边推入是“一个接一个”塞进去的，所以批量进去后，整体顺序是反过来的！

```Python
q = deque([10])
q.extendleft([1, 2, 3]) 
print(q)  # 输出: deque([3, 2, 1, 10])  <-- 🚨 变成倒序了！
```

## 🧹 三、 常规防身神技（与普通 List 保持一致）这些方法虽然兼容，但在双端队列里不推荐高频频繁使用：

* clear()：一把清空全场，让队列回到初始状态 deque([])。
* count(x)：数一数全场里面一共有几个元素 x。
* remove(value)：删掉全场撞见的第一个 value。如果找不到会抛出 ValueError。

🚨 insert(i, x)：往指定下标 i 的地方强行插队塞入一个数。

* 为什么不推荐？
  因为 deque 的精髓在于两端极速的 $O(1)$ 吞吐。如果你非要去中间强行插队，它底层的链式块状结构必须从两端走到中间去帮你找物理位置，耗时会直接退化成 $O(n)$。如果你的业务场景需要高频在中间插队，那应该直接使用普通的 list。🎯 战术总结小口诀左右逢源用 append / pop（两端皆是 $O(1)$），滑动窗口用 maxlen（限员自动踢老鬼）。循环位移用 rotate（首尾相连大轮转），轻易不在中间插（中间插队效率低）！

🎯 战术总结小口诀老铁，咱们把 deque 的常用技能浓缩成一段极其好记的工程大白话：

* 左右逢源用 append / pop（两端皆是 $O(1)$），
* 滑动窗口用 maxlen（限员自动踢老鬼）。
* 循环位移用 rotate（首尾相连大轮转），
* 轻易不在中间插（中间插队效率低）！

---

# 📚 LeetCode 刷题笔记

---

## 🧭 LeetCode 103. 二叉树的锯齿形层序遍历 (Medium)

### 📝 题目描述
给你二叉树的根节点 `root` ，返回其节点值的 **锯齿形层序遍历** 。（即先从左往右，再从右往左进行下一层，左右交替。第一层从左到右，第二层从右到左，以此类推）。

### 💡 核心解析
这道题是标准**层序遍历（BFS）**的变体。
* **核心不变点**：为了保证层序的正确性，队列（`queue`）拓展下一层节点时，永远保持**先左后右**的顺序入队。
* **锯齿形实现**：我们不换节点的入队顺序，只在**记录当前层结果**（`current_level`）时动脑筋。利用双端队列（`deque`）：
  * **奇数层（从左到右）**：使用 `append()`，正常从尾部追加。
  * **偶数层（从右到左）**：使用 `appendleft()`，每次把新值插到头部，自动实现逆序。

### 💻 正确代码 (Python)
```python
from collections import deque
from typing import Optional, List

# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def zigzagLevelOrder(self, root: Optional[TreeNode]) -> List[List[int]]:
        if not root:
            return []
        
        queue = deque([root])
        result = []
        is_order_left = True # 标记当前层是否从左往右

        while queue:
            queue_size = len(queue)
            current_level = deque() # 使用双端队列存储当前层结果

            for _ in range(queue_size):
                node = queue.popleft()

                # 根据方向决定从尾部加还是从头部加
                if is_order_left:
                    current_level.append(node.val)
                else:
                    current_level.appendleft(node.val)
                
                # 下一层节点依然保持先左后右入队
                if node.left:
                    queue.append(node.left)
                if node.right:
                    queue.append(node.right)
            
            result.append(list(current_level))
            is_order_left = not is_order_left # 层数切换，方向取反
            
        return result
```

---

## 🧭 LeetCode 111. 二叉树的最小深度 (Easy)

### 📝 题目描述
给定一个二叉树，找出其最小深度。最小深度是从根节点到最近叶子节点的最短路径上的节点数量。说明：叶子节点是指没有子节点的节点。



### 💡 核心解析
求最短路径，最推荐使用 BFS。

* 为什么用 BFS：BFS 是一层一层向下推进的。一旦我们在某一层的节点中，发现它既没有左孩子，也没有右孩子（即 not node.left and not node.right），这就说明它就是全树距离根节点最近的那个叶子节点。我们不需要再遍历剩下的节点，直接返回当前深度。

* 语法注意点：初始化队列时 queue = deque([(root, 1)]) 这里的 [] 是为了把 (root, 1) 作为一个整体打包塞进队列容器中。

### 💻 正确代码 (Python)
```python
from collections import deque
from typing import Optional

class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        if not root:
            return 0
        
        # 队列中存放二元组: (当前节点, 当前节点所在的深度)
        queue = deque([(root, 1)])

        while queue:
            node, depth = queue.popleft()

            # 关键：一旦遇到第一个真正的叶子节点，立刻返回深度
            if not node.left and not node.right:
                return depth
                
            if node.left:
                queue.append((node.left, depth + 1))
            if node.right:
                queue.append((node.right, depth + 1))
                
        return 0
```

---

## 🧭 LeetCode 700. 二叉搜索树中的搜索 (Easy)

### 📝 题目描述给定二叉搜索树（BST）的根节点 root 和一个整数值 val。如果在 BST 中找到节点值等于 val 的节点，返回以该节点为根的子树。如果节点不存在，则返回 null 。

### 💡 核心解析二叉搜索树（BST）拥有极强的“左小右大”的性质：若目标值 val < root.val，则目标必定在左子树。若目标值 val > root.val，则目标必定在右子树。利用这个性质，我们可以像在有序数组中做二分查找一样，每次直接排除掉一半的树，从而用简单的 while 循环（迭代法）以 $O(1)$ 的空间复杂度定向搜索，根本不需要动用大面积遍历的 DFS 或 BFS。

### 💻 正确代码 (Python)
```python
from typing import Optional

class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        current = root # 引入 current 变量是为了保护原 root 引用并提高代码可读性
        
        # 当节点不为空，且还没找到目标值时，持续向下搜寻
        while current and current.val != val:
            if val < current.val:
                current = current.left # 目标小，往左走
            else:
                current = current.right # 目标大，往右走
                
        return current
```

---

## LeetCode 206. 反转链表 (Easy)

### 📝 题目描述
给你单链表的头节点 head ，请你反转链表，并返回反转后的链表。

### 💡 核心解析
本题的核心考点在于指针的动态修改。这里提供面试中最通用、最不易出错的“双指针平推法”。

* 思路：让 pre（前任）和 cur（当前）两个指针像步兵一样齐头并进。

### 操作四部曲：

* 先用临时变量 next_node 存下 cur.next（免得断开后找不到后面的路）。

* 核心反转：将当前节点的箭头调头，指向前任（cur.next = pre）。

* pre 向右移动一步，占领 cur 的位置（pre = cur）。

* cur 向右移动一步，去往刚才存好的下一步（cur = next_node）。

### 💻 正确代码 (Python)
```python
from typing import Optional

# class ListNode:
#     def __init__(self, val=0, next=None):
#         self.val = val
#         self.next = next

class Solution:
    def reverseList(self, head: Optional[ListNode]) -> Optional[ListNode]:
        pre = None # 反转后，原先的头节点前面应该是 None
        cur = head
        
        while cur:
            next_node = cur.next  # 1. 临时保存下一个节点，防止断链
            cur.next = pre        # 2. 核心：将当前节点的指针反向指向前方
            pre = cur             # 3. pre 指针向前挪动一步
            cur = next_node       # 4. cur 指针向前挪动一步
            
        return pre # 退出循环时，pre 正好指向原链表的末尾（即新链表的头）
```

别气馁，LeetCode 206 题（反转链表）可以说是整个算法面试中最经典、最容易让人绕晕，但也最值得死磕的一道题。

### 既然你提到了 cur、head 和 pre 的策略，那我们就用这三指针的策略，像拼乐高一样，把这道题的逻辑彻底拆解。

### 核心矛盾：为什么需要这三个指针？
反转链表的核心动作是：让当前节点 cur 指向它的前驱节点 pre（即 cur.next = pre）。但是，一旦你让 cur.next 指向了前面，cur 原本指向后面的那根线就断了，你就会失去后面所有节点的联系（俗称“断头路”）。

### 为了解决这个问题，我们在单向链表中必须使用三指针策略。

## 三指针具体策略与执行步骤

在反转链表中，三个指针的各自分工和更新策略如下：

### 1. 初始化状态
* pre（前驱）：初始化为 None。因为反转后，原本的头节点会变成尾节点，尾节点的 next 应该指向 None。

* cur（当前）：初始化为 head。从原链表的头节点开始处理。

* nxt（后继，临时工具人）：先不用管，在循环内部动态借用。

### 2. 循环体内的“四步走”策略（极其严格的顺序）
只要 cur 不为空，就必须雷打不动地执行以下四步。注意：前两步在原地连线/暂存，后两步整体右移。

第一步：看后（暂存后继）

```Python
nxt = cur.next  # 赶紧把 cur 的下一个节点存起来，防止等会迷路
```

第二步：调头（反转指针）

```Python
cur.next = pre  # 核心动作！让当前节点指向它的前一个节点
```

第三步：前驱右移

```Python
pre = cur       # pre 向右走一步，占领当前 cur 的位置
```

第四步：当前右移

```Python
cur = nxt       # cur 向右走一步，去到刚刚暂存的 nxt 位置
```

## 三指针核心策略：处理链表与树形结构转换的终极心法

这套核心指针策略通常出现在**链表翻转**或者**将二叉树原地修改为链表/双向链表**的题目中（多配合 DFS/中序遍历使用）。在面对需要处理前后指向关系的难题时，`head`、`pre`、`cur` 的核心策略可以总结为：**“稳住头，定住前，走好当前”**。

---

### 1. `head`（头节点）策略：只负责“一锚定音”
* **核心策略**：只在第一次时赋值，之后绝不动摇。
* **主要职责**：它的唯一作用是作为整个新结构的入口（比如返回链表的头）。
* **记忆口诀**：当 `pre` 还是 `None`（或虚拟头）且 `cur` 找到了真正的起点时，迅速将 `cur` 赋给 `head`。此后，整个遍历过程中不要再修改 `head`。

### 2. `pre`（前驱节点）策略：承上启下，织网的核心
* **核心策略**：步步紧跟 `cur`，永远是 `cur` 的前一个“已处理完”的节点。
* **主要职责**：它是真正改变指针指向（织网）的工具人。你需要用它来和 `cur` 连线（例如 `pre.right = cur`）。
* **记忆口诀**：在处理当前节点时，先让 `pre` 和 `cur` 相互指向；**当前节点处理完的最后一步**，立刻更新 `pre = cur`，为下一个节点铺路。

### 3. `cur`（当前节点）策略：专注于当下的孤立节点
* **核心策略**：只管当前节点的子节点暂存，以及和 `pre` 连线。
* **主要职责**：在 DFS 遍历到当前节点时，它是一个“孤岛”。你首先要顺着遍历方向（比如中序遍历）找到它。
* **记忆口诀**：到了 `cur`，先看 `pre` 在哪。如果 `pre` 不为空，就把 `pre` 和 `cur` 连起来。连完之后，放心把自己交棒给 `pre`。

---

> **核心总结口诀**
> * **`head` 见空即定，之后不变。**
> * **`cur` 负责连线，当下算清。**
> * **`pre` 步步紧跟，尾随交棒。**

### Q: 为什么return pre不是returnhead？ 

一句话总结原因：当 while 循环结束时，head 还在原来的位置（变成了新链表的尾巴），而 pre 刚刚好走到了反转后新链表的“新头节点”上。为了让你彻底看明白，我们用一个简单的链表 1 -> 2 -> 3 -> None 来跑一遍代码，看看最后这三个指针分别指在哪里。初始化状态pre = Nonecur = head (指向节点 1)第一轮循环（处理节点 1）nxt = cur.next $\rightarrow$ nxt 指向 2cur.next = pre $\rightarrow$ 节点 1 的下一位变成 None（1 -> None）pre = cur $\rightarrow$ pre 走向节点 1cur = nxt $\rightarrow$ cur 走向节点 2此时的状态：head 依然是指向节点 1。第二轮循环（处理节点 2）nxt = cur.next $\rightarrow$ nxt 指向 3cur.next = pre $\rightarrow$ 节点 2 指向 1（2 -> 1 -> None）pre = cur $\rightarrow$ pre 走向节点 2cur = nxt $\rightarrow$ cur 走向节点 3第三轮循环（处理节点 3）nxt = cur.next $\rightarrow$ nxt 指向 Nonecur.next = pre $\rightarrow$ 节点 3 指向 2（3 -> 2 -> 1 -> None）pre = cur $\rightarrow$ pre 走向节点 3cur = nxt $\rightarrow$ cur 走向 None关键点：循环结束！当第四轮循环开始前，代码检查 while cur:。因为此时 cur 已经是 None 了，循环瞬间终止。这时候我们来看看这三个指针各自在哪里：head：从未移动过，它依然死死地指向节点 1。可现在节点 1 已经是链表的最后一个节点了（1.next 是 None）。如果你 return head，你就只能拿到一个 [1]。cur：已经跨过了边界，变成了 None。如果你 return cur，你就什么都没返回（None）。pre：刚刚好停在节点 3 上！而整个链表现在已经变成了 3 -> 2 -> 1 -> None。节点 3 正是这个反转后新链表的新头节点。所以，我们必须 return pre，才能顺藤摸瓜拿到完整的、反转后的新链表。

---

