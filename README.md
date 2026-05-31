# hv-analysis — 横纵分析法深度研究 Skill

一个用于系统性深度研究的 Claude Code / Agent Skill，基于**横纵分析法**产出 10,000–30,000 字的深度研究报告，并自动归档到个人知识库。

---

## 什么是横纵分析法

横纵分析法融合了历时-共时分析（Saussure）、纵向-横截面研究设计、商学院案例研究法和竞争战略分析。核心原则：

- **纵向**：沿时间轴还原研究对象的完整发展历程（起源、演化、决策逻辑）
- **横向**：以当前时间为切面，与同赛道竞品/同类进行全面对比
- **交汇**：将纵横线索结合，给出新的综合判断和未来推演

适用对象：产品、公司、技术概念、人物。

---

## 功能

- 联网搜索（并行子 Agent 策略），一手来源优先
- 10,000–30,000 字结构化研究报告
- WeasyPrint 生成排版精美的 PDF（中英文混排）
- **直接模式**：如已有报告文件，可跳过研究直接生成 PDF
- **自动归档**：报告 + 概念页 + 实体页一键写入个人知识库（wiki-template 框架）

---

## 安装

将本仓库克隆到你的 Claude Code skills 目录：

```bash
# Claude Code 默认 skills 路径
git clone https://github.com/<your-username>/hv-analysis ~/.claude/skills/hv-analysis

# 安装 PDF 转换依赖
uv pip install weasyprint markdown
```

---

## 使用

在 Claude Code 中：

```
/hv-analysis 帮我研究 [研究对象]
```

或直接说：

```
我想了解 [研究对象]，用横纵分析法研究一下
```

**直接模式**（已有报告文件时）：

```
这是我已经写好的报告文件：/path/to/report.md，帮我生成 PDF 并归档
```

---

## 知识库框架（wiki-template）

本 skill 内置了一套个人知识库框架，首次使用时会自动初始化：

1. 运行 skill 到第六步时，会询问你的知识库根目录路径
2. 在该路径下自动创建完整目录结构
3. 后续每次归档自动更新概念页、实体页、导航索引

**目录结构：**

```
你的知识库根目录/
├── TheSchema.md        ← 操作契约（定义所有规范）
├── README.md
├── raw/
│   ├── comparisons/    ← 第一层：hv-analysis 完整报告（神圣不可改）
│   ├── notes/
│   └── clippings/
├── wiki/
│   ├── index.md        ← 导航总入口
│   ├── log.md          ← 操作日志
│   ├── concepts/       ← 第二层：概念关系页
│   ├── entities/       ← 第二层：实体关系页
│   ├── overview/       ← 第三层：知识地图
│   └── sources/
└── _meta/
    └── taxonomy.md
```

三层知识结构：深度报告 → 关系层（概念/实体）→ 知识地图，同一领域积累 3 份报告后可生成知识地图。

**与 Obsidian 兼容**：所有文件为标准 Markdown + frontmatter，`[[WikiLink]]` 语法直接在 Obsidian 中可用。

---

## 文件结构

```
hv-analysis/
├── SKILL.md              ← Skill 主文件（AI 执行逻辑）
├── README.md             ← 本文件
├── scripts/
│   └── md_to_pdf.py      ← PDF 转换脚本（WeasyPrint）
├── references/
│   └── schema.json
└── wiki-template/        ← 知识库框架模板（首次初始化用）
    ├── TheSchema.md
    ├── README.md
    ├── _meta/taxonomy.md
    ├── raw/comparisons/
    ├── raw/notes/
    ├── raw/clippings/
    └── wiki/
        ├── index.md
        ├── log.md
        ├── concepts/
        ├── entities/
        ├── overview/
        └── sources/
```

---

## 许可

MIT
