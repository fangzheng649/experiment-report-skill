# LaTeX 模板参考

本文件包含 experiment-report skill 所需的全部 LaTeX 模板。SKILL.md 在生成 tex 文件时按需引用本文件内容。

## 1. Preamble

根据编号方式选择对应的 `\setcounter{secnumdepth}` 行：

```latex
\documentclass[a4paper,12pt]{ctexart}
\usepackage[top=2.5cm, bottom=2.5cm, left=3cm, right=3cm]{geometry}
\usepackage{amsmath,amssymb}
\usepackage{graphicx}
\usepackage{xcolor}
\usepackage{booktabs}
\usepackage{longtable}
\usepackage{array}
\usepackage{tabularx}
\usepackage{listings}
\usepackage{etoolbox}
\usepackage{enumitem}
\usepackage{pdfpages}
\usepackage{float}
\usepackage{hyperref}
\hypersetup{colorlinks=true, linkcolor=black, urlcolor=blue}
%% 编号方式（三选一，根据用户选择）：
%% 选项A（手动中文编号）：\setcounter{secnumdepth}{-1}
%% 选项B（阿拉伯数字自动编号）：\setcounter{secnumdepth}{3}
%% 选项C（不编号）：\setcounter{secnumdepth}{-1}\setcounter{tocdepth}{-1}
\setcounter{tocdepth}{3}
\graphicspath{{image/}}

% 代码样式
\lstset{
  basicstyle=\small\ttfamily,
  breaklines=true,
  columns=fullflexible,
  frame=single,
  xleftmargin=1em,
  xrightmargin=1em,
  backgroundcolor=\color{gray!5},
  keywordstyle=\color{blue!70!black},
  commentstyle=\color{green!50!black},
  stringstyle=\color{red!70!black},
  showstringspaces=false,
  extendedchars=true,
  tabsize=4,
  escapeinside={(*@}{@*)},
}

% 禁止代码块跨页
\BeforeBeginEnvironment{lstlisting}{\vspace{0.5em}\nopagebreak[4]}
\AfterEndEnvironment{lstlisting}{\nopagebreak[4]\vspace{0.5em}}

% 截图占位符命令
\newcommand{\screenshot}[1]{%
  \begin{center}
  \fbox{\parbox{0.85\textwidth}{\centering\vspace{2.5cm}【此处需插入#1】\vspace{2.5cm}}}
  \end{center}
}
```

## 2. 封面

### 方案A：外部PDF封面

```latex
\includepdf[pages=1]{封面.pdf}
```

### 方案B：LaTeX封面（字段动态生成）

```latex
\begin{titlepage}
\centering
\vspace*{1.5cm}
{\color{gray}\zihao{5}《课程名》课程实验报告}
\vspace{4.5cm}
{\heiti\zihao{2} 课程名}\\[0.5cm]
{\heiti\zihao{3} 实验报告}
\vspace{4cm}
\begin{tabular}{rl}
  实验项目： & 实验名称 \\
\end{tabular}
\vspace{0.8cm}
%% 以下字段根据用户选择动态生成，每行格式：
%% 字段名： & 值
%% 两列排版，左右各一组，用 @{\hspace{0.5em}} 和 @{\qquad} 分隔
\begin{tabular}{r@{\hspace{0.5em}}l@{\qquad}r@{\hspace{0.5em}}l}
  ...动态字段...
\end{tabular}
\vfill
{\heiti\zihao{4} 学校名称}
\end{titlepage}
```

## 3. 目录

```latex
\newpage
\tableofcontents
\newpage
```

## 4. 章节类型与写作策略

每个章节由阶段1标注类型标签，SKILL.md 根据标签匹配写作策略：

| 类型标签 | 推荐元素 | 典型章节名 |
|---------|---------|-----------|
| `theory` | 数学公式（`\[...\]`）、理论阐述、`\subsection` 分点 | 实验原理、理论基础 |
| `procedure` | lstlisting代码块、`\textbf{代码解读：}`、`\screenshot{...}` 占位符 | 实验步骤、实验方法 |
| `results` | booktabs/tabular表格、figure图片、数据分析文字 | 实验内容、实验结果 |
| `discussion` | itemize/enumerate列表、`\textbf{}` 小标题分段 | 实验总结、讨论与思考 |
| `general` | `\noindent (1)...` 编号段落、纯文字 | 实验目的、实验要求 |

默认五段式映射：目的→general、原理→theory、步骤→procedure、内容→results、总结→discussion。

自定义章节自动推断规则：
- 含"原理/理论/基础/机制"→ theory
- 含"步骤/方法/过程/流程"→ procedure
- 含"结果/内容/分析/数据"→ results
- 含"总结/讨论/思考/收获"→ discussion
- 其他 → general

## 5. 格式规则

### 表格

- **参数表**（多行配置项）：`\footnotesize` + `\setlength{\tabcolsep}{4pt}` + `\renewcommand{\arraystretch}{1.15}` + `\hline` 行分隔 + `tabularx` 自适应宽度
- **数据对比表**（少量行）：`booktabs`（`\toprule/\midrule/\bottomrule`）+ `tabular`
- **长表格**（超过20行）：`longtable` 避免浮动

### 代码块

- 使用 `lstlisting` 环境，指定 `language` 参数（Python/bash/JSON）
- 已通过 preamble 中的 `\BeforeBeginEnvironment` 禁止跨页
- 使用 `escapeinside={(*@}{@*)}` 在代码中插入LaTeX命令
- 代码块后跟 `\textbf{代码解读：}` 段落解释代码功能

### 图片

```latex
\begin{figure}[htbp]
\centering
\includegraphics[width=0.9\textwidth]{image/filename.png}
\caption{图片描述}
\end{figure}
```

- 正文中必须有引用文字（如"如图X所示"、"见图X"）
- 若浮动位置不对，改用 `[H]`（需 float 包，已在 preamble 中）

### 文字格式

- 粗体强调：`\textbf{}`
- 行内代码：`\texttt{}`，注意转义特殊字符（`_` → `\_`，`%` → `\%`）
- 编号段落：`\noindent (1)...(2)...`

## 6. 编译命令

```bash
# 两遍编译
xelatex -interaction=nonstopmode report.tex
xelatex -interaction=nonstopmode report.tex
```

编译前检测PDF锁定：
```bash
# 尝试删除旧PDF，失败则提醒用户关闭阅读器
rm -f report.pdf
```
