# 复盘

---

# 🗺️ LeetCode 远征记：王国之泪全景游戏地图大审计

> **审计总评：** 截至 2026 年 6 月，你已经在 LeetCode 大陆上斩获了 **20 道核心王牌母题**。你在“第一主大陆（线性战区）”展现出了恐怖的统治力，鸟望台几乎全满；“第二主大陆（树与图）”纵深推进顺利；而“第三主大陆（高级策略）”仍处于战争迷雾中，等待全面点火开荒。

---

## 🛡️ 【第一主大陆：海拉鲁平原 ── 线性结构战区】
### 📊 整体开塔进度：▓▓▓▓▓▓▓▓▓▓ 100% (满级神装 🏆)

这个大区是你的王牌大本营。你对指针的挪移、哈希的动态记账、栈的后进先出（LIFO）玩得极溜，基础地基稳如磐石。

### 🗼 1. 数组、链表与双指针鸟望台 (开荒率：90%)
- **1. Two Sum** `(Easy)` ── **核心分类**：哈希表 / 空间换时间
  - *开发度*：精通。乱序数组“四处打听”流，用字典当备忘录实现 $O(1)$ 查找。
- **167. Two Sum II - Input Array Is Sorted** `(Medium)` ── **核心分类**：相向双指针 (对撞)
  - *开发度*：精通。升序数组自带黄金导航，最矮最高两头夹击，注意 1-based 下标。
- **283. Move Zeroes** `(Easy)` ── **核心分类**：同向双指针 (快慢)
  - *开发度*：精通。质检员 `fast` 抓非 0 元素，搬运工 `slow` 原地控坑交换。
- **206. Reverse Linked List** `(Easy)` ── **核心分类**：高阶链表微操
  - *开发度*：精通。三指针原地乾坤大挪移。死死记住因为循环结束时 `cur` 踩空为 `None`，所以必须 `return pre` 才能拿到新车头。
- **92. Reverse Linked List II** `(Medium)` ── **核心分类**：高阶链表微操
  - *开发度*：精通。引入虚拟头 `dummy` 稳住大局，采用“头插法（火车车厢局部调头）”穿针引线。
- **21. Merge Two Sorted Lists** `(Easy)` ── **核心分类**：高阶链表微操
  - *开发度*：精通。`dummy` 头配合单步指针，像拉拉链一样让两条链表完美咬合，尾巴一刀缝合。

### 🗼 2. 滑动窗口与动态账本鸟望台 (开荒率：100% 满级)
- **3. Longest Substring Without Repeating Characters** `(Medium)` ── **核心分类**：不定长滑动窗口
  - *开发度*：精通。单向拉链法则，`>= left` 铁律护体，死锁左指针绝不倒退，严防踢出窗口的脏数据。
- **209. Minimum Size Subarray Sum** `(Medium)` ── **核心分类**：不定长滑动窗口
  - *开发度*：精通。3 题的反向魔改。右指针扩张（Bulk Up），左指针因满足条件被迫收缩（Weight Loss），贪心抓取最小值。
- **438. Find All Anagrams in a String** `(Medium)` ── **核心分类**：固定长度滑动窗口
  - *开发度*：精通。恒定为 `len(p)` 的窗口。左边开除老员工时数量归零**必须用 `del` 彻底删除键**，否则带 0 对比必然翻车。
- **567. Permutation in String** `(Medium)` ── **核心分类**：固定长度滑动窗口
  - *开发度*：精通。438 题的孪生兄弟，高频哈希账本克隆对比，撞见即熔断。

### 🗼 3. 贪心对撞与蓄水几何鸟望台 (开荒率：100% 满级)
- **11. Container With Most Water** `(Medium)` ── **核心分类**：相向双指针 / 贪心
  - *开发度*：精通。木桶效应决定水位。宽度注定减小时，无脑挪矮柱子去赌更高的未来。
- **15. 3Sum** `(Medium)` ── **核心分类**：排序 + 相向双指针
  - *开发度*：精通。细节狂魔。排序是去重的灵魂，固定 `i` 之后，`L` 和 `R` 两头包抄，三位一体极限跳过重复值。
- **42. Trapping Rain Water** `(Hard)` ── **核心分类**：相向双指针 / 空间极限优化
  - *开发度*：精通。11 题的二维魔改。利用对撞指针，哪边矮就结算哪边，将 $O(n)$ 空间压缩至绝对完美的 $O(1)$。

### 🗼 4. 消消乐栈与单调栈鸟望台 (开荒率：100% 满级)
- **735. Asteroid Collision** `(Medium)` ── **核心分类**：普通栈 / 对称消除
  - *开发度*：精通。利用“碰撞引擎”玩太空消消乐，严格控制正负方向对冲消除。
- **739. Daily Temperatures** `(Medium)` ── **核心分类**：单调递减栈
  - *开发度*：精通。单调栈教科书母题。小黑屋存天数下标，大个子新元素进门轰人，顺手用“今天 - 历史”结算天数差。
- **496. Next Greater Element I** `(Easy/假Easy)` ── **核心分类**：单调栈 + 哈希表
  - *开发度*：精通。标准的工程解耦破局思维。大数组用单调栈做“批发”，结果存入字典；小数组去字典里做 $O(1)$ “零售查询”。
- **503. Next Greater Element II** `(Medium)` ── **核心分类**：单调栈 + 循环数组
  - *开发度*：精通。用 `i % n` 取模超能力，在脑脑里把环形数组拉直虚拟转两圈，不浪费一丁点内存。
- **456. 132 Pattern** `(Medium/Hard级)` ── **核心分类**：倒序单调栈
  - *开发度*：精通。**单调栈的终极梦魇 Boss 题**。倒序遍历，用变量 `ak_2` 继承被轰走的小弟，极限抬高“中介位”门槛，一枪绝杀。

---

## 🌲 【第二主大陆：拉聂尔拉普达 ── 树形、图论与空间战区】
### 📊 整体开塔进度：▓▓▓▓▓▓▓▓░░ 80% (纵深合围，重兵推进 🔥)

你在这部分展现出了极其强悍的**宏观递归大局观**，摸清了前中后序的底层本质，且刚刚推平了 BST 的地盘。

### 🗼 5. 二叉树 DFS 递归与多路同步鸟望台 (开荒率：85%)
- **104. Maximum Depth of Binary Tree** `(Easy)` ── **核心分类**：二叉树 DFS
  - *开发度*：精通。**“甩手掌柜递归流”的开天辟地母题**。问左右要答案 ➔ 挑个大的 ➔ 加个 1 往上交。
- **226. Invert Binary Tree** `(Easy)` ── **核心分类**：二叉树 DFS
  - *开发度*：精通。镜像照镜子，左右副总内部翻转好后，大老板在当前层一刀切交换指针。
- **100. Same Tree** `(Easy)` ── **核心分类**：多路同步递归
  - *开发度*：精通。双线安检流。用三道生死防线（双空、单空、值不等）严丝合缝卡死多树结构。
- **101. Symmetric Tree** `(Easy)` ── **核心分类**：镜像递归
  - *开发度*：精通。镜像探戈。利用 `check(left, right)` 辅助函数强行扩充双参数，实现外侧对外侧、内侧对内侧的绝妙连线。

### 🗼 6. 二叉搜索树与中序织网鸟望台 (开荒率：80%)
- **700. Search in a Binary Search Tree** `(Easy)` ── **核心分类**：BST 极速导航
  - *开发度*：精通。认清了“左小右大”的黄金二分属性。并且建立底层心法：**在 while 循环里用新值覆盖旧值才是真迭代**（Python 不支持尾递归优化）。
- **98. Validate Binary Search Tree** `(Medium)` ── **核心分类**：BST 全局审计 / 前序
  - *开发度*：精通。看穿了局部父子对比的致命漏洞（隐藏叛徒 6），利用前序特权将全局边界 `(low, high)` 像紧箍咒一样层层下发。
- **230. Kth Smallest Element in a BST** `(Medium)` ── **核心分类**：BST 中序迭代流
  - *开发度*：精通。手操物理栈的奇迹。指针 `curr` 负责左滑探底，**`stack.pop()` 在物理本质上就正在扮演那个傲然登场的【根】节点**！
- **450. Delete Node in a BST** `(Medium)` ── **核心分类**：BST 指针动态调整
  - *开发度*：精通。树上最复杂的器官移植手术。拆解出“叶子直接斩断”、“单侧过继”、以及“双翼俱全下找右子树最左下角后继借尸还魂”的三大手术刀场景。
- **426. BST 转双向循环链表** `(Medium/Hard级)` ── **核心分类**：中序遍历 + 三指针织网
  - *开发度*：精通。将中序天然的升序属性，通过 `head`（一锚定音）、`pre`（双向织网）、`cur`（当下孤岛）完美缝合，首尾闭环。

### 🗼 7. 二叉树广度优先 (BFS) 鸟望台 (开荒率：100% 满级 🏆)
- **102. Binary Tree Level Order Traversal** `(Medium)` ── **核心分类**：BFS / 队列
  - *开发度*：精通。水波纹平推。在 `while` 里**必须提前拍照锁死 `queue_size` 发放当层入场券**，彻底隔离当层与下层家属。
- **103. Binary Tree Zigzag Level Order Traversal** `(Medium)` ── **核心分类**：BFS / 双端队列变形
  - *开发度*：精通。蛇形扭动。利用 `collections.deque` 的 **`appendleft()` 在头部插入具有 $O(1)$ 的开挂超能力**，优雅实现当层结果的原地逆序反转。
- **111. Minimum Depth of Binary Tree** `(Easy)` ── **核心分类**：BFS / 元组带权流
  - *开发度*：精通。元组自带 GPS 导航（节点, 深度）入队。**装配核心雷达：一旦撞见第一个既无左也无右的纯叶子节点，代表到了最近终点，直接提款跑路！**
- **2196. Create Binary Tree From Descriptions** `(Medium)` ── **核心分类**：哈希零件工厂 + 逆向构造
  - *开发度*：精通。解耦思维。用 `dict` 当零件工厂无脑造 TreeNode 并连线；用 `set` 登记所有儿子，**最后谁无父无母，谁就是全场大老板 Root**。

---

## 🎭 【第三主大陆：格鲁德高地 ── 高级策略与暴风思维战区】
### 📊 整体开塔进度：▓▓▓▓░░░░░░ 40% (突破海峡，刚建立先头哨所 💥)

这是整个算法面试的“终极修罗场”。你目前利用树形 DFS 积攒下的深厚内功，完美实现了向回溯大区的**无缝大跨步降维打击**。

### 🗼 8. 暴力穷举与树形回溯鸟望台 (开荒率：60%)
- **46. Permutations** `(Medium)` ── **核心分类**：排列回溯
  - *开发度*：精通。回溯算法的开天辟地第一题！做选择（装进背包） ➔ 纵向 DFS 探险 ➔ **撤销选择（`sublist.pop()` 吃后悔药）**。因为排列顺序敏感，每层用 `dict` $O(1)$ 查重并从头扫描。
- **78. Subsets** `(Medium)` ── **核心分类**：组合子集回溯
  - *开发度*：精通。引入变长指针 **`start_index` 筑起防走回头路的高墙**，下一层传入 `i + 1` 强制向后看。每个节点都是合法答案，一进来先交作业。
- **90. Subsets II** `(Medium)` ── **核心分类**：组合去重回溯
  - *开发度*：精通。**大厂面试官最爱的去重副本**。树枝可重，树层不可重。横向分叉路口**先排序（`nums.sort()`）后斩首（`if i > start_index and nums[i] == nums[i-1]: continue`）**，剪掉重复原料。

---

## 🔒 【第四主大陆：战争迷雾未解锁战区】
### 📊 整体开塔进度：░░░░░░░░░░ 0% (大后方待开垦 🔒)

以下 3 个区域鸟望台目前一片漆黑，是海拉鲁大陆最后的未知藏宝地：

- **区域 9：图论高阶（并查集 Union-Find、前缀树 Trie、拓扑排序 207/210）** ── *未开塔*
- **区域 10：贪心策略 (Greedy) & 分治算法 (Divide & Conquer)** ── *未开塔*
- **区域 11：动态规划大本营 (DP / Dynamic Programming)** ── *终极黑魔王，锁死中*

---

# 🌌 海拉鲁大陆：LeetCode 150道黄金母题终极全景通关大账本

> **系统提示：** 本地图全量收录了算法面试中最核心的 ~150 道骨架母题。
> 你的当前进度：**24 / 150 (神庙探索率：16%)**。
> 线性战区已 100% 构筑完毕，请按照路线图向高阶策略大区发起最后的总攻！

---

## 🛡️ 【第一主大陆：海拉鲁平原 ── 底层数据结构与线性战区】
### 📊 区域总统治率：▓▓▓▓▓▓▓▓▓▓ 100% (满级神装 🏆)

### 🗼 1. 数组与极致双指针区块 (Array & Two Pointers)
- [x] **1. Two Sum** `(Easy)` ── 哈希备忘录记账流 **`[已通关 🏆]`**
- [x] **167. Two Sum II - Input Array Is Sorted** `(Medium)` ── 相向双指针对撞 **`[已通关 🏆]`**
- [x] **283. Move Zeroes** `(Easy)` ── 同向快慢指针 / 原地控坑交换 **`[已通关 🏆]`**
- [x] **11. Container With Most Water** `(Medium)` ── 对撞指针 / 木桶效应贪心 **`[已通关 🏆]`**
- [x] **15. 3Sum** `(Medium)` ── 排序 + 三位一体去重相向包抄 **`[已通关 🏆]`**
- [x] **42. Trapping Rain Water** `(Hard)` ── 11题盛水容器二维空间极限压缩 **`[已通关 🏆]`**
- [ ] **26. Remove Duplicates from Sorted Array** `(Easy)` ── 快慢指针原地去重 `[未解锁 🔒]`
- [ ] **80. Remove Duplicates from Sorted Array II** `(Medium)` ── 快慢指针 / 允许重复最多2次高阶控坑 `[未解锁 🔒]`
- [ ] **18. 4Sum** `(Medium)` ── 三数之和外层再嵌套一层循环去重 `[未解锁 🔒]`
- [ ] **31. Next Permutation** `(Medium)` ── 字典序排列的下一个，纯单向寻找位微操 `[未解锁 🔒]`
- [ ] **48. Rotate Image** `(Medium)` ── 矩阵顺时针原地翻转，对角线翻转镜像魔术 `[未解锁 🔒]`
- [ ] **54. Spiral Matrix** `(Medium)` ── 螺旋矩阵，设置上右下左四面边界墙模拟收缩 `[未解锁 🔒]`
- [ ] **73. Set Matrix Zeroes** `(Medium)` ── 矩阵置零，利用首行首列当原位标记位 `[未解锁 🔒]`
- [ ] **135. Candy** `(Hard)` ── 分发糖果，左扫一遍右扫一遍的对撞贪心巅峰 `[未解锁 🔒]`

### 🗼 2. 连续区间滑动窗口区块 (Sliding Window)
- [x] **3. Longest Substring Without Repeating Characters** `(Medium)` ── 不定长滑窗 / 防止left倒退铁律 **`[已通关 🏆]`**
- [x] **209. Minimum Size Subarray Sum** `(Medium)` ── 不定长滑窗反向魔改 / 贪心抓取最小值 **`[已通关 🏆]`**
- [x] **438. Find All Anagrams in a String** `(Medium)` ── 固定长度滑窗 / 归零必 del 彻底抹除 **`[已通关 🏆]`**
- [x] **567. Permutation in String** `(Medium)` ── 固定长度滑窗 / 孪生Anagram子串判定 **`[已通关 🏆]`**
- [ ] **76. Minimum Window Substring** `(Hard)` ── **滑窗终极Boss**。涵盖所有小写大写字母的最小覆盖子串 `[未解锁 🔒]`
- [ ] **30. Substring with Concatenation of All Words** `(Hard)` ── 串联所有单词的子串，滑窗嵌套单词步长微操 `[未解锁 🔒]`
- [ ] **1004. Max Consecutive Ones III** `(Medium)` ── 翻转K个0之后的最长连续1，等价于窗口内最多允许K个0 `[未解锁 🔒]`
- [ ] **424. Longest Repeating Character Replacement** `(Medium)` ── 替换K个字母后的最长重复子串 `[未解锁 🔒]`

### 🗼 3. 极速二分查找边界控制区块 (Binary Search)
- [x] **33. Search in Rotated Sorted Array** `(Medium)` ── 局部有序数组的二分切刀判定 **`[已通关 🏆]`**
- [x] **34. Find First and Last Position in Sorted Array** `(Medium)` ── 撞见target向左右死磕寻找左右边界 **`[已通关 🏆]`**
- [ ] **704. Binary Search** `(Easy)` ── 闭区间 `left <= right` numerically 安全整除防御模板 `[未解锁 🔒]`
- [ ] **35. Search Insert Position** `(Easy)` ── 寻找插入位置，物理本质就是寻找左边界 `[未解锁 🔒]`
- [ ] **74. Search a 2D Matrix** `(Medium)` ── 二维矩阵二分，利用行除以列、模以列将一维下标映射回二维 `[未解锁 🔒]`
- [ ] **153. Find Minimum in Rotated Sorted Array** `(Medium)` ── 寻找旋转排序数组中的最小值 `[未解锁 🔒]`
- [ ] **162. Find Peak Element** `(Medium)` ── 寻找峰值，无序数组利用“爬坡贪心”施展二分神技 `[未解锁 🔒]`
- [ ] **4. Median of Two Sorted Arrays** `(Hard)` ── **二分大Boss**。寻找两个有序数组的中位数，高维切刀对齐 `[未解锁 🔒]`

### 🗼 4. 高阶链表穿针引线区块 (Linked List)
- [x] **206. Reverse Linked List** `(Easy)` ── 三指针原地反转，`return pre` 终极心法 **`[已通关 🏆]`**
- [x] **92. Reverse Linked List II** `(Medium)` ── 引入dummy虚拟头，车厢局部调度头插法 **`[已通关 🏆]`**
- [x] **21. Merge Two Sorted Lists** `(Easy)` ── dummy头开路，双线推进拉拉链合并 **`[已通关 🏆]`**
- [x] **141. Linked List Cycle** `(Easy)` ── 快慢指针操场套圈判环流 **`[已通关 🏆]`**
- [x] **142. Linked List Cycle II** `(Medium)` ── 兔子打回原形同速决战，数学定理 $X=Z$ 定位入口 **`[已通关 🏆]`**
- [ ] **19. Remove Nth Node From End of List** `(Medium)` ── 双指针恒定间距为 $N$，前哨踩空后卫刚好卡在倒数第 $N$ 前驱 `[未解锁 🔒]`
- [ ] **2. Add Two Numbers** `(Medium)` ── 两数相加，链表模拟大数加法，注意维持 carry 进位值 `[未解锁 🔒]`
- [ ] **24. Swap Nodes in Pairs** `(Medium)` ── 两两交换链表中的节点，极其考验临时指针防断链 `[未解锁 🔒]`
- [ ] **25. Reverse Nodes in k-Group** `(Hard)` ── **链表终极Boss**。K个一组反转链表，局部调用206的高级缝合怪 `[未解锁 🔒]`
- [ ] **138. Copy List with Random Pointer** `(Medium)` ── 复制带随机指针的链表，原地复制拆分流或哈希映射流 `[未解锁 🔒]`
- [ ] **143. Reorder List** `(Medium)` ── 重排链表：找中点 + 反转后半段 + 两条链表交叉穿插缝合 `[未解锁 🔒]`
- [ ] **148. Sort List** `(Medium)` ── 链表归并排序，自底向上分割合体，极限压榨空间到 $\mathcal{O}(1)$ `[未解锁 🔒]`
- [ ] **86. Partition List** `(Medium)` ── 分隔链表，开辟小链表和大链表两条 dummy 分头接人最后合体 `[未解锁 🔒]`

### 🗼 5. 消消乐栈与单调栈大本营 (Stack & Monotonic Stack)
- [x] **735. Asteroid Collision** `(Medium)` ── 普通栈消消乐引擎 / 正负对冲对称消除 **`[已通关 🏆]`**
- [x] **739. Daily Temperatures** `(Medium)` ── 单调递减栈教科书母题 / 存下标算天数差 **`[已通关 🏆]`**
- [x] **496. Next Greater Element I** `(Easy)` ── 解耦思维：单调栈“批发”答案，哈希表做“零售” **`[已通关 🏆]`**
- [x] **503. Next Greater Element II** `(Medium)` ── 单调栈 + 循环数组 `i % n` 虚拟转两圈 **`[已通关 🏆]`**
- [x] **456. 132 Pattern** `(Medium)` ── 倒序单调栈斩杀 Boss / 变量 `ak_2` 继承小弟极限抬高门槛 **`[已通关 🏆]`**
- [ ] **20. Valid Parentheses** `(Easy)` ── 经典括号匹配，对称消消乐桶型容器 `[未解锁 🔒]`
- [ ] **155. Min Stack** `(Easy)` ── 最小栈，辅助栈同步压入当前最小值，或者存差值神技 `[未解锁 🔒]`
- [ ] **150. Evaluate Reverse Polish Notation** `(Medium)` ── 逆波兰表达式求值，遇到数字压栈，遇到符号弹两个算完再压回 `[未解锁 🔒]`
- [ ] **224. Basic Calculator** `(Hard)` ── **栈大Boss**。实现通用基本计算器，处理加减括号与双栈符号互倒 `[未解锁 🔒]`
- [ ] **84. Largest Rectangle in Histogram** `(Hard)` ── 柱状图中的最大矩形，单调递增栈寻找左右两侧第一个矮柱子 `[未解锁 🔒]`
- [ ] **85. Maximal Rectangle** `(Hard)` ── 最大矩形，利用 LeetCode 84 题的内核，逐行滚动压缩累加高度 `[未解锁 🔒]`

---

## 🌲 【第二主大陆：拉聂尔天空 ── 树形拓扑、图论与高维搜索战区】
### 📊 区域总统治率：▓▓▓▓▓▓▓▓░░ 80% (纵深合围，胜利在望 🔥)

### 🗼 6. 二叉树 DFS 递归与汇报汇总区块 (Tree DFS)
- [x] **104. Maximum Depth of Binary Tree** `(Easy)` ── “甩手掌柜”自底向上汇报母题 **`[已通关 🏆]`**
- [x] **226. Invert Binary Tree** `(Easy)` ── 镜像照镜子 / 左右指针原地无脑对调 **`[已通关 🏆]`**
- [x] **100. Same Tree** `(Easy)` ── 双线安检 / 三道生死防线卡死结构与数值 **`[已通关 🏆]`**
- [x] **101. Symmetric Tree** `(Easy)` ── 镜像探戈 / check双参数开辟外部战场 **`[已通关 🏆]`**
- [x] **94. Binary Tree Inorder Traversal** `(Easy)` ── 中序遍历大模板 **`[已通关 🏆]`**
- [x] **236. Lowest Common Ancestor of a Binary Tree** `(Medium)` ── 左右军上报线索，大老板当前层当场拦截 LCA **`[已通关 🏆]`**
- [ ] **144. Binary Tree Preorder Traversal** `(Easy)` ── 根左右前序大模板 `[未解锁 🔒]`
- [ ] **145. Binary Tree Postorder Traversal** `(Easy)` ── 左右根后序大模板 `[未解锁 🔒]`
- [ ] **112. Path Sum** `(Easy)` ── 路径总和，自上而下做减法，触底看叶子是否等于目标 `[未解锁 🔒]`
- [ ] **113. Path Sum II** `(Medium)` ── 收集所有路径总和，需要配合回溯 pop 清理背包 `[未解锁 🔒]`
- [ ] **124. Binary Tree Maximum Path Sum** `(Hard)` ── **树形DFS大Boss**。最大路径和，后序位贡献给上层 vs 原地横向开花拼历史新高 `[未解锁 🔒]`
- [ ] **543. Diameter of Binary Tree** `(Easy)` ── 二叉树的直径，利用求深度的后序位，顺手刷新左右高度和的最大值 `[未解锁 🔒]`

### 🗼 7. 二叉搜索树黄金二分区块 (Binary Search Tree)
- [x] **98. Validate Binary Search Tree** `(Medium)` ── 前序投递参数 / 全局边界紧箍咒下发 **`[已通关 🏆]`**
- [x] **230. Kth Smallest Element in a BST** `(Medium)` ── 手操物理栈中序迭代 / `stack.pop()` 即是真命天子【根】 **`[已通关 🏆]`**
- [ ] **700. Search in a Binary Search Tree** `(Easy)` ── BST 黄金查找导航。While 循环原地新值盖旧值才是真迭代 `[未解锁 🔒]`
- [ ] **450. Delete Node in a BST** `(Medium)` ── 树上复杂的器官移植。叶子斩断、独苗过继、双翼俱全下找右子树最左下角后继借尸还魂 `[未解锁 🔒]`
- [ ] **426. Convert BST to Sorted Doubly Circular List** `(Medium)` ── 中序天然升序，通过 `head`（一锚定音）、`pre`（双向织网）完美缝合 `[未解锁 🔒]`
- [ ] **108. Convert Sorted Array to Binary Search Tree** `(Easy)` ── 将有序数组转换为平衡BST，自底向上二分中点平地起高楼 `[未解锁 🔒]`
- [ ] **95. Unique Binary Search Trees II** `(Medium)` ── 构筑所有合法的 BST，利用分治穷举左右子树的所有乐高拼法并嵌套交叉组合 `[未解锁 🔒]`
- [ ] **96. Unique Binary Search Trees** `(Medium)` ── 卡特兰数数学递推，求合法 BST 的总数量 `[未解锁 🔒]`

### 🗼 8. 广度优先水波纹平推区块 (Tree & Graph BFS)
- [x] **102. Binary Tree Level Order Traversal** `(Medium)` ── 提前拍照锁死 `queue_size` 彻底隔离当层与下层家属 **`[已通关 🏆]`**
- [x] **103. Binary Tree Zigzag Level Order Traversal** `(Medium)` ── 利用 `appendleft()` 的 $O(1)$ 头部插入超能力优雅实现蛇形反转 **`[已通关 🏆]`**
- [x] **111. Minimum Depth of Binary Tree** `(Easy)` ── 元组带权流 / 撞见全场第一个纯叶子节点直接提款跑路 **`[已通关 🏆]`**
- [ ] **107. Binary Tree Level Order Traversal II** `(Medium)` ── 自底向上的层序遍历，最终大账本反转或头部插入 `[未解锁 🔒]`
- [ ] **199. Binary Tree Right Side View** `(Medium)` ── 二叉树的右视图，层序平推时，严格只抓 `for` 循环的最后一个老哥（即最右侧可见节点） `[未解锁 🔒]`
- [ ] **116. Populating Next Right Pointers in Each Node** `(Medium)` ── 填充每个节点的下一个右侧指针，层序平推连横线 `[未解锁 🔒]`

### 🗼 9. 高阶图论、拓扑排序与零件工厂区块 (Graph & Advanced Structures)
- [x] **133. Clone Graph** `(Medium)` ── 无向图物理深拷贝 / 哈希映射表阻断鬼打墙死循环 **`[已通关 🏆]`**
- [ ] **2196. Create Binary Tree From Descriptions** `(Medium)` ── 哈希字典零件工厂流水线造树 + 集合去重捞出“无父无母”的大老板 Root `[未解锁 🔒]`
- [ ] **207. Course Schedule** `(Medium)` ── 课程表 I，有向图判环。可用 DFS 染色法（0未访问，1访问中，2安全）或者 BFS 入度表拓扑排序 `[未解锁 🔒]`
- [ ] **210. Course Schedule II** `(Medium)` ── 课程表 II，拓扑排序升级版。不仅要判环，还要输出一条工业级合法的依赖流水线先后顺序 `[未解锁 🔒]`
- [ ] **200. Number of Islands** `(Medium)` ── 岛屿数量，网格图 DFS/BFS 经典母题。撞见陆地“1”触发感染流，无脑炸成海洋“0”，数一数一共触发了几次大爆炸 `[未解锁 🔒]`
- [ ] **695. Max Area of Island** `(Medium)` ── 岛屿的最大面积，在感染流 DFS 的回溯归程期计算并上报这一整片陆地的面积总和 `[未解锁 🔒]`
- [ ] **208. Implement Trie (Prefix Tree)** `(Medium)` ── 实现前缀树（字典树），二十六个叉的高阶树，用于搜索引擎关键词自动补全 `[未解锁 🔒]`
- [ ] **215. Kth Largest Element in an Array** `(Medium)` ── 数组中第K大的元素，**工业级堆（Heap / Priority Queue）经典母题**。维护一个小顶堆，强行控容为K，顶端剩下的就是TopK `[未解锁 🔒]`

---

## 🎭 【第三主大陆：格鲁德高地 ── 高级算法策略与暴风思维战区】
### 📊 区域总统治率：▓▓▓▓░░░░░░ 40% (突破海峡，刚建立先头哨所 💥)

### 🗼 10. 暴力穷举、树形回溯与后悔药区块 (Backtracking)
- [x] **46. Permutations** `(Medium)` ── 排列回溯母题 / 顺序敏感 / 每次重新扫全场 + 字典 $O(1)$ 查重 **`[已通关 🏆]`**
- [x] **78. Subsets** `(Medium)` ── 组合子集回溯母题 / 顺序不敏感 / `start_index` 筑起防回头高墙，一进门先交作业 **`[已通关 🏆]`**
- [x] **90. Subsets II** `(Medium)` ── 组合去重 / 树层去重铁律：在分叉路口**先排序、后斩首**，剪掉重复原料 **`[已通关 🏆]`**
- [x] **39. Combination Sum** `(Medium)` ── 组合总和 / 数字允许无限次重复挑选，下一层纵向递归时无脑传递当前索引 `i` **`[已通关 🏆]`**
- [ ] **40. Combination Sum II** `(Medium)` ── 组合总和 II / 原料自带重复且每个数只能用一次。结合了 39 题与 90 题的高阶超级大缝合题 `[未解锁 🔒]`
- [ ] **77. Combinations** `(Medium)` ── 组合，固定长度为 $k$ 的子集，达到限定人数立刻收网深拷贝 `[未解锁 🔒]`
- [ ] **22. Generate Parentheses** `(Medium)` ── 括号生成，左右括号面临“要还是不要”的单选题，用 `left_used` 和 `right_used` 严格控制剪枝防线 `[未解锁 🔒]`
- [ ] **17. Letter Combinations of a Phone Number** `(Medium)` ── 电话号码的字母组合，多重哈希字典九宫格字母交叉横向拓荒 `[未解锁 🔒]`
- [ ] **79. Word Search** `(Medium)` ── 单词搜索，在二维网格图上展开带回溯性质的 DFS。进门踩坑改字符，出门（撤销选择）必须把字符改回来还原现场 `[未解锁 🔒]`
- [ ] **51. N-Queens** `(Hard)` ── **回溯终极Boss**。N皇后问题，在二维矩阵上利用三个集合（列、正对角线 `r-c`、反对角线 `r+c`）进行高能剪枝防御 `[未解锁 🔒]`

### 🗼 11. 局部最优与区间贪心区块 (Greedy)
- [ ] **55. Jump Game** `(Medium)` ── 跳跃游戏 I，维护一个全场能达到的“最高远历史纪录 `max_reach`”，一路上如果连当前的格点都走不到直接死亡 `[未解锁 🔒]`
- [ ] **45. Jump Game II** `(Medium)` ── 跳跃游戏 II，求最少跳跃次数。利用贪心划分出“当前这一步能承载的边界”，一旦右手踩到边界边缘，立刻步数加1，并刷新下一个大边界 `[未解锁 🔒]`
- [ ] **56. Merge Intervals** `(Medium)` ── 合并区间，**工业界超高频工程题**。先按左端点原地排序，然后拿当前区间的左端点和最终账本里最后一个区间的右端点进行大吞噬合并 `[未解锁 🔒]`
- [ ] **57. Insert Interval** `(Medium)` ── 插入区间，将一个新区间强行插进有序区间里，分左边、中间大吞噬、右边三段式线性缝合 `[未解锁 🔒]`
- [ ] **452. Minimum Number of Arrows to Burst Balloons** `(Medium)` ── 用最少数量的箭引爆气球，区间贪心重叠区间的相交下限求极值 `[未解锁 🔒]`
- [ ] **121. Best Time to Buy and Sell Stock** `(Easy)` ── 买卖股票的最佳时机，维护全场历史见过的“最低价格底座”，每天都假想自己高位抛售刷新最高利润 `[未解锁 🔒]`
- [ ] **122. Best Time to Buy and Sell Stock II** `(Medium)` ── 买卖股票 II，允许无限次交易。目光短浅流贪心巅峰：只要明天的价格比今天高，今天买明天卖，一路上把所有的利润碎屑全部捡进口袋 `[未解锁 🔒]`

### 🗼 12. 愚公移山大任务劈半区间分治区块 (Divide & Conquer)
- [ ] **53. Maximum Subarray** `(Easy)` ── 最大子数组和。虽然用贪心流（财富变成负数立马白手起家重新做人）最优雅，但它底层可以用最正统的分治（左边最大、右边最大、横跨中点最大三者求max）来解决 `[未解锁 🔒]`
- [ ] **23. Merge k Sorted Lists** `(Hard)` ── **分治大Boss**。合并 K 个升序链表。利用两两分治两半合并（Merge Sort的思路），将复杂的多路合并降维打击成最擅长的二路链表合并 `[未解锁 🔒]`
- [ ] **169. Majority Element** `(Easy)` ── 多数元素，摩尔投票法最快；但分治（左半边霸主和右半边霸主PK，谁多听谁的）能帮你建立完美的分治时空图像 `[未解锁 🔒]`

### 🦹 【最终隐藏大本营：城堡核心 ── 动态规划降维打击战区 (DP)】
### 📊 整体开塔进度：░░░░░░░░░░ 0% (终极黑魔王，全面封锁中 🔒)

这是整个算法面试的巅峰防御塔，专门用来解决“求最值、求方法总数”的超级大杀器。

- [ ] **70. Climbing Stairs** `(Easy)` ── 爬楼梯，DP的总母题底座。想走到第 `i` 阶，要么从 `i-1` 跨一步，要么从 `i-2` 跨两步：`dp[i] = dp[i-1] + dp[i-2]` `[未解锁 🔒]`
- [ ] **322. Coin Change** `(Medium)` ── 零钱兑换，背包问题变型。`dp[i]` 代表凑齐总金额 `i` 所需的最少硬币数，横向穷举每种硬币面值向后递推 `[未解锁 🔒]`
- [ ] **300. Longest Increasing Subsequence** `(Medium)` ── 最长递增子序列（LIS），双重 `for` 循环，每个格子都回头看一眼历史所有比我矮的兄弟里的最高成就 `[未解锁 🔒]`
- [ ] **1143. Longest Common Subsequence** `(Medium)` ── 最长公共子序列（LCS），二维 DP 矩阵大对线，字符对得上就斜对角加1，对不上就继承左边或上边的最大值 `[未解锁 🔒]`
- [ ] **72. Edit Distance** `(Hard)` ── **二维DP终极Boss**。编辑距离，两个字符串互相增删改的最小代价，大厂 Senior 级面试压轴核武器 `[未解锁 🔒]`
- [ ] **198. House Robber** `(Medium)` ── 打家劫舍，序列型 DP 经典。今天这家偷不偷？取决于：如果偷，前天财富加上今天的；如果不偷，继承昨天的最大财富 `[未解锁 🔒]`
- [ ] **213. House Robber II** `(Medium)` ── 打家劫舍 II，房子围成一个环。斩断因果流：要么不偷首家（去 1 到 n-1 里面去偷），要么不偷尾家（去 0 到 n-2 里面去偷），两次常规 DP 取最大值 `[未解锁 🔒]`
- [ ] **62. Unique Paths** `(Medium)` ── 不同路径，二维网格走到右下角的方法总数。`dp[i][j]` 只能由它的上面和左边走过来：`dp[i][j] = dp[i-1][j] + dp[i][j-1]` `[未解锁 🔒]`
- [ ] **64. Minimum Path Sum** `(Medium)` ── 最小路径和，在 62 题的基础上，每次走格子都贪心地挑上面和左边里代价比较小的那个加上当前格子的数值 `[未解锁 🔒]`
- [ ] **5. Longest Palindromic Substring** `(Medium)` ── 最长回文子串，区间型 DP。如果两端字符一样且肚子里也是回文，那大区间也是回文 `[未解锁 🔒]`

---

