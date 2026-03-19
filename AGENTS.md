# AGENTS.md — MRI Physics 项目 AI Agent 指南

## 项目概述

这是一个基于 **Quarto Book** 的 MRI 物理学中文文档项目。输出 HTML（GitHub Pages）和 PDF（XeLaTeX）。采用 Tufte 风格排版，支持边栏注释、LaTeX 数学公式和交叉引用。

- **在线地址**：https://wyd1216.github.io/mri-physics/
- **仓库地址**：https://github.com/wyd1216/mri-physics

## 目录结构

```
mri-physics/
├── _quarto.yml                          # 唯一项目配置（章节注册、格式、字体）
├── index.qmd                            # 序言/首页（Quarto 强制要求在根目录）
├── references.qmd                       # 参考文献页
├── references.bib                       # BibTeX 文献库
├── README.md
├── AGENTS.md                            # 本文件
├── .gitignore
├── .github/workflows/publish.yml        # GitHub Actions → GitHub Pages
│
├── chapters/                            # 所有章节，按 NN-name/ 编号
│   ├── 01-hydrogen-proton/
│   │   ├── hydrogen-proton.qmd
│   │   └── images/                      # 该章节专属图片
│   ├── 02-basics/                       # 一个主题目录可包含多个 .qmd
│   │   ├── magnetic-moment.qmd
│   │   ├── gyromagnetic-ratio.qmd
│   │   ├── precession.qmd
│   │   ├── relaxation.qmd
│   │   ├── bloch-equation.qmd
│   │   └── images/
│   └── 03-sequences/
│       ├── sequences.qmd
│       └── images/
│
├── _assets/                             # 构建配置资源
│   ├── custom.scss                      # Tufte 风格 HTML 样式
│   └── ieee.csl                         # IEEE 引用格式
│
├── assets/                              # 全局共享资源（logo、封面等）
│   └── images/
│
├── _book/                               # 构建输出（已 gitignore）
└── .quarto/                             # Quarto 缓存（已 gitignore）
```

## 新增内容的工作流程

### 1. 判断归属

收到新内容时，按以下逻辑判断：

- **属于现有章节** → 在对应 `.qmd` 中合适位置插入，保持逻辑连贯
- **需要新建章节** → 创建新目录 + `.qmd`，可能需要重编号现有章节
- **某主题目录下新增文件** → 一个目录（如 `02-basics/`）可包含多个 `.qmd`，每个文件作为独立章节在 `_quarto.yml` 中注册

### 2. 内容审核与优化

在融合新内容之前，必须先进行审核和优化：

- **事实性检查**：核实物理常数、公式、单位是否正确（如旋磁比数值、弛豫方程形式等）
- **术语一致性**：确保中英文术语与项目已有内容保持统一（如"弛豫"而非"松弛"、"射频脉冲"而非"RF 脉冲"单独使用）
- **结构优化**：如果原始内容的层级或逻辑顺序不够清晰，应重组段落、调整标题层级
- **内容去重**：检查新内容是否与已有章节存在重复，重复部分应合并或通过交叉引用关联，而非重复书写
- **补充完善**：对于关键概念缺少解释或数值举例的地方，适当补充以增强可读性

如果进行了优化，在 Git 提交信息中简要说明改动点。

### 3. 新建章节的步骤

```bash
# 1. 创建目录（编号在合适位置插入，后续章节需要重命名）
mkdir -p chapters/NN-chapter-name/images

# 2. 如果需要，重命名后续章节目录保持编号连续
mv chapters/03-sequences chapters/04-sequences  # 示例

# 3. 创建 .qmd 文件（见下方格式规范）

# 4. 更新 _quarto.yml 中 chapters 列表（顺序即为书中顺序）

# 5. 验证构建
quarto render --to html
quarto render --to pdf
```

### 4. 更新 `_quarto.yml`

章节注册在 `book.chapters` 下，当前结构：

```yaml
chapters:
  - index.qmd
  - part: "导论"
    chapters:
      - chapters/01-hydrogen-proton/hydrogen-proton.qmd
  - part: "基础物理"
    chapters:
      - chapters/02-basics/magnetic-moment.qmd
      - chapters/02-basics/gyromagnetic-ratio.qmd
      - chapters/02-basics/precession.qmd
      - chapters/02-basics/relaxation.qmd
      - chapters/02-basics/bloch-equation.qmd
  - part: "脉冲序列"
    chapters:
      - chapters/03-sequences/sequences.qmd
  - references.qmd
```

- `index.qmd` 必须在第一位且位于项目根目录
- `references.qmd` 必须在最后
- 可以用 `part:` 对章节分组

## 内容格式规范

### 数学公式

- 行内：`$\mathbf{B}_0$`、`$T_1$`
- 块级公式必须带交叉引用 ID：

```markdown
$$
\omega_0 = \gamma B_0
$$ {#eq-larmor}
```

- 物理量用 LaTeX 排版：`$\text{H}_2\text{O}$`（化学式用 `\text{}`）
- **禁止使用 Unicode 特殊符号**（如 →、←、≈、≤ 等），必须用 LaTeX 命令替代（如 `$\to$`、`$\leftarrow$`、`$\approx$`、`$\leq$`），否则 PDF 编译会产生乱码

### 边栏注释（Tufte 风格）

```markdown
::: {.column-margin}
这段文字会显示在右侧边栏。
:::
```

### Callout 高亮

```markdown
::: {.callout-tip}
## 标题
核心概念或关键数值。
:::

::: {.callout-note}
## 标题
补充说明或规则。
:::
```

可用类型：`tip`、`note`、`warning`、`important`、`caution`

### 表格

章末小结表格应带标签：

```markdown
| 列1 | 列2 |
|:---|:---|
| 内容 | 内容 |

: 表格标题 {#tbl-label}
```

### 交叉引用

- 公式：`@eq-larmor`
- 表格：`@tbl-label`
- 章节：`@sec-label`（需在标题后加 `{#sec-label}`）
- 图片：`@fig-label`

### 参考文献

在 `references.bib` 中添加 BibTeX 条目，正文中用 `[@key]` 引用。

## 构建命令

```bash
quarto preview              # 本地实时预览（HTML）
quarto render --to html     # 构建 HTML → _book/index.html
quarto render --to pdf      # 构建 PDF  → _book/MRI-Physics.pdf
```

## PDF 中文支持

PDF 使用 XeLaTeX + xeCJK，字体配置：

| 用途 | 字体 |
|---|---|
| 正文 (mainfont) | Songti SC |
| 粗体 | Heiti SC |
| 斜体 | Kaiti SC |
| 无衬线 (sansfont) | PingFang SC |
| 等宽 (monofont) | Menlo |

边栏注释已通过 `ragged2e` 包设置为左对齐。

## Git 提交规范

```
feat: add Chapter N - 章节中文名    # 新增章节
fix: 修复描述                       # 修复问题
docs: 更新描述                      # 文档更新
ci: CI 相关变更                     # GitHub Actions
style: 样式调整                     # 排版/样式
```

## 部署

推送到 `main` 分支后，`.github/workflows/publish.yml` 会自动通过 GitHub Actions 构建并部署到 GitHub Pages。

## 注意事项

1. **`index.qmd` 必须在项目根目录** — Quarto Book 硬性要求
2. **PDF 不支持 Typst** — Book 项目中 `pdf-engine: typst` 的 callout 会报错，必须用 `xelatex`
3. **章节编号连续** — 重命名目录时确保 `chapters/` 下的编号无间断
4. **构建验证** — 每次修改后必须同时验证 `--to html` 和 `--to pdf`
5. **图片路径** — 章节内图片使用相对路径 `![](images/fig.png)`，图片放在对应章节的 `images/` 目录下
6. **标题层级** — PDF 使用 `tufte-book` 文档类，不支持 `\subsubsection`（`####`）。每个 `.qmd` 文件顶级标题为 `#`，正文最深到 `###`
7. **多文件章节** — 同一目录下的多个 `.qmd` 各自作为独立章节注册到 `_quarto.yml`，交叉引用跨文件自动生效
