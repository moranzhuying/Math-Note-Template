# Math-Note-Template

一个面向数学笔记与书籍写作的 LaTeX 模板，基于 `ctexbook` 构建，内置统一的定理环境体系、数学符号库与交换图支持。

## 目录结构

```
.
├── main.tex              # 主文件：定义 \part 并汇总各章节
├── structure.sty         # 核心样式：页面布局、定理环境、符号库、引用
├── quiver.sty            # 交换图支持（q.uiver.app 导出，封装 tikz-cd）
├── Content/              # 内容目录
│   ├── Preface/          # 前言：全书结构、更新记录、记号说明
│   ├── Test_Chapter/     # 正文测试章节（三级嵌套示例）
│   │   ├── intro.tex         #   本章导引
│   │   ├── 1_Test_Section/   #   节：index.tex + 小节文件
│   │   ├── Summary/          #   本章小结
│   │   └── Appendix_Test_A/  #   章内附录
│   └── Appendix/         # 后记：术语对照表、参考文献
├── ExerciseBook/         # 习题集（模式 1 时使用，由脚本生成）
├── Figures/              # 插图目录
├── README.md             # 本文件：模板总览
├── 正文写作规范.md        # 正文的写法约定
├── 模板使用规范.md        # 使用模板的结构与技术约束
├── 术语对照表.md          # 数学术语的中法英对照
├── ChangeLog.md          # 模板更新记录
├── commit.py             # 一键提交脚本（Python，Windows 可直接运行）
├── commit.sh             # 一键提交脚本（Bash 版，兼容）
├── commit.md             # commit.py / commit.sh 的说明
├── symbols.md            # 符号管理脚本的说明（工作区级工具）
├── setup_mode.py         # 习题编排模式切换脚本（模式 1 / 2）
└── setup_mode.md         # setup_mode.py 的说明
```

## 模板特点

### 1. 页面布局

- **纸张与边距**：A4，左右 1.5cm、上下 2cm，单倍行距（`geometry`）。
- **字体**：中文由 `ctexbook` 处理；英文用 Latin Modern（Computer Modern 系）。
- **标题**：`ctexbook` 默认让「Chapter 1」与标题行差一个字级，这里统一为 `\Huge`；`\section` 用 `titlesec` 设为 `\Large` ，加粗并左对齐。
- **页眉页脚**：页码居中于页脚底部，无页眉横线（`fancyhdr`）。
- **目录**：标题通栏，目录内容双栏，减少占页数；PDF 内的书签可以显示精确到小节的编号。

### 2. 定理环境与引用

**在个人需求的基础上，内置了 22 种环境**，基于 `tcolorbox`配置，按照不同的类别配色，允许跨页断行。

| 类别   | 环境                                                                                      |
| ---- | --------------------------------------------------------------------------------------- |
| 基础陈述 | `definition`（蓝）、`axiom`（靛蓝）、`hypothesis`（青）                                             |
| 推演结论 | `theorem`（红）、`lemma`（橙）、`proposition`（紫）、`corollary`（绿）、`metatheorem`（靛蓝）、`criteria`（茶） |
| 补充说明 | `problem`（黄）、`example`（绿）、`remark`（灰）                                                   |
| 习题   | `exercise`（品红，独立编号）、`solution`（灰，题后即答）                                                  |
| 其他   | `algorithm`、`convention`、`alarm`、`sketch`（分析，带编号）                                       |
| 无编号  | `proof`（自动加 ∎）、`answer`、`hint`、代码块                                                      |

**编号机制**：定理类环境**共用一个计数器** `mathcount`（按章编号，即「定义 1.1、命题 1.2、定理 1.3」连号），习题则用独立计数器 `exercisecount`。

**用法**：`\begin{环境名}{标题}{label}`，两个参数都不能省；label 留空即可，需要引用时才填。详见《正文写作规范》第三节。

**引用**：支持`hyperref` 和 `cref`。

**跨文档引用**：在习题集引用笔记中的定理时，使用 `\noteref{标签}`，在两种习题编排模式下，自动展开为跨文档引用或本地引用，习题文件因而完全可移植。详见《模板使用规范》第六节。

### 3. 数学公式

引入的宏包：`amsmath`、`amssymb`、`amsfonts`、`amsthm`、`mathtools`、`mathrsfs`、`stmaryrd`、`bm`。

### 4. 交换图与图表

引入的宏包：

- **绘图**：`tikz`，`pgfplots`
- **交换图**：`tikz-cd`。模板里引入了 `quiver.sty` ，可以直接使用 q.uiver.app 上导出的tikz代码。
- **插图**：`graphicx`
- **表格**：`booktabs`、`longtable`、`tabularx`、`multirow`、`array`、`caption`、`subcaption`

### 5. 符号库

`structure.sty` 的模块 VI 预置三类常用记号，共 147 条：**代数 98 条、几何 33 条、分析 16 条**。如果希望新增记号，请使用`symbols.py`，其具体使用说法见 [symbols.md](./symbols.md)。

### 6. 内容结构

正文采用 **Chapter → Section → 小节文件** 三级嵌套，每层目录各有一个 `index.tex` 汇总、逐级 `\input`：

- 每章一个目录，章内可带 `intro.tex` 与 `Summary/`；
- 每节一个目录，节内可带 `Summary/` 与章内附录；
- 长文档因此拆成小文件，便于维护与复用。

**`intro.tex` 与 `Summary/` 的用途**：

| 位置                         | 用途                                                                                       |
| -------------------------- | ---------------------------------------------------------------------------------------- |
| `Chapter/intro.tex`        | **本章导引**——用 `\section*` 写成不编号的一节，手工加入目录（`\addcontentsline`）。交代本章要解决的问题、与前后章的关系、以及阅读顺序建议。 |
| `Chapter/Summary/`         | **本章小结**——以 `\subsection` 起头的「本章小结」节，汇总本章的主要结论与引入的记号，便于回查。                               |
| `Chapter/Section/Summary/` | **本节小结**——同上，为「本节小结」。                                                                    |

导引与小结都用**无编号标题**（`\section*` / `\subsection*`），因为它们是正文的辅助线索，不应占据编号。

**新增章节**时，在 `Content/` 下按三级结构新建目录，每层建 `index.tex`，并在上一级 `\input` 引入。

**列表环境**：`enumitem` 定制了 `enumerate` / `itemize` 的标签格式，逐级区分。

### 7. 习题集

**目的**：让习题与正文各自独立，同时保持两者**相互引用而无需人工同步**。

- 正文可以只讲理论、不含习题，保持阅读的连贯；
- 习题可以独立成册，按难度或主题另行编排；
- 两者之间的引用（习题引用正文的定理、正文指路相关习题）由模板自动处理。

**基本要求**：

- 习题中引用正文的定理、定义时，一律写 `\noteref{标签}`，**不要**直接写 `\cref` 或 `\ref`。
- 正文中引用习题时不需特殊处理，按常规 `\cref` 写即可（同一文档内）。

**基于不同的需求，模板采用了2种编排模式**，以下是对二者的简要说明。

- **模式 1（默认）**：正文无习题，将习题集作为另一本书放在 `ExerciseBook/`，与 `Content/` 同级，通过 `xr-hyper` 跨文档引用笔记的编号。
- **模式 2：**&#x4E60;题作为 Section 置于每章章末，目录为 `Content/NN_Chapter/Exercise/`。

你可以使用 `setup_mode.py` 在模式 1 和模式 2 之间进行切换，该脚本的详细说明参见 [setup\_mode.md](./setup_mode.md)。

> **脚本会做什么**：同步目标目录使其镜像正文结构（缺失的小节自动补建并接入 `index.tex`），并自动管理 `\noteref` 宏的覆盖行。因此**切换模式只需运行一次脚本，习题源码零修改**。

### 8. 写作规范

基于本人的实际写作需求，模板配备了三份规范文档，以下是简要介绍：

- [正文写作规范.md](./正文写作规范.md) —— 正文的写法。
- [模板使用规范.md](./模板使用规范.md) —— 模板基本结构的说明。
- [术语对照表.md](./术语对照表.md) —— 部分数学术语的中法英对照。

> **这三份文档仅反映作者的写作习惯，** 你完全可以按照**自己喜欢的风格使用模板。**

## 使用

### 编译

本模板必须使用 **XeLaTeX** 编译，不支持 pdfLaTeX。你可以输入下面的代码来完成编译：

```bash
xelatex main.tex
```

**`structure.sty` 是核心样式文件，请勿随意改动**。确有需要时，请做好备份，并确认改动不会影响既有正文的编译，本人不对因自行修改文件造成的损失负责。

### 脚本工具

模板随附几个脚本，用于处理与写作无关的机械事务：

| 脚本              | 位置 | 用途                           | 说明                                |
| --------------- | --- | ---------------------------- | --------------------------------- |
| `commit.py`     | 模板目录 | 一键提交并推送到 GitHub（适合 Windows ） | [commit.md](./commit.md)          |
| `commit.sh`     | 模板目录 | 同上，Bash 版                    | [commit.md](./commit.md)          |
| `setup_mode.py` | 模板目录 | 切换习题的编排模式                    | [setup\_mode.md](./setup_mode.md) |
| `symbols.py`    | 工作区级 | 符号库管理（提取、回填、刷新补全、分发）           | [symbols.md](./symbols.md)        |

- **`commit.py` / `commit.sh`**——把「检查改动 → 暂存 → 提交 → 推送」四步合成一条命令，无改动时自动跳过。两个版本行为一致，Python 版在 Windows 下无需额外环境。
- **`setup_mode.py`**——按所选模式同步习题目录结构；目录不存在则创建骨架，已存在且结构一致则跳过。
- **`symbols.py`**——维护 `structure.sty` 中的自定义记号，并提供交互式面板（扫描目录结构 / 提取符号 / 回填 / 检验并删除未使用 / 引入新记号）。它不属于本模板目录，而是放在**工作区级**的位置统一管理多本笔记：改动符号库后运行 `--all --write`，即可回填、刷新 TeXStudio 补全、并分发到各笔记。

> **注意**：`symbols.py` 是唯一会同时改动 `structure.sty` 与编辑器补全文件的工具，使用前建议先提交一次改动，便于回退。

## 更新日志

参见 [ChangeLog.md](./ChangeLog.md)。
