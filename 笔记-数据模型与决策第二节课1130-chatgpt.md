

---

# 目录

**Part A — 理论与基本概念（老师课堂主线）**  
**Part B — Excel 建模四要素（完整流程 + SUMPRODUCT）**  
**Part C — 敏感性分析（Shadow Price 全解析）**  
**Part D — 例题1：电脑生产组合 LP（你的截图全部整合）**  
**Part E — 例题2：环球影城排班（整数规划）**  
**Part F — 例题3：医院 DEA（数据包络分析）**  
**Part G — 第二节课知识图谱（全结构总结）**

---

# ————————————————

# **Part A. 课堂理论主线（基于 Cht3 + 录音）**

# ————————————————

## 🌟 1. 什么是线性规划（Linear Programming, LP）

课堂老师原话（录音）：

> “LP 是我们目前唯一能在多项式时间里有效求解的优化模型。”

LP 必须同时满足三要素：

### **(1) 决策变量（Changing Cells in Excel）**

代表你要决定的数量  
例：

- x₁ = Basic 电脑产量
    
- x₂ = XP 电脑产量  
    

---

### **(2) 目标函数（Objective Cell）**

形式是线性的：  
[  
\max Z = 80 x_1 + 129 x_2  
]

**老师强调：unit margin（单位利润）就是目标的系数。**

---

### **(3) 约束条件（Constraints）**

都是“线性表达式”：

例：

- 装配时间：5x₁ + 6x₂ ≤ 10,000
    
- 测试时间：x₁ + 2x₂ ≤ 3,000
    
- 销售上限：x₁ ≤ 600、x₂ ≤ 1,200  
    

---

## 🌟 2. 图解法（老师黑板推导过程）

老师课堂步骤完全对应 Cht3：

1️⃣ 将每个约束画成直线  
2️⃣ 用不等号划出“可行域”  
3️⃣ 目标函数画成 “等利润直线”  
4️⃣ 沿利润方向移动，直到碰到区域边界  
5️⃣ **最优解一定在可行域的顶点（Corner Point）**

课堂结论：  
[  
x_1=560,\quad x_2=1200  
]  
利润 199,600  
（与你 Screenshot 结果一致）

---

## 🌟 3. Excel LP 建模结构（老师上课重点）

Excel 需要四个区：

### A. 输入区（Input）

蓝色单元格，不参与求解  
包括：

- 每种产品的工时
    
- 销售上限
    
- 总工时资源
    
- 单位利润  
    （图示内容你截图中已有）
    

### B. 决策变量区（Changing Cells）

红色单元格（不能放公式）

- Number_to_produce: x₁, x₂
    

### C. 目标函数区（Objective Cell）

Excel 公式：

```
=SUMPRODUCT(Unit_Margins, Number_to_produce)
```

### D. 约束计算区

例如 Assembly Hours：

```
=SUMPRODUCT(Assembly_per_unit, Number_to_produce)
```

老师强调：

> “成批相乘求和，一律 SUMPRODUCT。”

---

## 🌟 4. 求解器（Solver）设置

老师课堂完整流程：

- **目标单元格设为 Max**
    
- **可变单元格：产量两格**
    
- **约束：**
    
    - 使用量 ≤ 资源可用量
        
    - 产量 ≤ 销售上限
        
- 选择 **Simplex LP**
    

求得最优解（与你截图一致）。

---

## 🌟 5. 线性规划的三个假设（必须满足）

老师在录音中强调：

|假设|含义|
|---|---|
|比例性|每件产品的资源消耗 × 数量|
|可加性|不同产品的资源使用直接相加|
|可分性（Divisibility）|可以非整数解（除非要求整数）|

老师强调：

> “变量不能出现在分母里，那就不是线性。”

（你在排班问题里遇到部门 ∕ 总人数时便违反此规则）

---

# ————————————————

# **Part B. Excel建模全流程（课堂 + 例题融合版）**

# ————————————————

以下流程适用于每个 LP 问题（例题1/2/3 都用到）。

---

## 🎯 Step 1：输入区（蓝色）

把所有固定数据输入。  
示例（来自例题1）：

||Basic|XP|
|---|---|---|
|Unit Profit|80|129|
|Assembly Hours|5|6|
|Testing Hours|1|2|
|Max Sales|600|1200|

---

## 🎯 Step 2：命名区域（Naming）

从截图可知你对以下区域命名：

- Unit_Margins
    
- Assembly_hours
    
- Testing_hours
    
- Number_to_produce
    
- Max_sales  
    等
    

老师课堂同样强调命名 → 让公式可读性提升。

---

## 🎯 Step 3：决策变量区（红色）

例题1：

||Number_to_produce|
|---|---|
|Basic|x₁|
|XP|x₂|

**不能有公式！只能写数字。**

---

## 🎯 Step 4：目标函数（Objective）

Excel 写：

```
=SUMPRODUCT(Unit_Margins, Number_to_produce)
```

---

## 🎯 Step 5：约束计算区

Assembly Used：

```
=SUMPRODUCT(Assembly_hours, Number_to_produce)
```

Testing Used：

```
=SUMPRODUCT(Testing_hours, Number_to_produce)
```

---

## 🎯 Step 6：求解器参数

目标：

- **Set Objective:** 总利润
    
- **To:** Max
    
- **Change Variable Cells:** x₁:x₂
    

添加约束：

- Assembly_used ≤ Assembly_available
    
- Testing_used ≤ Testing_available
    
- Number_to_produce ≤ Max_sales
    
- Number_to_produce ≥ 0
    

方法：**Simplex LP**

---

# ————————————————

# **Part C. 敏感性分析模板（老师讲得很重要）**

# ————————————————

## 🌟 1. Shadow Price（影子价格）

含义：

> “资源多 1 单位，目标函数增加多少？”

例题1（截图内容）：

- 装配时间 Shadow Price = **16**
    
- 测试时间 Shadow Price = **0**
    

解释：

- 装配时间用满 → 成为 Binding
    
- 多 1 小时装配 → 利润 ↑ 16
    
- 测试时间没有 Binding → 增加测试资源无价值
    

---

## 🌟 2. Allowable Increase / Decrease

例题1（截图）：

- 装配时间可增加到 10200
    
- 或减少到 7200  
    在此区间内 Shadow Price = 16 恒成立。
    

---

## 🌟 3. Reduced Cost（未生产的产品）

XP 的 Reduced Cost = 0（有生产）  
Basic 的 Reduced Cost = 0（也有生产）

如果某产品 **未生产**，Reduced Cost 会告诉你：  
→ “再加多少收益系数才能让它变得可行”。

---

# ————————————————

# **Part D. 例题1：完整 LP 模型 + Excel 步骤（重组你的截图内容）**

# ————————————————

## 🖥️ 例题1：电脑生产组合 Assembling and Testing Computers

（引用你的文件）

---

## 1. 决策变量（老师课堂常写法）

[  
x_1 = \text{Basic 生产量}  
]  
[  
x_2 = \text{XP 生产量}  
]

---

## 2. 目标函数

单位利润（截图中计算）：

- Basic：300 − 150 = 150 − Labor(5×11 + 1×15) = 80
    
- XP：450 − 225 − Labor(6×11 + 2×15) = 129
    

所以目标函数为：

[  
\max Z=80x_1 +129x_2  
]

---

## 3. 约束条件

### 销售上限

[  
x_1 \le 600,\quad x_2 \le 1200  
]

### 装配资源

[  
5x_1+6x_2 \le 10000  
]

### 测试资源

[  
x_1+2x_2 \le 3000  
]

### 非负

[  
x_1, x_2 \ge 0  
]

---

## 4. Excel 公式（根据你的截图重建）

### Assembly Used

```
=SUMPRODUCT(Assembly_hours, Number_to_produce)
```

### Testing Used

```
=SUMPRODUCT(Testing_hours, Number_to_produce)
```

### Profit

```
=SUMPRODUCT(Unit_Margins, Number_to_produce)
```

---

## 5. 求解器设置（文字重现你截图）

- Objective: Profit
    
- Max
    
- Changing cells: x₁:x₂
    
- Constraints:
    
    - Assembly_used ≤ 10000
        
    - Testing_used ≤ 3000
        
    - x₁ ≤ 600
        
    - x₂ ≤ 1200
        

---

## 6. 最优解（与你一致）

[  
x_1 =560,\quad x_2=1200,\quad Z=199{,}600  
]

---

## 7. 敏感性报告（截图内容重建）

### 影子价格：

- 装配：16（Binding）
    
- 测试：0（Non-binding）
    

### Allowable increase/decrease

- 装配：+2000 / −2800
    
- 测试：较大区间（未绑定）
    

---

# ————————————————

# **Part E. 例题2：环球影城排班（整数规划）**

# ————————————————

来自你的文件：

---

## 🎯 任务：

满足每一天的最低员工需求，同时**最小化总员工数**。

---

## 1. 决策变量

课堂 + PPT + 截图一致：

[  
x_j =\text{从星期 j 开始连续工作 5 天的员工人数}  
]

j = Mon, Tue, …, Sun（共7类）

---

## 2. 目标函数

[  
\min Z=x_1+x_2+x_3+x_4+x_5+x_6+x_7  
]

---

## 3. 每天的需求约束（来自课件 Cht4）

根据“五天上两天休”周期推导：

- Monday: x₁ + x₄ + x₅ + x₆ + x₇ ≥ 17
    
- Tuesday: x₁ + x₂ + x₅ + x₆ + x₇ ≥ 13
    
- Wednesday: x₁ + x₂ + x₃ + x₆ + x₇ ≥ 15
    
- Thursday: x₁ + x₂ + x₃ + x₄ + x₇ ≥ 19
    
- Friday: x₁ + x₂ + x₃ + x₄ + x₅ ≥ 14
    
- Saturday: x₂ + x₃ + x₄ + x₅ + x₆ ≥ 16
    
- Sunday: x₃ + x₄ + x₅ + x₆ + x₇ ≥ 11
    

---

## 4. Excel 建模（你截图步骤重建）

### Step 1: 输入区

|Day|Mon|Tue|...|Sun|
|---|---|---|---|---|
|Demand|17|13|...|11|

### Step 2: 决策变量

x₁~x₇（整数）

### Step 3: 每天工作人数

例如 Monday：

```
= x1 + x4 + x5 + x6 + x7
```

你的截图中每一列都对应公式重建。

---

## 5. 求解器设置

- Objective: Min
    
- Variables: x₁:x₇
    
- Constraints:
    
    - 每天工作人数 ≥ Demand
        
    - x_j ≥ 0
        
    - Integer（你的截图特意显示无法敏感性分析）
        

---

## 6. 最优解（你截图中的结果）

求得的最小员工数为：  
**21 人**

你在课堂曾出现错误（录音里你说 “我知道我21错哪了”）  
最终老师确认正确答案 = **21**

---

## 7. 特殊约束：

“周一开始工作的人 ≥ 总人数的 50%”

老师强调：

🚫 **变量不能出现在分母（非线性）**

正确线性化方法：  
[  
x_1 \ge 0.5(x_1+x_2+...+x_7)  
]

即  
[  
x_1 -0.5x_1-0.5x_2-...-0.5x_7\ge 0  
]

课堂截图已给出，你成功使用该模型求解。

---

# ————————————————

# **Part F. 例题3：医院 DEA 模型（数据包络分析）**

# ————————————————

来自你的文件：

课堂 Cht4 有明确说明：

---

## 1. DEA 的基本思想

对每个医院设定：

- 输入权重：u₁, u₂
    
- 输出权重：v₁, v₂, v₃
    

**让目标医院在权重下“效率最大”，但其他医院效率必须 ≤1**

---

## 2. 决策变量

[  
u_1,u_2,v_1,v_2,v_3 \ge 0  
]

---

## 3. 目标函数（以医院 1 为例）

[  
\max ; \text{Efficiency}_1=\frac{v_1y_{11}+v_2y_{21}+v_3y_{31}}{u_1x_{11}+u_2x_{21}}  
]

线性化 → 设置：

[  
u_1x_{11}+u_2x_{21}=1  
]

目标变为：

[  
\max v_1y_{11}+v_2y_{21}+v_3y_{31}  
]

---

## 4. 约束条件

对每个医院 k：

[  
v_1y_{1k}+v_2y_{2k}+v_3y_{3k} \le u_1x_{1k}+u_2x_{2k}  
]

你截图中逐个医院求解，得到：

- 医院1：效率 = 1
    
- 医院2：效率 < 1
    
- 医院3：效率 = 1
    

结论与你截图一致：

> 医院 1 和 3 比医院 2 更有效率。

---

# ————————————————

# **Part G. 第二节课知识图谱（整体复习结构）**

# ————————————————

## 🧠 Linear Programming 学习结构

1. LP 三要素
    
    - Variables
        
    - Objective
        
    - Constraints
        
2. 图解法
    
    - 可行域
        
    - 边界
        
    - 顶点最优
        
3. Excel 模型
    
    - 输入区
        
    - 命名
        
    - SUMPRODUCT
        
    - 求解器 (Simplex LP)
        
4. 敏感性分析
    
    - Shadow Price
        
    - Binding / Non-binding
        
    - Reduced Cost
        
    - Allowable Increase/Decrease
        
5. 应用模型
    
    - 产品组合（例题1）
        
    - 排班（例题2）
        
    - DEA 效率评价（例题3）
        

---

