## Lamarck &nbsp; &nbsp; &nbsp; 2025-08-26
#### 该文档用于记录 ChimeraX 命令行的常用指令
---

> **01 查看 ChimeraX 的工作目录**
```bash
pwd
```

> **02 打开工作目录中的某个 pdb 文件**
```bash
open asu.pdb
```

> **03 关闭某个结构文件**
```bash
close #1     # 关闭单个文件
close all    # 关闭全部文件
```

> **04 导出选中的结构**
```bash
save asu.pdb selectedOnly true
```

> **05 选中某条链的残基**
```bash
select #1/A:100-200        # 选中连续的残基
select #1/A:100,150,200    # 选中离散的残基
select ~sel                # 反选残基
```

> **06 按链拆分某个模型**
```bash
split #1
```

> **07 删除某个氨基酸，从而实现链的断开**
```bash
delete #1/C:72            # 删除单个残基
delete #1/C:72-78         # 删除连续残基
delete #1/C:72,75,78      # 删除离散残基
```

> **08 寻找邻近的残基**
```bash
select zone #1/C:260-268 5    # 选中距离目标片段 5 Å 以内的残基
```

> **09 把 model1 和 model2 叠在一起（看结构上的突变 / 差异，log 会自动给出 RMSD）**

`mm` 是 `matchmaker` 的简写
```bash
mm #3 to #1
```

> **10 连续选中残基**
```text
按住 Ctrl + Shift，左键
```

> **11 自动选中复合物的相互作用位点**
```text
Molecule Display 导航栏 → 点 Interfaces → 在右下角无向图中左键某条线 → 选择 Select contact residues of xx and xx
```

> **12 把结构导出为 png 并设置透明背景**
```bash
save output.png transparentBackground true
```

> **13 手动设置选中片段的颜色**
```text
选中结构后，导航栏依次点：Actions → Color
```

> **14 处理 T=3 衣壳的 ASU —— 给链重命名并从 1 开始重新编号**

当前 pdb 有三条链，但是编号是：`/A 1-245, /B 246-440, /B 442-637`，需要改造成：`/A 1-245, /B 1-195, /C 1-196`
```bash
select #1/A:1-245
save A_chain.pdb selectedOnly true
select #1/B:246-440
save B_chain.pdb selectedOnly true
select #1/B:442-637
save C_chain.pdb selectedOnly true

close #1
open A_chain.pdb
open B_chain.pdb
open C_chain.pdb

changechains #1 A
changechains #2 B
changechains #3 C

combine #1-3 name ABC
save ABC.pdb model #4
close #1-4

open ABC.pdb
renumber #1 start 1 relative false
save ABC.pdb model #1
```

> **15 调整选中部分的透明度**
```bash
transparency sel 60 target ac
```

##### [ChimeraX 官方文档](https://www.cgl.ucsf.edu/chimerax/docs/user/index.html)
