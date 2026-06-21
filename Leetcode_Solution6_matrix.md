# Matrix

咱们这就把 LeetCode 的 【二维矩阵与坐标变换大区（Matrix）】 按照 “从易到难、步步为营” 的工业级阶梯完全拆开！矩阵这类题最妙的地方在于：它没有复杂的动态数据结构（没有链表指针，没有树分叉），它的物理外壳永远是固定的 matrix[row][col]。 它的难点演进，纯粹是 【坐标控制精密度的升级】。

```text
【天梯 1 段】 ➔ LeetCode 48.  旋转图像        (主对角线转置 + 水平掉头的几何魔术 🪓)

【天梯 2 段】 ➔ LeetCode 54.  螺旋矩阵        (四面大铁墙边界动态向内合围 ⚔️)

【天梯 3 段】 ➔ LeetCode 73.  矩阵置零        (不花一粒沙子的“借尸还魂记账流” 🔥)

【天梯 4 段】 ➔ LeetCode 240. 搜索二维矩阵 II  (寻找马鞍点的阶梯降维斩杀 🏆)
```
---

# 🛡️ LeetCode 48. Rotate Image (旋转图像)

## 📝 1. 题目描述 (Description)

给定一个 `n × n` 的二维矩阵 `matrix` 表示这一幅图像。请你将图像顺时针旋转 90 度。

- **⚠️ 核心约束**：你必须在 **【原地 (In-place)】** 修改输入矩阵。这意味着你不能开辟一个新的临时二维数组去转存结果，空间复杂度必须死死卡在 $\mathcal{O}(1)$！

### 示例 1:


> **输入**: matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
> **输出**: [[7, 4, 1], [8, 5, 2], [9, 6, 3]]
> **解释**: 
> 原矩阵为：
> [1, 2, 3]
> [4, 5, 6]
> [7, 8, 9]
> 顺时针旋转 90 度后变成：
> [7, 4, 1]
> [8, 5, 2]
> [9, 6, 3]

---

## 🪓 2. 核心算法解析 ── 矩阵转置 + 逐行水平翻转的几何魔法

如果直接在矩阵里搞“四个角落手拉手原地环形覆盖”，代码里需要维护一堆极其复杂的错位下标，极易发生坐标手抖 Bug。

线性代数揭示了一个优美的空间变换规律：**任何方阵顺时针旋转 90 度，在几何上完美等价于：先做【矩阵转置】，再做【逐行水平对轴翻转】！**

### 🏃‍♂️ 物理行军两步走：

1. **第一步：沿主对角线转置（Matrix Transpose）**
   主对角线就是从左上角到右下角的那条线（$1 \rightarrow 5 \rightarrow 9$）。我们沿这条线将矩阵对折，把行和列的索引互换，即让 `matrix[i][j]` 和 `matrix[j][i]` 原地对调。
   - **🚨 核心微操（防无效返工）**：外层 `i` 扫全行，内层 `j` **必须从 `i` 开始（`range(i, n)`）**，也就是只扫矩阵的右上三角！如果 `j` 每次都从 0 开始扫，翻过去的元素在下半场又被翻回来了，原地做无用功。
2. **第二步：逐行水平反转（Reverse Rows）**
   把转置后的矩阵，每一行直接水平调头（第一个和最后一个换，第二个和倒数第二个换）。在 Python 里，直接调用原生 `matrix[i].reverse()` 原地降维打击。

---

## 💻 3. Python3 完美通关代码

```python
class Solution:
    def rotate(self, matrix: List[List[int]]) -> None:
        """
        Do not return anything, modify matrix in-place instead.
        """
        n = len(matrix)
        
        # 1. 物理微操第一步：沿主对角线转置（对称对折）
        for i in range(n):
            # 🚨 灵魂线：j 从 i 开始，只扫描右上三角，防止重复对调
            for j in range(i, n):
                matrix[i][j], matrix[j][i] = matrix[j][i], matrix[i][j]
                
        # 2. 物理微操第二步：将转置后的矩阵每一行原地水平“掉头”
        for i in range(n):
            matrix[i].reverse()
```

---

# 🛡️ LeetCode 54. Spiral Matrix (螺旋矩阵)

## 📝 1. 题目描述 (Description)

给你一个 `m × n` 的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

### 示例 1:


> **输入**: matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
> **输出**: [1, 2, 3, 6, 9, 8, 7, 4, 5]

---

## 🪓 2. 核心算法解析 ── 四面大铁墙边界动态向内合围流

这道题的几何外壳是一条顺时针不断“转弯、向内盘旋”的贪吃蛇。

最优雅的工业级解法是 **【四墙合围法】**。我们不需要算步数，也不需要用方向数组，而是直接在矩阵的四个最外缘筑起四面大铁墙：
* **`top = 0`**（顶墙）
* **`bottom = len(matrix) - 1`**（底墙）
* **`left = 0`**（左墙）
* **`right = len(matrix[0]) - 1`**（右墙）

### 🏃‍♂️ 贪吃蛇四路循环大扫荡：

贪吃蛇按照 **“向右 ➔ 向下 ➔ 向左 ➔ 向上”** 的钢铁纪律路线循环行军。每扫完一行或一列（代表这面墙的数据彻底报废），对应的铁墙就立刻向内收缩一级，**并立刻进行安全审计（熔断检查）**：

1. **➡️ 1. 向右冲锋（扫顶行）**：
   从当前的 `left` 一路扫到 `right`。扫完后，顶层宣告作废，顶墙向下压一级（`top += 1`）。
   - **🚨 核心安检**：如果发现 `top > bottom`，说明上下大军已经撞头，战场清洗完毕，直接 `break`！
2. **⬇️ 2. 向下冲锋（扫右列）**：
   从当前的 `top` 一路扫到 `bottom`。扫完后，右侧列宣告作废，右墙向左缩一级（`right -= 1`）。
   - **🚨 核心安检**：如果发现 `left > right`，说明左右大军撞头，直接 `break`！
3. **⬅️ 3. 向左冲锋（扫底行）**：
   从当前的 `right` **倒序**一路扫回 `left`（`range(right, left - 1, -1)`）。扫完后，底层作废，底墙向上抬一级（`bottom -= 1`）。
   - **🚨 核心安检**：如果 `top > bottom`，立马熔断。
4. **⬆️ 4. 向上冲锋（扫左列）**：
   从当前的 `bottom` **倒序**一路扫回 `top`（`range(bottom, top - 1, -1)`）。扫完后，左侧列作废，左墙向右推一级（`left += 1`）。
   - **🚨 核心安检**：如果 `left > right`，立马熔断。

> 💡 **矩阵走水口诀**：**“每扫完一面墙，高墙立刻往内缩；缩完秒做安全检，撞墙瞬间就收工！”**

---

## 💻 3. Python3 完美通关代码

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        # 1. 进来先做边界防御
        if not matrix or not matrix[0]:
            return []
            
        ans = []
        # 2. 顶格摆开四面大铁墙边界
        top = 0
        bottom = len(matrix) - 1
        left = 0
        right = len(matrix[0]) - 1
        
        # 3. 开启四路循环大扫荡
        while True:
            # ➡️ A. 向右冲锋扫顶行（从左到右）
            for col in range(left, right + 1):
                ans.append(matrix[top][col])
            top += 1  # 顶墙无情下压一级
            if top > bottom:  # 🚨 安全审计：一旦越界，当场熔断
                break
                
            # ⬇️ B. 向下冲锋扫右列（从上到下）
            for row in range(top, bottom + 1):
                ans.append(matrix[row][right])
            right -= 1  # 右墙无情左移一级
            if left > right:  # 🚨 安全审计：一旦越界，当场熔断
                break
                
            # ⬅️ C. 向左冲锋扫底行（倒序！从右到左）
            for col in range(right, left - 1, -1):
                ans.append(matrix[bottom][col])
            bottom -= 1  # 底墙无情上抬一级
            if top > bottom:  # 🚨 安全审计：一旦越界，当场熔断
                break
                
            # ⬆️ D. 向上冲锋扫左列（倒序！从下到上）
            for row in range(bottom, top - 1, -1):
                ans.append(matrix[row][left])
            left += 1  # 左墙无情右移一级
            if left > right:  # 🚨 安全审计：一旦越界，当场熔断
                break
                
        # 4. 交出全场螺旋大扫荡战报
        return ans
```

---

# 🛡️ LeetCode 54. Spiral Matrix (螺旋矩阵)

## 📝 1. 题目描述 (Description)

给你一个 `m × n` 的矩阵 `matrix` ，请按照 **顺时针螺旋顺序** ，返回矩阵中的所有元素。

### 示例 1:


> **输入**: matrix = [[1, 2, 3], [4, 5, 6], [7, 8, 9]]
> **输出**: [1, 2, 3, 6, 9, 8, 7, 4, 5]

---

## 🪓 2. 核心算法解析 ── 四面大铁墙边界动态向内合围流

这道题的几何外壳是一条顺时针不断“转弯、向内盘旋”的贪吃蛇。

最优雅的工业级解法是 **【四墙合围法】**。我们不需要算步数，也不需要用方向数组，而是直接在矩阵的四个最外缘筑起四面大铁墙：
* **`top = 0`**（顶墙）
* **`bottom = len(matrix) - 1`**（底墙）
* **`left = 0`**（左墙）
* **`right = len(matrix[0]) - 1`**（右墙）

### 🏃‍♂️ 贪吃蛇四路循环大扫荡：

贪吃蛇按照 **“向右 ➔ 向下 ➔ 向左 ➔ 向上”** 的钢铁纪律路线循环行军。每扫完一行或一列（代表这面墙的数据彻底报废），对应的铁墙就立刻向内收缩一级，**并立刻进行安全审计（熔断检查）**：

1. **➡️ 1. 向右冲锋（扫顶行）**：
   从当前的 `left` 一路扫到 `right`。扫完后，顶层宣告作废，顶墙向下压一级（`top += 1`）。
   - **🚨 核心安检**：如果发现 `top > bottom`，说明上下大军已经撞头，战场清洗完毕，直接 `break`！
2. **⬇️ 2. 向下冲锋（扫右列）**：
   从当前的 `top` 一路扫到 `bottom`。扫完后，右侧列宣告作废，右墙向左缩一级（`right -= 1`）。
   - **🚨 核心安检**：如果发现 `left > right`，说明左右大军撞头，直接 `break`！
3. **⬅️ 3. 向左冲锋（扫底行）**：
   从当前的 `right` **倒序**一路扫回 `left`（`range(right, left - 1, -1)`）。扫完后，底层作废，底墙向上抬一级（`bottom -= 1`）。
   - **🚨 核心安检**：如果 `top > bottom`，立马熔断。
4. **⬆️ 4. 向上冲锋（扫左列）**：
   从当前的 `bottom` **倒序**一路扫回 `top`（`range(bottom, top - 1, -1)`）。扫完后，左侧列作废，左墙向右推一级（`left += 1`）。
   - **🚨 核心安检**：如果 `left > right`，立马熔断。

> 💡 **矩阵走水口诀**：**“每扫完一面墙，高墙立刻往内缩；缩完秒做安全检，撞墙瞬间就收工！”**

---

## 💻 3. Python3 完美通关代码

```python
class Solution:
    def spiralOrder(self, matrix: List[List[int]]) -> List[int]:
        # 1. 进来先做边界防御
        if not matrix or not matrix[0]:
            return []
            
        ans = []
        # 2. 顶格摆开四面大铁墙边界
        top = 0
        bottom = len(matrix) - 1
        left = 0
        right = len(matrix[0]) - 1
        
        # 3. 开启四路循环大扫荡
        while True:
            # ➡️ A. 向右冲锋扫顶行（从左到右）
            for col in range(left, right + 1):
                ans.append(matrix[top][col])
            top += 1  # 顶墙无情下压一级
            if top > bottom:  # 🚨 安全审计：一旦越界，当场熔断
                break
                
            # ⬇️ B. 向下冲锋扫右列（从上到下）
            for row in range(top, bottom + 1):
                ans.append(matrix[row][right])
            right -= 1  # 右墙无情左移一级
            if left > right:  # 🚨 安全审计：一旦越界，当场熔断
                break
                
            # ⬅️ C. 向左冲锋扫底行（倒序！从右到左）
            for col in range(right, left - 1, -1):
                ans.append(matrix[bottom][col])
            bottom -= 1  # 底墙无情上抬一级
            if top > bottom:  # 🚨 安全审计：一旦越界，当场熔断
                break
                
            # ⬆️ D. 向上冲锋扫左列（倒序！从下到上）
            for row in range(bottom, top - 1, -1):
                ans.append(matrix[row][left])
            left += 1  # 左墙无情右移一级
            if left > right:  # 🚨 安全审计：一旦越界，当场熔断
                break
                
        # 4. 交出全场螺旋大扫荡战报
        return ans
```

---

