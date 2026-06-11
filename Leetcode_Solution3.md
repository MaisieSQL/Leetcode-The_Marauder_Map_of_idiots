# 🗼 【区域 6】二叉搜索树 (BST) 核心防御塔开荒大纲

> **战区大局观：** BST（Binary Search Tree）战区天生自带“左小右大”的黄金二分导航属性。大厂面试官极度喜欢在这里高强度考察候选人对边界控制、中序遍历指针微操、以及树形结构动态调整（CRUD）的硬核功底。
> 
> 只要将以下 3 座最具代表性的核心母题防御塔连根拔起，即可在整个 BST 战区宣告 100% 满级统治！

## 🚩 1号塔：LeetCode 98. 验证二叉搜索树 (Validate BST)

* **Boss 难度**：`Medium` / 大厂高频必考面霸
* **守塔核心逻辑**：判定一棵给定的常规二叉树到底是不是合法的二叉搜索树（BST）。

### 🛑 致命直觉陷阱
绝大多数伪架构师一看到这题，本能地觉得很简单：“只要写个递归，看左孩子比我小，右孩子比我大不就行了？” **当场掉进面试官挖的深坑！** 因为 BST 的铁律是：**根节点的整个左子树里的“所有节点”都必须比根节点小。** 光看局部父子关系，左孩子的右孩子极有可能越界比根节点还大！

### 🪓 绝杀开塔心法
1. **边界紧箍咒流（参数传递）**：
   高强度训练如何把全局的上下限范围 $(-\infty, +\infty)$ 像紧箍咒一样，作为递归参数层层向下游节点传递约束。
2. **中序遍历流（物理特性）**：
   利用任何一棵合法 BST 在中序遍历（Inorder Traversal）下，打印出来的数值必定是“严格单调递增”的物理特性一枪爆头。


## 🚩 2号塔：LeetCode 230. 二叉搜索树中第 K 小的元素 (Kth Smallest Element in a BST)

* **Boss 难度**：`Medium` / 大厂高频高回报题
* **守塔核心逻辑**：在一棵合法的 BST 里，找出数值从小到大排在第 $k$ 个的那个目标节点。

### 🛑 必刷母题原因
这题高强度检验对 BST 另一个威震天下的隐藏超能力 ── **“中序遍历（左 $\rightarrow$ 根 $\rightarrow$ 右）”** 的顿悟与计数器微操。

### 🪓 绝杀开塔心法
任何一棵合法的 BST，只要用中序遍历去爬它，打印出来的数字序列**雷打不动是一个绝对完美的升序（从小到大）排列数组**！
因此，本题的本质就是开启中序遍历，在底层内存里数格子。当数到第 $k$ 个节点时，雷达当场锁死，直接提款跑路！它能把你的树形动态计数内功锤炼到常数级常数级。


## 🚩 3号塔：LeetCode 450. 删除二叉搜索树中的节点 (Delete Node in a BST)

* **Boss 难度**：`Medium` / 物理指针微操天花板
* **守塔核心逻辑**：在 BST 里找到一个目标值并把它无情删掉。删掉后还要原地重组剩下的树枝，保持 BST “左小右大”的阵型不乱。

### 🛑 必刷母题原因
相比于单纯的查找（700题）和插入，删除节点涉及到极其复杂的“内脏重组”。是要想在面试现场写出无 Bug 的逻辑，必须具备极强的图解和物理指针连线能力。

### 🪓 绝杀开塔心法
如果要删的节点是叶子或者只有一个孩子，那很好办；但如果要删的节点**既有左子树又有右子树**（比如要把整棵树的腰部关节砍掉），我们需要寻找一位**“完美的继承人”**：
* 必须去被删节点的**右子树里，一路向左狂飙到尽头**，揪出那个右子树里最小的节点。
* 把它顶替到被删节点的位置上，再将其原位置抹去。这题能直接把局部指针接线、换头微操能力拉到 Staff 架构师级别！

---

98. Validate Binary Search Tree

Given the root of a binary tree, determine if it is a valid binary search tree (BST).

A valid BST is defined as follows:

The left subtree of a node contains only nodes with keys strictly less than the node's key.
The right subtree of a node contains only nodes with keys strictly greater than the node's key.
Both the left and right subtrees must also be binary search trees.
 

Example 1:

Input: root = [2,1,3]
Output: true

Example 2:

Input: root = [5,1,4,null,null,3,6]
Output: false
Explanation: The root node's value is 5 but its right child's value is 4.

Constraints:

The number of nodes in the tree is in the range [1, 104].
-231 <= Node.val <= 231 - 1



