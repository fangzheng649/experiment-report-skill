# experiment-report-skill

一个 [Claude Code](https://docs.anthropic.com/en/docs/claude-code) 技能，用于自动生成中文课程实验报告。基于 XeLaTeX + ctexart 编译为 PDF。

## 功能

- **全流程覆盖**：环境检查 → 内容收集 → tex 生成 → 图片插入 → 迭代修改 → PDF 输出
- **两种使用场景**：从对话上下文提取实验信息，或引导用户提供文件/数据
- **并行撰写**：多个章节由独立 agent 同时生成，提升效率
- **灵活章节结构**：默认五段式（目的/原理/步骤/内容/总结），可自由增删重排
- **智能写作策略**：根据章节类型自动匹配 LaTeX 元素（公式、代码块、表格、列表等）
- **编译容错**：自动检测并修复常见 LaTeX 语法错误
- **图片后置插入**：先写内容后补图片，支持自动扫描匹配

## 前置要求

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) CLI
- TeX 发行版（二选一）：
  - [TeX Live](https://tug.org/texlive/)（推荐）
  - [MiKTeX](https://miktex.org/)
- 需包含 `xelatex` 和 `ctexart` 文档类
- **推荐**：[VS Code](https://code.visualstudio.com/) + [LaTeX Workshop](https://marketplace.visualstudio.com/items?itemName=James-Yu.latex-workshop) 插件，用于实时预览 PDF 和编辑定位

## 安装

将 `SKILL.md` 和 `templates.md` 复制到 Claude Code 的全局技能目录：

```bash
# Linux / macOS
mkdir -p ~/.claude/skills/experiment-report
cp SKILL.md templates.md ~/.claude/skills/experiment-report/

# Windows (PowerShell)
New-Item -ItemType Directory -Force -Path "$HOME\.claude\skills\experiment-report"
Copy-Item SKILL.md, templates.md "$HOME\.claude\skills\experiment-report\"
```

安装后，在对话中提到 "实验报告" 或 "撰写报告" 即可自动触发该技能。

## 使用方法

1. 启动 Claude Code 对话
2. 说 "帮我写一份实验报告" 或 "撰写实验报告"
3. 按引导依次提供：姓名、学号、课程名、实验项目名等基本信息
4. 确认章节结构和编号方式
5. Skill 自动生成 tex 文件、编译预览
6. 提供图片后自动插入
7. 反馈修改意见，迭代至满意
8. 最终输出 PDF

## 封面建议

推荐使用学校或课程提供的封面模板（通常是 `.doc` / `.docx` 格式）：

1. 用 Word 填写好封面中的个人信息（姓名、学号、日期等）
2. 将填写好的封面另存为 PDF（如 `封面.pdf`）
3. 在阶段0中告诉 skill 你有现成的封面 PDF，skill 会自动用 `\includepdf` 将其插入报告第一页

这种方式比 skill 内置的 LaTeX 封面更贴合学校格式要求，排版效果也更好。

## 文件结构

```
experiment-report-skill/
├── SKILL.md        # 主文件：YAML 前置信息 + 6 阶段流程 + 编译规则
├── templates.md    # LaTeX 模板：preamble、封面、格式规则
└── README.md       # 本文件
```

## 输出示例

Skill 会在工作目录下生成以下文件：

```
report/
├── report.tex      # LaTeX 源文件
├── report.pdf       # 编译输出的 PDF
└── image/           # 图片目录
    ├── screenshot1.png
    └── ...
```

## 自定义

- **章节结构**：默认五段式，可增删重排，名称完全自定义
- **编号方式**：手动中文编号（默认）/ 阿拉伯数字自动编号 / 不编号
- **封面**：推荐使用 docx 模板填写后转 PDF 插入（见上方「封面建议」），也可使用 LaTeX 动态生成

## 许可证

MIT
