# Math-Note-Template-By-Yukina

一个面向数学笔记与书籍写作的 LaTeX 模板，基于 `ctexbook` 构建，内置统一的定理环境体系、数学符号库与交换图支持。

## 目录结构

```
.
├── main.tex              # 主文件：定义 \part 并汇总各章节
├── structure.sty         # 核心样式：页面布局、定理环境、符号库、引用
├── quiver.sty            # 交换图支持（q.uiver.app 导出，封装 tikz-cd）
├── commit.py             # 一键提交脚本（Python，Windows 可直接运行）
├── commit.sh             # 一键提交脚本（Bash 版，兼容）
├── update_cwl.py         # 从 structure.sty 自动生成 TeXStudio 补全（可选）
├── Content/                  # 内容目录
│   ├── Preface/              # 前言：全书结构、更新记录、记号说明
│   ├── 01_Test_Chapter/      # 正文测试章节（三级嵌套示例）
│   │   ├── intro.tex         #   本章导引
│   │   ├── 01_Test_Section/  #   节：index.tex + 小节文件
│   │   ├── Summary/          #   本章小结
│   │   └── Appendix_Test_A/  #   章内附录
│   ├── 02_Test_Chapter/      # 正文测试章节
│   └── Appendix/             # 后记：术语对照表、参考文献
└── Figures/                  # 插图目录
```

## 模板特点

### 1. 中文书籍排版

使用 `ctexbook`（10pt / A4 / oneside），几何边距左右 1.5cm、上下 2cm，适合中文数学书籍与长篇笔记。

### 2. 统一的定理环境体系

基于 `tcolorbox` 预置 22 种环境，按类别配色、可跨页断行（定理类共用章节编号，习题独立编号）：

| 类别 | 环境 |
|------|------|
| 基础陈述 | 定义（蓝）、公理（靛蓝）、假设（青） |
| 推演结论 | 定理（红）、引理（橙）、命题（紫）、推论（绿）、元定理（靛蓝）、准则（茶） |
| 补充说明 | 问题（黄）、例（绿）、注记（灰） |
| 习题 | 习题（品红，独立编号）、解答（灰，题后即答） |
| 其他 | 算法、约定、警示、证明（自动加 ∎）、回答、分析、提示、代码块 |

每个环境带独立学术配色与顶边色条，视觉统一且易于区分。

### 3. 智能引用

`hyperref` + `cleveref`：`\cref` / `\autoref` 自动带类型名（显示"定义 1.1"而非"1.1"），中英文引用名均已配置。

### 4. 交换图支持

`quiver.sty` 封装 `tikz-cd`，可直接使用 q.uiver.app 导出的交换图，支持弯曲箭头、路径缩短与多种箭头样式。

### 5. 预置数学符号库

内置代数 / 几何 / 分析三大类常用记号，如 `\N \Z \Q \R \C`、`\Hom \End \Aut`、`\coker \coim \tor`、`\GL \SL \SO`、`\closure \interior` 等，面向抽象代数、范畴论与线性代数写作。

### 6. 内容分层管理

正文采用 Chapter → Section → 小节三级嵌套结构，每层目录有 `index.tex` 汇总、逐级 `\input`。每章附带 `intro.tex`（本章导引）、`Summary/`（本章小结），每节可带 `Summary/`（本节小结）与章内附录 `Appendix_Name/`，将长文档拆分为小文件，便于维护与复用。

## 测试计划

模板内置两章测试内容（`Content/01_Test_Chapter`、`02_Test_Chapter`），按复杂度从低到高分四轮验证：

| 轮次 | 复杂度 | 测试项目 |
|------|--------|----------|
| 第一轮 | 基础 | 正文文字与列表、基础公式（equation/align/gather）、核心定理环境、单图与单表插入、四种引用（\ref / \autoref / \cref / \eqref） |
| 第二轮 | 进阶 | 公式进阶（multline/cases/矩阵/subequations/\tag）、其余定理环境、并排图、多列表格、脚注、超链接、嵌套列表、多标签引用 |
| 第三轮 | 复杂 | 范围引用、跨页长表格（longtable）、复杂数学（分式嵌套/大运算符/split）、跨章引用与页码引用 |
| 第四轮 | 交换图 | tikz-cd 交换图：范畴论短图（泛性质、自然变换）与同调代数长图（五引理、蛇引理、长正合列） |

## 更新日志

详细更新记录见 [ChangeLog.md](./ChangeLog.md)。

## 使用

用 XeLaTeX 编译 `main.tex` 即可。新增章节时，在 `Content/` 下按三级结构新建目录（Chapter → Section → 小节文件），每层建 `index.tex` 汇总，并在上一级 `\input` 引入。

提交改动：`python commit.py "提交说明"`（自动完成暂存、提交、推送）。
