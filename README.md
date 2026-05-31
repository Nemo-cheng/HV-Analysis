# HV-Analysis — 会累积的个人知识库

> 用横纵分析法深度研究一个东西，然后让这个知识永久留下来，和你已有的知识连起来。

---

## 为什么做这个

用 AI 研究一个东西的体验，通常是这样的：

你问了一个很好的问题，AI 给了一个很好的答案，然后对话结束了。下次你研究另一个东西，之前的知识就消失了——或者说，它从来没有真正「存在」过，只是被临时组装了一次。

这个项目想解决的，是让每一次深度研究的结果**真正留下来**，而且不只是存档，而是和已有的知识形成连接。

---

## 三个来源，一个系统

这个项目把三个独立的想法拼在一起：

**1. [KKKKhazix 的 hv-analysis skill](https://github.com/KKKKhazix/khazix-skills)**

横纵分析法：纵向追一个东西从诞生到现在的完整历程，横向把它和同时代的竞品/同类放在一起对比，最后交汇出新的判断。一次研究出一份 10,000–30,000 字的深度报告。

这解决了「怎么把一个东西研究透」的问题。

**2. [Karpathy 的 llm-wiki 框架](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)**

Karpathy 提出的核心洞察：大多数人用 AI 处理知识的方式是 RAG——每次查询都从原始文档重新检索、重新组装。没有积累。

llm-wiki 的做法不同：让 LLM 主动维护一套结构化的、相互链接的 wiki 页面。每次 ingest 一个新来源，LLM 不只是存档，而是更新相关页面、建立跨文档的连接、标记矛盾。知识是**复利增长**的，不是每次从零开始。

**3. Obsidian**

这套 wiki 的所有文件都是标准 Markdown + frontmatter，`[[WikiLink]]` 语法直接在 Obsidian 里可用。Obsidian 的图谱视图让知识之间的连接可视化——你真的能看到一个知识网络在慢慢长出来。

---

## 它是怎么工作的

```
一次 hv-analysis 研究
        ↓
深度报告（纵向 + 横向 + 交汇洞察）
        ↓
自动归档到三层知识结构：
  第一层 raw/comparisons/   完整报告（神圣不可改）
  第二层 wiki/concepts/     报告里出现的关键概念，每个有一个关系页
          wiki/entities/    涉及的公司/人物/产品，每个有一个关系页
  第三层 wiki/overview/     同领域积累 3 份报告后，生成知识地图
        ↓
下次研究同领域内容，概念页和实体页自动更新
——你能看到「OpenAI 在我分析过的 5 个项目里分别扮演了什么角色」
```

随着研究的积累，知识不是一堆孤立的文件，而是一张越来越密的网。

---

## 安装

直接告诉你的 Agent：

```
帮我安装这个 skill：https://github.com/Nemo-cheng/HV-Analysis
```

支持 Claude Code、Codex、OpenCode 等兼容 Agent Skills 规范的工具。

**如果 Agent 无法自动完成安装，用以下命令行方式：**

Codex（Windows PowerShell）：
```powershell
python "$env:USERPROFILE\.codex\skills\.system\skill-installer\scripts\install-skill-from-github.py" --url https://github.com/Nemo-cheng/HV-Analysis
```

Claude Code（手动 git clone）：
```bash
git clone https://github.com/Nemo-cheng/HV-Analysis ~/.claude/skills/hv-analysis
```

---

## 使用

**开始一次研究：**

```
我想了解 [研究对象]
```

**直接模式**（已有报告文件，跳过研究直接归档 + 生成 PDF）：

```
这是我已经写好的报告：/path/to/report.md，帮我生成 PDF 并归档
```

**第一次使用时**，运行到归档步骤（第六步）时，skill 会询问你的知识库根目录路径，然后自动初始化完整的目录结构。

---

## 知识库结构

```
你的知识库根目录/
├── TheSchema.md        ← 操作契约（定义所有规范，LLM 每次归档前必读）
├── raw/
│   └── comparisons/    ← 第一层：完整报告（写入后内容不得修改）
├── wiki/
│   ├── index.md        ← 导航总入口
│   ├── concepts/       ← 第二层：概念关系页
│   ├── entities/       ← 第二层：实体关系页
│   └── overview/       ← 第三层：知识地图（3 份同领域报告后触发）
└── _meta/
    └── taxonomy.md     ← 受控标签体系
```

---

## 致谢

- **横纵分析法**：[KKKKhazix/khazix-skills](https://github.com/KKKKhazix/khazix-skills)
- **llm-wiki 框架设计灵感**：[Andrej Karpathy](https://gist.github.com/karpathy/442a6bf555914893e9891c11519de94f)

---

MIT License
