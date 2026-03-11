# MRI Physics 📖

磁共振成像物理学基础 — 一本基于 [Quarto Book](https://quarto.org/docs/books/) 的开源文档。

## 概览

本项目以简洁、直观的方式介绍 MRI 的核心物理原理，采用 Tufte 风格排版，支持边栏注释、LaTeX 数学公式和交叉引用。

## 快速开始

### 环境要求

- [Quarto](https://quarto.org/docs/get-started/) ≥ 1.4
- [Typst](https://typst.app/) (PDF 输出)

### 本地预览

```bash
quarto preview
```

### 构建

```bash
# HTML (GitHub Pages)
quarto render --to html

# PDF (Typst)
quarto render --to pdf
```

## 项目结构

```
mri-physics/
├── _quarto.yml        # Quarto 配置文件
├── custom.scss         # Tufte 风格自定义样式
├── index.qmd          # 序言
├── basics.qmd         # 基础物理
├── sequences.qmd      # 脉冲序列
├── references.qmd     # 参考文献页
├── references.bib     # 文献数据库
└── README.md          # 本文件
```

## 贡献

欢迎通过 Issue 和 Pull Request 参与贡献。

## 许可证

本项目采用 [CC BY-SA 4.0](https://creativecommons.org/licenses/by-sa/4.0/) 许可证。
