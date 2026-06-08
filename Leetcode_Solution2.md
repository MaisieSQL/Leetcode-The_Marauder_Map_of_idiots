05/29/2026 

这套“温故（巩固平推）+ 知新（树型精进）+ 降维破局（开启回溯）”的组合拳，不仅能通过堆量瞬间拉高你的总题数，还能用最省脑细胞的方式，把你的 DFS 递归内功完美嫁接到全新的回溯大陆上。今天咱们的特种兵提速日，总共安排 6 道题，咱们二话不说，直接上阵：

* 阶段一：老战场大平推（2 道高频经典变形题）142. 环形链表 II：高强度检验你的快慢指针。这次兔子和乌龟不仅要抓到有没有环，还要精准揪出环的入口（起点）在哪里，极度考验数学大局观。21. 合并两个有序链表：检验高阶链表穿针引线。用一个虚拟头 dummy 配合一个步进指针，像拉拉链一样把两条链表完美咬合。
* 阶段二：树型内功再度精进（2 道经典 DFS 母题）98. 验证二叉搜索树：深度检验你的“左小右大”黄金导航。怎么用递归把“全局边界上限和下限”像紧箍咒一样层层套给子节点？236. 二叉树的最近公共祖先：大厂面试的“面霸级”高频题。再次把“甩手掌柜递归流”发挥到极致——问左边、问右边，谁抓到人谁就往上汇报。
* 阶段三：跨越海峡，开启回溯新大陆（2 道回溯必修课）46. 全排列：回溯算法的开天辟地第一题！让你第一次在代码里写出“做选择 $\rightarrow$ 深入递归 $\rightarrow$ 撤销选择（吃后悔药）”的终极闭环。78. 子集：回溯决策树的完美变形。全排列是挑满所有数字，而子集是每个数字面临“要还是不要”的单选题，彻底吃透回溯的大局观。

---

# 🧭 LeetCode 142. 环形链表 II (Linked List Cycle II)

## 📌 题目描述
给定一个链表的头节点 `head`，返回链表开始进入环的第一个节点。如果链表无环，则返回 `null`。
* **不能修改链表。**
* **进阶要求**：是否可以使用 $O(1)$ 的空间复杂度解决？

---

## 🛑 直觉陷阱：为什么相遇点不是环的入口？
在 141 题中，快慢指针（兔子和乌龟）相遇只能证明**有环**。但由于兔子的速度是乌龟的两倍，当乌龟刚慢吞吞地挪进环里时，兔子可能已经在环里转了好几圈，然后从后面狠狠地撞上了乌龟。
141 题（环形链表 I） 和 142 题（环形链表 II） 名字长得几乎一模一样，都是拿快慢指针（乌龟与兔子赛跑）来搞事情，但它们的目的和难度级别完全不同！

简单粗暴地用一句话总结它们的区别：

141 题（Easy）：只当裁判。你只需要回答我：“这个操场到底有没有环？” （True 或 False）

142 题（Medium）：不仅要当裁判，还要当侦探。你不仅要告诉我有没有环，如果发现有环，你还要精准地把“环的入口节点（也就是乌龟和兔子第一次进入操场的那个连接点）”给我揪出来！

因此，**它们第一次相遇的点，绝对不等于环的入口！**

---

## 🪓 核心绝杀：数学定理证明 ($X = Z$)

为了在不使用额外空间（哈希表）的情况下找到入口，我们需要利用快慢指针相遇时的路程关系进行推导。

我们将链表拆解为以下三段距离：
* `X`：从 **链表起点** 到 **环入口** 的距离。
* `Y`：从 **环入口** 到 **快慢指针相遇点** 的距离。
* `Z`：从 **相遇点** 继续往前走，**回到环入口** 的距离。



### 📐 数学推导过程：
1. **乌龟（slow）走过的路程**：
   $$\text{Dist}_{\text{slow}} = X + Y$$
2. **兔子（fast）走过的路程**（假设兔子在环里已经转了 $n$ 圈）：
   $$\text{Dist}_{\text{fast}} = X + Y + n \times (Y + Z)$$
3. **根据速度关系**（兔子的速度是乌龟的 2 倍）：
   $$\text{Dist}_{\text{fast}} = 2 \times \text{Dist}_{\text{slow}}$$
   $$X + Y + n(Y + Z) = 2(X + Y)$$
4. **化简方程**：
   $$n(Y + Z) = X + Y$$
   $$X = n(Y + Z) - Y$$
   $$X = (n - 1)(Y + Z) + Z$$

当 $n = 1$（即兔子转了一圈就相遇）时，公式简化为：
$$X = Z$$

> 💡 **惊天铁律**：**从链表起点走到环入口的距离（X），刚好等于从相遇点继续往前走回到环入口的距离（Z）！**（即使 $n > 1$，兔子也只是在环里多转了几圈，最终相撞的位置依然不变）。

---

## 🏃‍♂️ 特种兵通关三步走

1. **上半场（寻找相遇点）**：兔子（步长 2）和乌龟（步长 1）同时从起点出发，直到它们在环里**第一次相遇**（`fast == slow`）。
2. **下半场变阵（打回原形）**：把兔子（`fast`）**当场打回原形，放回链表的起点 `head`**。乌龟（`slow`）保持原地不动（停在相遇点）。
3. **同速决战（见证奇迹）**：从这一刻起，让兔子和乌龟**改用相同的速度（每次只走 1 步）**向前推进。由于 $X = Z$，它们俩再次向前走的时候，**必定会在环的入口处应面相撞**！

---

## 💻 Python3 完美代码实现

```python
# Definition for singly-linked list.
# class ListNode:
#     def __init__(self, x):
#         self.val = x
#         self.next = None

class Solution:
    def detectCycle(self, head: Optional[ListNode]) -> Optional[ListNode]:
        fast = head
        slow = head

        # ─── 上半场：快慢指针寻找相遇点 ───
        while fast and fast.next:
            fast = fast.next.next  # 每次走 2 步
            slow = slow.next       # 每次走 1 步

            if fast == slow:
                # 💥 抓到相遇点！触发下半场剧情
                
                # 1. 把兔子无情打回起点
                fast = head
                
                # 2. 两人改用同等速度（每次只走 1 步）向前逼近
                while fast != slow:
                    fast = fast.next
                    slow = slow.next
                    
                # 3. 再次相遇的地方，就是数学定理证明的环入口！
                return slow
                
        # 如果跳出循环，说明指针指向了 None（无环），直接返回 None
        return None
```

---

# 什么是回溯算法

## 🎭 回溯算法的真谛：有记忆、能自动倒退的树形 DFS

用最接地气、最符合你现有的“树形递归”内功的方式来理解：**回溯算法，其实就是“有记忆的、能自动倒退的深度优先搜索（DFS）”。**

### 💡 别慌，你其实早就天天在用“回溯”了！

想象一个最生活化的场景：你走进了一座大迷宫（或者玩老式单机角色扮演游戏分叉路口选错剧情）。

1. 你来到第一个分叉路口，有两扇门：**左边门** 和 **右边门**。
2. 你决定先去**左边门**。顺着路一直走，走啊走，结果前面是一堵死墙（撞南墙了）。
3. 这时候你会怎么办？你绝对不会原地躺下，也不会直接退出迷宫。
4. 你会**往回退**，退回到刚才的那个分叉路口。
5. 然后，你假装自己**从来没走过左边门**一样，拍拍屁股，转身走向**右边门**。

> 🪓 **这个“往回退、抹掉刚才的选择、尝试另一种可能”的完整动作，在计算机里就叫做“回溯”。**

### 🌲 为什么说它本质就是“树”？

你擅长做二叉树，对二叉树的这种深度优先搜索（DFS）遍历代码肯定不陌生：

```python
def dfs(root):
    if not root: 
        return
    dfs(root.left)   # 往左子树深入
    dfs(root.right)  # 往右子树深入
```

当你在写 dfs(root.left) 的时候，计算机的程序运行栈（Activation Record）会自动帮你记住当前这个节点。当左边全部遍历完了（撞南墙返回了），程序会自动弹栈，回到当前节点，接着执行下一行的 dfs(root.right)。

你看，计算机在遍历树的时候，本身就是自带“回溯（自动倒退）”功能的！

⚔️ 那 LeetCode 里的“回溯题”到底多出了什么？
既然计算机的递归本来就会自动倒退，为什么全排列（46题）这种题目会成为卡人的高频副本？区别只在于：

普通二叉树题目：地图是现成的（树的节点和指针已经由题目建好了，你顺着指针爬就行）。

回溯算法题目：地图不存在，需要你自己在脑海里凭空用“选择”画出一棵决策树！ 并且，你手里多了一个背包（状态记录器）。

我们拿 [1, 2, 3] 的全排列来做例子。我们要拼出一个三位数的排列：

第一层决策（确定第一位数）：你有 3 个选择（1 或 2 或 3）。

你把 1 放进你的背包 path 里。此时 path = [1]。

第二层决策（确定第二位数）：因为 1 已经在背包里了，你只能在剩下的里面选。你有 2 个选择（2 或 3）。

你把 2 放进背包。此时 path = [1, 2]。

第三层决策（确定第三位数）：你只剩 1 个选择（3）。

你把 3 放进背包。此时 path = [1, 2, 3]。

满足终点条件：三位数凑齐了！把 [1, 2, 3] 记录到最终结果里。

关键一步（吃后悔药）：

计算机要顺着递归往回退了。但是注意！计算机只负责把控制权退回到第二层，它可不会帮你清理背包！

如果你不主动把 3 从背包里拿出来，背包里依然是 [1, 2, 3]，你就没法去尝试别的组合。

所以你必须在代码里手动写一句：path.pop()（把最后放进去的 3 吐出来）。

接着再退一层，把 2 也吐出来。此时背包回到 path = [1]。

这样你才能在第二位数的位置，高高兴兴地去选择 3！

🎯 核心心法总结
💡 回溯的真谛 = 树形 DFS 递归 + 手动清理背包（状态重置）。

你之所以觉得陌生，是因为之前做二叉树时，你只需要往下爬（读数据）；而回溯题需要你一边往下爬，一边往背包里塞东西（做选择），上去的时候还要把东西拿出来（撤销选择）。

它的万能核心骨架长这样：

```Python
path.append(选择)  # 把东西放进背包（做选择）
backtrack(...)    # 递归（去看下一层能选啥）
path.pop()        # 【回溯的核心】把东西拿出来（撤销选择/吃后悔药），假装没选过
```
--- 

# 🗺️ 回溯算法（Backtracking）三剑客终极通关指南

回溯算法的真谛 = **树形 DFS 递归 + 手动清理背包（状态重置）**。
以下三道题代表了回溯算法最核心的三大流派，其核心区别在于：**如何控制下一层递归的选择范围，以及如何防止答案重复。**

---

## ⚔️ 第一战：46. 全排列 (Permutations)

### 📌 题目描述
给定一个**不含重复数字**的数组 `nums` ，返回其 **所有可能的全排列** 。你可以按 **任意顺序** 返回答案。

* **示例**: `nums = [1,2,3]`
* **输出**: `[[1,2,3],[1,3,2],[2,1,3],[2,3,1],[3,1,2],[3,2,1]]`

### 💡 核心心法：字典查重，$O(1)$ 纵向狂飙
* **排列的特点**：`[1, 2]` 和 `[2, 1]` 算两种不同的方案。因此，每一层横向遍历时，我们都必须从头 `for num in nums` 扫一遍。
* **状态管理**：为了防止同一个坑位重复选择同一个数字，我们引入一个字典 `mydict`。利用其 $O(1)$ 的查找速度，一旦发现 `num` 已经在字典里，直接 `continue` 跳过。

### 🛑 避坑细节：深拷贝（切片 `[:]`）的威力
如果直接写 `res.append(sublist)`，由于 Python 的列表是引用传递，`res` 存的只是快捷方式。随着后面代码不断 `pop()`，之前攒好的结果全会被洗成空列表。必须写成 `res.append(sublist[:])` 复制一份快照。

### 💻 Python3 完美代码

```python
from typing import List

class Solution:
    def permute(self, nums: List[int]) -> List[List[int]]:
        res = []        # 大账本
        sublist = []    # 当前背包
        mydict = {}     # O(1) 状态跟踪字典
        
        def backtracking():
            # 1. 递归出口：凑齐了 N 个数字，触发深拷贝收网
            if len(sublist) == len(nums):
                res.append(sublist[:])  # 🌟 必须使用切片 [:] 防止被后续 pop 清空
                return
            
            # 2. 横向遍历：每次都从头扫描
            for num in nums:
                # 如果当前数字被用过了，直接跳过
                if num in mydict and mydict[num] == True: 
                    continue 
                
                # 3. 核心微操三部曲
                sublist.append(num)
                mydict[num] = True      # 标记占领
                
                backtracking()          # 纵向深入递归
                
                sublist.pop()           # 撤销选择（吃后悔药），列表弹尾部不需要传参数
                mydict[num] = False     # 释放状态
                
        backtracking()
        return res
```
* 时间复杂度：$O(N \times N!)$。总共有 $N!$ 种排列，每次触底收网执行 sublist[:] 的浅拷贝成本为 $O(N)$。
* 空间复杂度：$O(N)$。递归树的最大深度为 $N$，字典与背包的消耗均为线性级别。

---

# 🌲 LeetCode 78. 子集 (Subsets)

## 📌 题目描述
给你一个整数数组 `nums` ，数组中的元素 **互不相同** 。返回该数组所有可能的子集（幂集）。解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回答案。

* **示例**:
  * 输入：`nums = [1,2,3]`
  * 输出：`[[],[1],[2],[1,2],[3],[1,3],[2,3],[1,2,3]]`

---

## 🗺️ 核心心法：`start_index` 筑起防回头的高墙

子集题（以及组合题）和全排列题有着完全不同的游戏规则：
1. **顺序不敏感**：在子集里，`[1, 2]` 和 `[2, 1]` 被视为完全相同的方案。如果像排列题那样每次从头扫描，就会搜出大量的重复子集。
2. **战术武器（`start_index`）**：我们不使用字典查重，而是引入一个数字指针 `start_index`，用来**强行控制当前这一层 `for` 循环的起点**。
3. **绝不回头**：当我们在当前层挑了索引为 `i` 的数字后，向下递归进入下一层时，传入的起始位置是 **`i + 1`**。这就强制计算机只能去看当前数字**后面**的数，彻底切断了走回头路的可能性。



---

## 🛑 两个颠覆认知的核心细节

### 1. 为什么没有写 `if` 终止条件？
在 46 题全排列中，我们需要凑齐 $N$ 个数（`len(sublist) == len(nums)`）才触发 `return` 收网。
但子集题的特点是：**决策树上的每一个节点都是一个合法的子集**（包括刚开始的空集 `[]`、单元素集 `[1]`、双元素集 `[1,2]` 等）。
所以，我们**不需要设置任何显式的终止条件**，一进入递归函数，二话不说先交作业（将当前路径快照存入大账本）。当 `start_index` 超过数组长度时，`for` 循环自然不会执行，函数会自动安全返回。

### 2. 依然雷打不动的深拷贝 `sublist[:]`
由于 Python 列表的引用传递特性，一进入函数就要用 `res.append(sublist[:])` 抓取当前背包的快照，否则后续的 `pop()` 弹栈会把大账本里的内容全部洗成空列表。

---

## 💻 Python3 完美代码实现

```python
from typing import List

class Solution:
    def subsets(self, nums: List[int]) -> List[List[int]]:
        res = []        # 大账本：存放所有合法的子集
        sublist = []    # 小背包：沿途收集数字的路径
        
        def backtracking(start_index):
            # ─── 1. 一进来先交作业 ───
            # 树上的每一个节点都是一个合法子集，必须立刻用切片 [:] 复制一份存起来
            res.append(sublist[:])
            
            # ─── 2. 横向遍历（从 start_index 开始往后扫） ───
            for i in range(start_index, len(nums)):
                
                # ─── 3. 核心微操三部曲 ───
                sublist.append(nums[i])      # Step 1: 做选择（把数装进背包）
                
                # 🌟 Step 2: 纵向递归。传入 i + 1，强迫下一层只能从当前数字的后面开始挑
                backtracking(i + 1)          
                
                sublist.pop()                # Step 3: 撤销选择（吃后悔药，把数吐出来）
                
        # 从下标 0 开始拓荒
        backtracking(0)
        return res
```

* 时间复杂度：O(N×2^N)。长度为 N 的集合，其子集（幂集）的总个数雷打不动是2^N个（每个数字都有“选”或“不选”两种命运）。每次触底收网执行 sublist[:] 拷贝背包需要 O(N) 的时间。

* 空间复杂度：O(N)。系统递归调用栈的最大深度为 N，小背包 sublist 最多装 N 个元素，空间占用均为线性级别。

---

# 👑 LeetCode 90. 子集 II (Subsets II)

## 📌 题目描述
给你一个整数数组 `nums` ，其中**可能包含重复元素**。请你返回该数组所有可能的子集（幂集）。解集 **不能** 包含重复的子集。你可以按 **任意顺序** 返回答案。

* **示例**:
  * 输入：`nums = [1,2,2]`
  * 输出：`[[],[1],[1,2],[1,2,2],[2],[2,2]]`

---

## 🗺️ 核心心法：树层去重（先排序，后斩首）

这道题是整个回溯大区里最经典、面试官最爱考的**去重副本**。

### 🛑 致命痛点：原料重复导致答案重复
因为给定的数组里自带重复数字（比如有两个 `2`），如果直接套用 78 题的无脑指针模板，当我们在同一个路口横向遍历时：
1. 挑第一个 `2` 往下深入，会搜出子集 `[2]`。
2. 循环走到下一轮，挑第二个 `2` 往下深入，又会搜出子集 `[2]`。
这就导致了最终解集的重复。

### 🌟 绝杀微操：树枝可重，树层不可重
回溯的决策树中有两个截然不同的去重维度：
* **树枝重复（纵向）**：同一个组合一路向下深入时，能选重复的数。例如先选第一个 `2`，下一层再选第二个 `2`，凑成 `[1, 2, 2]`，这是完全**合法**的。
* **树层重复（横向）**：在同一个分叉路口，**不能挑两个长得一模一样的原料去开辟新分支**。因为前一个数已经把后面所有的可能性都穷举过了，现在的你和它一样，再进去就是画蛇添足。



为了实现树层去重，我们必须执行以下黄金两步：
1. **原地排序（`nums.sort()`）**：强迫一模一样的数字紧紧挨在一起（变成 `[1, 2, 2]`）。
2. **黄金剪枝线**：
   ```python
   if i > start_index and nums[i] == nums[i-1]:
       continue
   ```


## 💻 Python3 完美代码实现

```python
from typing import List

class Solution:
    def subsetsWithDup(self, nums: List[int]) -> List[List[int]]:
        res = []        # 大账本
        sublist = []    # 小背包
        
        # 🌟 树层去重至关重要的前提：必须先原地排序！
        # ⚠️ 注意：不能写成 nums = nums.sort()，否则变量会被洗成 None 导致报错
        nums.sort()  

        def backtracking(start_index):
            # 每一个节点都是合法子集，一进来先交作业
            res.append(sublist[:])
            
            # 从 start_index 开始横向遍历
            for i in range(start_index, len(nums)):
                # 🌟 你的绝杀树层去重线，极其漂亮！
                # 只有 i > start_index，才说明 nums[i-1] 是在【当前这一层】里上一次被选过的数
                if i > start_index and nums[i] == nums[i-1]:
                    continue
                
                # 核心微操三部曲
                sublist.append(nums[i])      # 做选择
                backtracking(i + 1)          # 纵向递归
                sublist.pop()                # 撤销选择（吃后悔药）

        backtracking(0)
        return res
```
* 时间复杂度：$O(N \times 2^N)$。最坏情况下（无重复元素）总共有 $2^N$ 个子集，每次拷贝需要 $O(N)$ 的时间。原地排序的时间复杂度为 $O(N \log N)$，在整体复杂度中被忽略不计。
* 空间复杂度：$O(N)$。递归栈的最大深度为 $N$。
--------------------------------------------------------------------------------------------------------------------

# 2196. 根据描述创建二叉树

`中等` `二叉树` `哈希表` `逆向构造树` `高频工程实战题`

## 📝 题目描述

给你一个二维整数数组 `descriptions` ，其中 `descriptions[i] = [parent_i, child_i, isLeft_i]` 表示还有一棵二叉树中 `parent_i` 是 `child_i` 的 **父节点**。
* `isLeft_i == 1` 表示 `child_i` 是 `parent_i` 的 **左孩子**。
* `isLeft_i == 0` 表示 `child_i` 是 `parent_i` 的 **右孩子**。

请你根据这些描述**构造出这棵二叉树**，并返回它的 **根节点 (Root)** 。

### 示例 1：
> **输入：** descriptions = [[20,15,1],[20,17,0],[50,20,1],[50,80,0],[80,19,1]]
> **输出：** [50,20,80,15,17,19]
> **解释：**
> 零散的关系拼起来后，整棵树长这样：
>         50
>       /    \
>     20      80
>    /  \    /
>  15   17  19
> 根节点是 50。


## 💡 核心通关秘籍（工厂流水线制造法 + 孤儿院找爸爸）

这道题如果直接用传统的递归或者人肉去树里找节点，你会写得痛苦不堪，而且时间复杂度会直接爆炸。作为一名优秀的工程师，我们用**“解耦思维”**把这个问题拆成两步：

### 🎬 第一步：零件工厂流水线（利用哈希表 `dict` 疯狂造人）
题目给我们的都是数字（比如 `50`，`20`）。在内存里，它们还不是正式的 `TreeNode` 对象。
1. 我们准备一个“花名册哈希表”：`nodes = {}`。
2. 只要遇到一个数字，先去花名册里查，如果没来过，就地现造一个：`nodes[val] = TreeNode(val)`。
3. 这样我们就能在 $O(1)$ 时间内，随时揪出任何一个父节点和子节点的对象，然后根据 `isLeft` 是 `1` 还是 `0`，像拼乐高一样把它们无脑连起来：`parent_node.left = child_node`。

### 🎬 第二步：全场寻找大老板（利用集合 `set` 揪出根节点）
树的关系全部连好了，但我们怎么知道谁是整棵树的**大老板（Root，根节点）**呢？
* **黄金逻辑：** 在一棵合法的二叉树里，**除了根节点之外，全场所有的节点都一定有且仅有一个爸爸！只有根节点是“无父无母”的。**
* **破局战术：** 我们准备一个“孩子集合”：`children = set()`。在连线的时候，只要谁当了别人的儿子，就一把扔进这个集合里。最后，我们遍历所有的父节点，**谁不在这个孩子集合里，谁就是那个高高在上的大老板！**


## 💻 完美 Python3 中文通关代码

带上这套流水线大局观，看代码写起来有多顺理成章：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

class Solution:
    def createBinaryTree(self, descriptions: List[List[int]]) -> Optional[TreeNode]:
        # 1. 初始化我们的两大核心武器
        nodes = {}         # 花名册字典：[数字] -> [实际的TreeNode对象]
        children = set()   # 孩子集合：用来登记全场所有当了别人孩子的数字
        
        # 2. 第一步：流水线拼装二叉树
        for parent_val, child_val, is_left in descriptions:
            # 💡 零件工厂：如果父节点/子节点在花名册里没有，就当场现造一个！
            if parent_val not in nodes:
                nodes[parent_val] = TreeNode(parent_val)
            if child_val not in nodes:
                nodes[child_val] = TreeNode(child_val)
                
            # 抓出它们的真实对象指针
            parent_node = nodes[parent_val]
            child_node = nodes[child_val]
            
            # ⚙️ 像拼乐高一样连线
            if is_left == 1:
                parent_node.left = child_node
            else:
                parent_node.right = child_node
                
            # 🚨 核心标记：把当了孩子的数字送入 children 集合登记
            children.add(child_val)
            
        # 3. 第二步：全场寻找大老板（谁没当过别人的孩子，谁就是 Root）
        for parent_val, _, _ in descriptions:
            if parent_val not in children:
                # 揪出来了！直接去花名册里拿到它的TreeNode对象返回
                return nodes[parent_val]
                
        return None
```
----------------------------------------------------------------------------------------------------------------------


战术看板：今日清单实时战况
~~LeetCode 103. 锯齿形层序遍历~~（已无伤斩杀 🏆）
~~LeetCode 2196. 根据描述创建二叉树~~（已硬核填坑 🏆）

LeetCode 111. 二叉树的最小深度 👈 【当前合围目标 A】

LeetCode 700. 二叉搜索树中的搜索 👈 【当前合围目标 B】

LeetCode 206. 反转链表（指针乾坤大挪移，待命）

四大高级新大陆新手村（回溯、动规、贪心、分治各 3 题，待命）

---

# 111. Minimum Depth of Binary Tree

`Easy` `二叉树` `广度优先搜索 (BFS)` `元组带权入队流`

## 📝 Description

Given a binary tree, find its minimum depth.

The **minimum depth** is the number of nodes along the shortest path from the root node down to the nearest leaf node.

**Note:** A leaf is a node with no children.

### Example 1:
> **Input:** root = [3,9,20,null,null,15,7]
> **Output:** 2
> **Explanation:** The shortest path is from root `3` to leaf `9`, which has a depth of 2.

### Example 2:
> **Input:** root = [2,null,3,null,4,null,5,null,6]
> **Output:** 5

### Constraints:
* The number of nodes in the tree is in the range $[0, 10^5]$.
* $-1000 \le \text{Node.val} \le 1000$

---

## 💡 核心漏洞深度解剖（为什么原代码会卡死报错？）

[cite_start]你的解法采用“元组自带 GPS 导航”的思路，让每个节点入队时自带深度属性，非常具有工业级工程思维 [cite: 133][cite_start]。但原版代码中藏着 3 个语法硬伤和 1 个逻辑盲点 [cite: 134]：

1. [cite_start]**双端队列初始化参数错误 (`TypeError`)**：`deque()` 的内部必须接收一个**可迭代对象**（如列表 `[]`）。直接传入元组 `deque((root, 1))` 会引发解包崩溃 [cite: 134][cite_start]。且末尾的右括号 `）` 误敲成了中文全角括号 [cite: 134]。
2. [cite_start]**`append()` 函数参数超载 (`TypeError`)**：在 Python 中，`append()` 只能接收**一个**参数。写成 `myqueue.append(node, depth)` 会报错 [cite: 134][cite_start]。必须用小括号将其重新打包成单一元组：`myqueue.append((node, depth))` [cite: 134]。
3. [cite_start]**三元表达式语法错误 (`SyntaxError`)**：Python 中没有其他语言的 `if-then-else` 关键字 [cite: 135]。
4. [cite_start]**🚨 致命逻辑盲点**：原代码没有在循环内部设置 `return` 语句，导致程序傻傻地遍历了全场所有节点，直接废掉了 BFS 的**最短路径光环** [cite: 135][cite_start]！BFS 的灵魂就在于：**一旦在横向平推中撞见第一个既没有左孩子、又没有右孩子的纯叶子节点，它绝对就是最近的，必须当场直接返回深度提款跑路** [cite: 135, 136]！

---

## 💻 完美 Python3 通关代码（元组带权 GPS 导航流）

[cite_start]顺着你的最初心路历程，我们将语法细节全部修复，并装配上“撞见叶子立即提款”的雷达检测器 ：

```python
# Definition for a binary tree node.
# class TreeNode:
#     def __init__(self, val=0, left=None, right=None):
#         self.val = val
#         self.left = left
#         self.right = right

from collections import deque

class Solution:
    def minDepth(self, root: Optional[TreeNode]) -> int:
        # Case 1: Corner case 进来先防御，空树深度直接为 0
        if not root:
            return 0
            
        # Case 2: ✅ 修复：用英文规范括号，且必须用列表 [] 包裹初始元组
        myqueue = deque([(root, 1)])

        while myqueue:
            # 弹出当前节点，并解包出它自带的“深度 GPS 导航”
            mynode, mydepth = myqueue.popleft()
            
            # 🚨 ✅ 灵魂修正：装配核心雷达！一旦撞见全场第一个“无家属”的纯叶子节点
            if not mynode.left and not mynode.right:
                # 广度优先自带的最短路径光环发挥威力，直接当场带着深度提款跑路！
                return mydepth
            
            # ✅ 修复：将节点和更新后的深度重新打包成【单一元组】传入 append
            if mynode.left:
                myqueue.append((mynode.left, mydepth + 1))
            if mynode.right:
                myqueue.append((mynode.right, mydepth + 1))
        
        return 0
```

---

# 700. Search in a Binary Search Tree (三轨流终极对比)

> [cite_start]**前言：** 二叉搜索树（BST）的黄金铁律——**左子树全小，右子树全大** [cite: 130][cite_start]。无论用哪种形态的代码，核心战术都是：**目标值小了就无脑左转，大了就无脑右转，每一步直接干掉半棵树！** [cite: 130, 142]

---

## 🛠️ 方法一：直接迭代流（While 循环 · 空间之王）

> [cite_start]**🚀 战术精髓：** 纯度 100% 的迭代。不借助系统递归栈，也不开辟任何队列，单枪匹马用一个 `while` 指针顺着楼梯一步一步往下踩 。这是工业界最推崇的写法，因为空间开销是绝对完美的 $O(1)$！


```python
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        # 只要指针没走到尽头（None），且还没撞见我们要找的 val，就顺着导航死磕往下走
        while root and root.val != val:
            # 🧭 黄金导航：小了往左打方向盘，大了往右打方向盘
            if val < root.val:
                root = root.left
            else:
                root = root.right
                
        # [cite_start]退出循环时，要么是 root 撞上目标节点了，要么是走到尽头变成 None 了 [cite: 144]
        return root
```

## 🎭 方法二：DFS 递归流（深度优先形式 · 极其优雅）

> [cite_start]** 🚀 战术精髓： 形式上是 DFS 递归（顺着树枝一条路走到黑），但骨子里由于“单线深入”，它绝对不会发生常规 DFS 那种“左右两边都去试”的分叉穷举 ！写它纯粹是因为代码少，写起来像写诗 。

```python
class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        # 🧱 终极防线：碰空了（没找到）或者当场抓获值相等了，直接原样交差返回 [cite: 144]
        if not root or root.val == val:
            return root
            
        # 🧭 极速导航：目标值小，右边的一大半树枝直接不要，打包 luggage 去左子树顶层 [cite: 142, 147]
        if val < root.val:
            return self.searchBST(root.left, val)
        # 目标值大，左边全部原地开除，打包无脑右转 [cite: 142, 147]
        else:
            return self.searchBST(root.right, val)
```

## 🌊 方法三：BFS 队列流（广度优先形式 · 降维微操）

> [cite_start]** 🚀 战术精髓： 很多老铁以为 BFS 必须“一层一层铺满了扫描”。错！那是普通树！在 BST 里，既然我们有 GPS 导航，即便是用 queue 队列平推，我们在任何时候也只让满足大小方向的那“一个”孩子入队！队列里永远最多只有一个幸存者，堪称 BFS 里的刺客流。

```python
from collections import deque

class Solution:
    def searchBST(self, root: Optional[TreeNode], val: int) -> Optional[TreeNode]:
        if not root:
            return None
            
        # 初始化双端队列
        queue = deque([root])
        
        while queue:
            # 弹出当前层的唯一指挥官
            node = queue.popleft()
            
            # 锁定目标，当场直接提款跑路
            if node.val == val:
                return node
                
            # 🧭 刺客流 BFS 导航：绝不贪多！符合条件的独生子才允许入队
            if val < node.val and node.left:
                queue.append(node.left)
            elif val > node.val and node.right:
                queue.append(node.right)
                
        return None
```

---

