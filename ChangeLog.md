# 更新日志 (ChangeLog)

**版本日期**: 2026-08-25
**当前状态**: 模板初始版本，持续完善中

---

## 2026-08-25 更新概览

本日完成模板结构搭建与四轮功能测试：确立前言/后记结构与正文三级目录约定，覆盖公式、图表、定理环境、交叉引用与交换图等核心能力，并修复测试中发现的引用显示问题。

### 新增功能

* **前言与后记结构**
    * `Content/Preface/`：全书结构、更新记录、记号说明。
    * `Content/Appendix/`：术语对照表（中、英、法）、参考文献。
* **正文三级目录结构**
    * Chapter → Section → 小节 三级嵌套，每层 `index.tex` 汇总。
    * 每章含 `intro.tex`（本章导引）、`Summary/`（本章小结），每节含 `Summary/`（本节小结），章内附录 `Appendix_Name/`。
* **四轮测试内容**
    * 第一轮：文字、列表、基础公式、核心定理环境、图片、表格与四种引用方式。
    * 第二轮：公式进阶、其余定理环境、并排图、多列表格、脚注、超链接、嵌套列表、多标签引用。
    * 第三轮：范围引用、跨页长表格、复杂数学、跨章引用与页码引用。
    * 第四轮：tikz-cd 交换图（范畴论短图与同调代数长图，含五引理、蛇引理）。
* **一键提交脚本 `commit.sh`**
    * 自动完成暂存、提交、推送三步，无改动时自动跳过。
* **一键提交脚本 `commit.py`（Python 版）**
    * 与 `commit.sh` 行为一致，Windows 下可直接 `python commit.py` 运行。
* **补全同步脚本 `update_cwl.py`**
    * 从 `structure.sty` 自动提取数学符号，生成 TeXStudio 的 `custom.cwl` 补全条目，无需手动维护。

### 优化与改进

* 图、表、公式的引用名与标题名统一为中文（补充 `\crefname` 与 `\figurename`、`\tablename`）。
* cleveref 多标签引用连接词改为中文（同类型与不同类型两套 conjunction）。
* 测试正文统一使用英文标点，双引号采用 LaTeX 标准写法。
* 新增 `longtable` 宏包，支持跨页长表格。
* 新增 `.gitattributes`，固定 shell 脚本与 Python 脚本使用 LF 换行。
* PDF 书签（侧边栏）显示章节编号（`\pdfstringdefDisableCommands`）。
* 英文字体定为 TeX Gyre Termes（Times 风格衬线）；数学字体保持原样。
* 定理标题去掉编号后的句点（`separator sign`）。
* `structure.sty` 模块化整理：划分为 页面/宏包/章节标题/定理环境/引用/符号库 六大模块并补充注释。
* `README.md`：更新目录结构与模板特点，新增测试计划章节。

### 缺陷修复

* 前言 `\chapter*` 后紧跟 `\section*` 触发书签锚点警告：加 `\phantomsection` 消除。
* thebibliography 游离标题：重定义 `\bibname`，并入目录显示。

---

## 2026-08-24 更新概览

首次发布，完成模板核心骨架。

### 新增功能

* **文档骨架**
    * 主文件 `main.tex`、核心样式包 `structure.sty`、交换图包 `quiver.sty`。
* **定理环境体系**
    * 基于 tcolorbox 的 20 种定理、证明、示例环境，按类别配色、共用章节编号、可跨页断行。
* **数学符号库**
    * 预置代数、几何、分析三大类常用记号。
* **交换图支持**
    * 封装 tikz-cd，支持 q.uiver.app 导出的交换图。
* **内容分层示例**
    * 按部分（part）分目录的 Content/ 结构与示例插图 Figures/。
