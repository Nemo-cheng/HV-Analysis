# 标签分类表（Taxonomy）

> 本文件定义知识库的受控标签体系。所有 wiki 页面的 tags 字段必须从此表选取。
> 标签可组合使用，但每个标签必须属于下列前缀之一。

---

## wiki/ — 页面类型（必选）

| 标签 | 用于 | 说明 |
|------|------|------|
| `wiki/comparison` | comparisons/ | hv-analysis 深度报告页 |
| `wiki/concept` | concepts/ | 概念关系页 |
| `wiki/entity` | entities/ | 实体关系页 |
| `wiki/overview` | overview/ | 知识地图页 |
| `wiki/source` | sources/ | 手动导入的资料摘要页 |
| `wiki/index` | index.md | 导航入口（仅 index.md 使用） |

---

## subject-type/ — 研究对象类型

| 标签 | 说明 | 示例 |
|------|------|------|
| `subject-type/concept` | 技术范式、方法论、抽象概念 | Harness Engineering、Prompt Engineering |
| `subject-type/product` | 产品、工具、平台 | Claude Code、LangChain |
| `subject-type/company` | 公司、组织 | Anthropic、OpenAI |
| `subject-type/person` | 人物 | Martin Fowler、Sam Altman |

---

## domain/ — 研究领域

| 标签 | 说明 |
|------|------|
| `domain/ai-engineering` | AI 工程、智能体基础设施 |
| `domain/software-engineering` | 软件工程、开发实践 |
| `domain/llm` | 大语言模型、NLP |
| `domain/product` | 产品设计、商业模式 |
| `domain/economics` | 经济学、金融 |
| `domain/science` | 自然科学、数学 |

> 领域标签可扩展。当出现新领域时，在此表追加并保持一致。

---

## entity/ — 实体类型（entities/ 页必选）

| 标签 | 说明 |
|------|------|
| `entity/person` | 人物 |
| `entity/company` | 公司、机构 |
| `entity/product` | 产品、工具 |
| `entity/model` | AI 模型 |
| `entity/institution` | 学术机构、研究机构 |

---

## source-type/ — 原始资料类型（sources/ 页用）

| 标签 | 说明 |
|------|------|
| `source-type/article` | 博客文章、媒体报道 |
| `source-type/paper` | 学术论文 |
| `source-type/book` | 书籍、章节 |
| `source-type/video` | 视频、播客笔记 |
| `source-type/clipping` | 网页剪藏 |
| `source-type/note` | 个人零散笔记 |

---

## status/ — 页面状态

| 标签 | 说明 | 触发条件 |
|------|------|---------|
| `status/draft` | 草稿，信息来源少于 3 份 | 默认初始状态 |
| `status/complete` | 完整，信息来源 3 份及以上 | 手动升级 |
| `status/needs-update` | 需要更新 | 有新报告但尚未更新时标记 |

---

## 特殊标签（无前缀）

| 标签 | 说明 |
|------|------|
| `hv-analysis` | 由 hv-analysis skill 生成的报告，直接使用，无前缀 |
