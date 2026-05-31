# 知识库

基于 hv-analysis 横纵分析法 + llm-wiki 框架的个人知识网络。

---

## 快速入门

### 目录结构

```
<YOUR_WIKI_ROOT>/
├── TheSchema.md        ← 操作契约（LLM 每次操作前必读）
├── README.md           ← 本文件
│
├── raw/                ← 原始资料层（内容写入后神圣不可改动）
│   ├── comparisons/    ← 第一层：hv-analysis 完整报告
│   ├── notes/          ← 零散笔记
│   └── clippings/      ← 网页剪藏、引用段落
│
├── wiki/               ← LLM 全权维护的知识层
│   ├── index.md        ← 知识库导航总入口（从这里开始）
│   ├── log.md          ← 操作日志
│   ├── concepts/       ← 第二层：概念关系页
│   ├── entities/       ← 第二层：实体关系页
│   ├── sources/        ← 手动导入的资料摘要页
│   └── overview/       ← 第三层：知识地图页
│
└── _meta/
    └── taxonomy.md     ← 受控标签分类表
```

### 三层知识结构

| 层级 | 目录 | 回答什么问题 |
|------|------|------------|
| 第一层 | raw/comparisons/ | X 是什么、怎么来的（完整 hv-analysis 报告） |
| 第二层 | concepts/ + entities/ | X 和其他知识点是什么关系 |
| 第三层 | overview/ | 这一片知识长什么形状 |

---

## 主要工作流

### 1. 归档 hv-analysis 报告

在 Claude Code 中执行 hv-analysis skill 后，skill 会自动：

1. 把完整报告写入 `raw/comparisons/[研究对象]_横纵分析.md`
2. 为报告中涉及的关键概念创建或更新 `wiki/concepts/` 页
3. 为关键实体创建或更新 `wiki/entities/` 页
4. 更新 `wiki/index.md` 和 `wiki/log.md`
5. 若同领域报告达到 3 份，提示创建知识地图

### 2. 查询

直接告诉 LLM 你的问题，它会通过 `wiki/index.md` 定位相关页面后综合回答。

**典型查询：**
- 「我分析过哪些 AI 工程相关的主题？」
- 「OpenAI 在我的分析里出现过几次？」
- 「Prompt Engineering 和 Context Engineering 是什么关系？」

### 3. 创建知识地图

当同一领域积累 3 份以上报告时：

```
请为 [主题] 创建知识地图
```

### 4. 手动导入资料

把资料放入 `raw/notes/` 或 `raw/clippings/`，然后：

```
请摄入 raw/notes/xxx.md
```

### 5. 健康检查

```
请执行 lint
```

---

## 操作规范

- **报告内容神圣**：归档时不改动报告正文，只在最前面加 frontmatter
- **先提议再执行**：批量创建多个页面前，先列出计划由用户确认
- **关系是核心资产**：concepts/ 和 entities/ 页的关系图谱每次更新时认真填写
- **Index 是导航**：index.md 只放一行摘要 + 链接，不放正文
