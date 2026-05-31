# 知识库 — TheSchema

> 本文件是知识库的核心契约，定义目录结构、Frontmatter 规范和操作工作流。
> **LLM 每次操作前必须读此文件。**

---

## 一、系统概述

本知识库以 **hv-analysis（横纵分析法）** 为主要输入，将深度研究报告沉淀为可串联的个人知识网络。

**核心设计原则：**

| 原则 | 说明 |
|------|------|
| 报告是一等公民 | hv-analysis 报告本身就是已处理的知识，不需要二次提炼 |
| 三层知识结构 | 深度报告 → 关系层 → 知识地图，从点到线到面 |
| 关系优先 | 知识的价值在于连接，而不只是存储 |
| 懒写 index，勤写 related | 每页的 related 字段和 appears_in 字段是知识图谱的边 |

---

## 二、三层知识结构

```
第一层：raw/comparisons/   深度报告层
                           每份 hv-analysis 报告在这里，回答「X 是什么、怎么来的」
                           报告内容神圣，写入后不得修改正文

第二层：wiki/concepts/     关系层
        wiki/entities/     每个跨报告出现的概念/实体有一个聚焦页，
                           回答「X 和其他知识点是什么关系」

第三层：wiki/overview/     知识地图层
                           把多份报告串联成一个领域的全局视图，
                           回答「这一片知识长什么形状」
```

**三层创建逻辑：**

- **第一层**：每次 hv-analysis 归档后自动创建
- **第二层**：每次归档后，对报告中出现的关键实体和概念，检查是否已有页面：
  - 已有 → 在该页追加 `appears_in` 条目
  - 没有 → 创建新页面（含基本信息和第一条 appears_in）
- **第三层**：由用户主动触发，或当同一领域积累 3 份以上报告时，由 LLM 建议创建

---

## 三、目录结构

```
<YOUR_WIKI_ROOT>/
├── TheSchema.md             # 本文件（操作契约）
├── README.md                # 快速入门
│
├── raw/                     # 原始资料层（内容写入后神圣不可改动）
│   ├── comparisons/         # 第一层：hv-analysis 完整报告
│   ├── notes/               # 手动投入的零散笔记
│   └── clippings/           # 网页剪藏、引用段落
│
├── wiki/                    # LLM 全权维护的知识层
│   ├── index.md             # 知识库导航总入口
│   ├── log.md               # 操作日志
│   ├── concepts/            # 第二层：概念关系页
│   ├── entities/            # 第二层：实体关系页（人/公司/产品/模型）
│   ├── sources/             # 手动 Ingest 的资料摘要页
│   └── overview/            # 第三层：知识地图页
│
└── _meta/
    └── taxonomy.md          # 受控标签分类表
```

---

## 四、Frontmatter 规范

### 4.1 hv-analysis 报告页（raw/comparisons/）

```yaml
---
type: comparison
title: "[研究对象] 横纵分析"
summary: "[从报告「一句话定义」部分提炼，30字以内，独立可读]"
subject: "[研究对象名称，如 Harness Engineering]"
subject_type: 技术概念 | 产品 | 公司 | 人物
domain: "[所属领域，如 AI工程 / 软件工程]"
author: llm | human | hybrid
tags: [wiki/comparison, hv-analysis, subject-type/concept, domain/ai-engineering]
entities_mentioned: ["OpenAI", "Anthropic", "Ryan Lopopolo"]
concepts_mentioned: ["Prompt Engineering", "Context Engineering", "MCP"]
sources: []
created: YYYY-MM-DD
updated: YYYY-MM-DD
related: []
---
```

**字段说明：**
- `subject`：研究主体名称，用于跨报告检索
- `subject_type`：研究对象类型，影响 tag 选取（见 taxonomy）
- `domain`：来自报告头部的「所属领域」字段
- `entities_mentioned`：报告中涉及的关键实体（公司/人/产品），用于自动创建或更新 entities/ 页
- `concepts_mentioned`：报告中涉及的关键概念，用于自动创建或更新 concepts/ 页

---

### 4.2 概念关系页（concepts/）

```yaml
---
type: concept
title: "概念名称"
summary: "一句话定义，30字以内"
author: llm | hybrid
tags: [wiki/concept, subject-type/concept]
appears_in: ["[[raw/comparisons/机器学习_横纵分析]]", "[[raw/comparisons/深度学习_横纵分析]]"]
created: YYYY-MM-DD
updated: YYYY-MM-DD
related: []
---
```

**概念页正文结构（关系优先）：**

```markdown
# 概念名称

## 定义
[一句话到一段话的定义]

## 关系图谱
- **包含**：[[子概念A]]、[[子概念B]]
- **属于**：[[父概念]]
- **演化自**：[[前身概念]] — [简短说明为什么]
- **催生了**：[[衍生概念]] — [简短说明如何催生]
- **竞争/对比**：[[竞争概念]] — [核心差异是什么]
- **互补/依赖**：[[协作概念]] — [如何配合]

## 出现在哪些分析里
| 报告 | 在该报告中的角色 |
|------|----------------|
| [[raw/comparisons/xxx_横纵分析]] | 作为主体分析 |
| [[raw/comparisons/yyy_横纵分析]] | 作为背景概念出现 |

## 跨报告的核心洞察
[只有看完多份报告才能总结的观点，单页看不到的东西]
```

> **关系图谱说明**：不需要所有关系类型都填，只填实际存在且有意义的。关系类型可扩展。

---

### 4.3 实体关系页（entities/）

```yaml
---
type: entity
title: "实体名称"
summary: "一句话说明这个实体是什么"
entity_type: person | company | product | model | institution
author: llm | hybrid
tags: [wiki/entity, entity/company]
appears_in: ["[[raw/comparisons/xxx_横纵分析]]"]
created: YYYY-MM-DD
updated: YYYY-MM-DD
related: []
---
```

**实体页正文结构：**

```markdown
# 实体名称

**类型：** 公司 / 产品 / 人物 / 机构 / 模型

## 简介
[来自各分析报告的综合描述]

## 在各分析中的角色
| 报告 | 该实体在其中的角色 |
|------|-----------------|
| [[raw/comparisons/xxx_横纵分析]] | 主体 / 竞品 / 背景机构 |

## 相关概念与实体
[[关联概念1]]、[[关联实体1]]

## 跨报告观察
[只有积累多份报告后才能看到的规律]
```

---

### 4.4 知识地图页（overview/）

```yaml
---
type: overview
title: "[主题] 知识地图"
summary: "一句话说明这张地图的范围和价值"
author: llm | hybrid
tags: [wiki/overview, domain/ai-engineering]
covers: ["[[raw/comparisons/xxx_横纵分析]]", "[[concepts/xxx]]"]
created: YYYY-MM-DD
updated: YYYY-MM-DD
related: []
---
```

**知识地图页正文结构：**

```markdown
# [主题] 知识地图

## 这片领域的全局形状
[一到两段话：这片知识的边界是什么，核心矛盾是什么，为什么值得系统理解]

## 知识节点与关系

### 演化关系（A 催生了 B）
- [[概念A]] → [[概念B]] — [原因/机制]

### 层级关系（A 包含 B）
- [[概念A]] ⊃ [[概念B]] — [B 是 A 的什么子集]

### 竞争关系（A vs B）
- [[概念A]] vs [[概念B]] — [核心分歧点]

### 互补关系（A + B）
- [[概念A]] + [[概念B]] — [组合使用的价值]

## 各节点分析报告
| 节点 | 报告 | 核心结论 |
|------|------|---------|
| [节点名] | [[raw/comparisons/xxx_横纵分析]] | [一句话] |

## 这片知识的核心争议
[跨报告才能看到的矛盾]

## 开放问题
[还没有报告覆盖、但属于这个领域的重要问题]
```

---

### 4.5 资料摘要页（sources/）

```yaml
---
type: source
title: "资料标题"
summary: "一句话说明这份资料的核心观点"
author: llm | human | hybrid
tags: [wiki/source, source-type/article]
sources: ["原始 URL 或文件路径"]
created: YYYY-MM-DD
updated: YYYY-MM-DD
related: []
---
```

---

## 五、核心操作工作流

### 5.1 hv-analysis 归档（主要工作流，自动触发）

**触发条件**：hv-analysis skill 执行完毕，或用户手动提供报告文件。

**执行步骤：**

1. **读取报告**，提取以下信息用于 frontmatter：
   - 研究对象名称（`subject`）
   - 研究对象类型（`subject_type`，来自报告头部「研究对象类型」字段）
   - 所属领域（`domain`，来自报告头部「所属领域」字段）
   - 一句话定义（`summary`，来自报告「一、一句话定义」部分）
   - 关键实体列表（`entities_mentioned`）
   - 关键概念列表（`concepts_mentioned`）

2. **写入报告**：将带 frontmatter 的完整报告写入：
   `raw/comparisons/[研究对象]_横纵分析.md`

3. **处理第二层——概念页**：对 `concepts_mentioned` 中每个概念：
   - 若 `wiki/concepts/[概念名].md` 已存在：在其 `appears_in` 列表和「出现在哪些分析里」表格里追加本报告
   - 若不存在：创建新概念页

4. **处理第二层——实体页**：对 `entities_mentioned` 中每个实体：
   - 若 `wiki/entities/[实体名].md` 已存在：追加
   - 若不存在：创建新实体页

5. **更新 index.md**：在对应分类节下追加新报告条目

6. **追加 log.md**：
   ```
   ## [YYYY-MM-DD] hv-analysis | [研究对象] → raw/comparisons/[研究对象]_横纵分析.md
   新建概念页：[列表] | 新建实体页：[列表] | 更新已有页：[列表]
   ```

7. **检查是否建议创建知识地图**：若同 domain 下报告达到 3 份，提示用户。

---

### 5.2 知识地图创建（Synthesize）

**触发方式**：用户说「请为 [主题] 创建知识地图」，或接受 5.1 的自动建议。

1. 读取 `wiki/index.md`，找出与主题相关的所有报告和概念页
2. 在 `wiki/overview/[主题]_知识地图.md` 创建知识地图页
3. 在相关报告和概念页的 `related` 字段中加入该知识地图的链接
4. 更新 `wiki/index.md` 和 `wiki/log.md`

---

### 5.3 Query（查询）

通过 `wiki/index.md` 和各页 frontmatter 的 `summary`、`entities_mentioned`、`concepts_mentioned` 字段定位相关页面，综合回答。

---

### 5.4 手动 Ingest（导入外部资料）

当用户说「请摄入 raw/xxx」时：

1. 读取原始资料，提炼核心要点
2. 在 `wiki/sources/` 创建摘要页
3. 检查摘要中出现的概念/实体，更新对应的 concepts/ 或 entities/ 页
4. 更新 index.md 和 log.md

---

### 5.5 Lint（健康检查）

当用户说「请执行 lint」时：

1. 扫描所有 wiki 页面，找出缺少必填字段、死链、孤立页面
2. 生成建议清单，**不自动大改**，经用户确认后执行
3. 追加 log.md

---

## 六、页面操作原则

1. **报告内容神圣** — 归档时不改动报告正文，只在最前面加 frontmatter
2. **关系是核心资产** — concepts/ 和 entities/ 页的关系图谱是最有价值的部分
3. **Index 是导航，不是内容** — index.md 只放一行摘要 + 链接，不放正文
4. **Log 是审计轨迹** — 每次操作后追加，不修改历史记录
5. **先提议再执行** — 批量创建多个页面前，先列出计划由用户确认

---

## 七、标签体系

参见 `_meta/taxonomy.md`

| 前缀 | 作用 | 示例 |
|------|------|------|
| `wiki/` | 页面类型（必选） | `wiki/comparison` |
| `subject-type/` | 研究对象类型 | `subject-type/concept` |
| `domain/` | 研究领域 | `domain/ai-engineering` |
| `entity/` | 实体类型（entities/ 页必选） | `entity/company` |
| `source-type/` | 原始资料类型（sources/ 页用） | `source-type/article` |
| `status/` | 页面状态 | `status/draft` |
| `hv-analysis` | 由 hv-analysis skill 生成（直接用，无前缀） | — |

---

## 八、index.md 结构规范

```markdown
# 知识库目录

## 按领域浏览（raw/comparisons/）
### [领域名]
| 报告 | subject_type | 一句话摘要 | 日期 |
|------|-------------|-----------|------|

## 概念网络（concepts/）
| 概念 | 关系数 | 一句话定义 |
|------|--------|-----------|

## 实体图谱（entities/）
| 实体 | 类型 | 出现次数 | 一句话说明 |
|------|------|---------|-----------|

## 知识地图（overview/）
| 地图 | 覆盖领域 | 节点数 |
|------|---------|--------|

## 资料摘要（sources/）
| 资料 | 一句话摘要 |
|------|-----------|

## 统计
| 类型 | 数量 |
|------|------|
```

---

## 九、执行质量标准

### 9.1 提取标准：`concepts_mentioned` 和 `entities_mentioned`

**`concepts_mentioned` 选取规则：**

| 纳入 | 排除 |
|------|------|
| 横向分析中被**直接对比**的概念 | 只在信息来源里出现的概念名词 |
| 纵向分析中推动演化的**关键前驱概念** | 通用技术词汇（如「软件工程」「数据库」） |
| 横纵交汇里被拿来类比或对照的概念 | 一笔带过、没有任何分析展开的概念 |

数量上限：5-10 个。

**`entities_mentioned` 选取规则：**

| 纳入 | 排除 |
|------|------|
| 在纵向叙事中**扮演实质角色**的公司、人物 | 只作为引用来源出现的机构 |
| 横向分析中被**展开分析**的竞品/参照产品 | 横向对比表格里一行带过的条目 |
| 在横纵交汇洞察里被直接提及的实体 | 背景介绍里顺带提到的历史人物 |

数量上限：10-15 个。

---

### 9.2 关系图谱写法

**核心原则：每条关系必须回答「为什么」，不能只写关系名称。**

❌ 差的写法：
```
演化自：[[Prompt Engineering]]
```

✅ 好的写法：
```
演化自：[[Prompt Engineering]] — Prompt Engineering 只优化单次交互的措辞，
无法保证 Agent 跨会话的稳定性，这个结构性缺口直接催生了更高层的 Harness 概念
```

| 关系类型 | 来源 | 写法要求 |
|----------|------|----------|
| 演化自 / 催生了 | 纵向分析的阶段演化和决策节点 | 说清是什么**机制或缺口**导致了演化 |
| 属于 / 包含 | 横向分析的生态位描述 | 说清**边界在哪里** |
| 竞争 / 对比 | 横向分析的直接比较段落 | 说清**核心分歧点** |
| 互补 / 依赖 | 横向分析的生态位分析 | 说清**互补的具体场景** |

**信息不足时**：不猜、不编。写 `「暂缺——当前报告信息不足以判断」`。

---

### 9.3 渐进丰富规则

**阶段一：首次创建（仅 1 份报告提及）**
- 填写：definition（一句话）、appears_in（第一条）、1-2 条最明显的关系（附 WHY 说明）
- 不写：跨报告观察
- 状态标记：`status/draft`

**阶段二：更新（2 份报告提及）**
- 追加：新的 appears_in 条目；补充关系图谱
- 可写：跨报告观察草稿，必须标注「基于 N 份报告，结论待验证」
- 状态标记：`status/draft`

**阶段三：深化（3 份及以上报告提及）**
- 填写完整关系图谱
- 写完整的跨报告观察
- 状态标记：`status/complete`

---

### 9.4 跨报告观察写法

**写之前先问：** 这个观察，只看一份报告能得出吗？如果能，它不是跨报告观察。

✅ 合格示例：
> 在三份关于 AI 工程方法论的报告中，Prompt Engineering、Context Engineering、Harness Engineering 对「工程师角色」的定义出现了明显漂移：从「写好问题的人」→「管理信息流的人」→「设计执行环境的人」。

❌ 不合格：
> 根据多份报告可以看出，Harness Engineering 是一个重要的概念。

**禁止事项：**
- 不写「首先…其次…最后」「综上所述」「不难发现」「本质上」
- 不说「某个概念」「某个工具」，要写具体名字
- 信息不足时写「暂缺」，绝不编造

---

### 9.5 知识地图创建标准

知识地图不是把多份报告摘要拼在一起，而是要回答：**把这些分析放在一起看，能得出什么单看一篇看不到的结论？**

对照 skill 横纵交汇的 5 个核心问题来写：

1. **全局形状**：这片知识的边界在哪里？核心矛盾是什么？
2. **演化路径**：各节点之间的历史演化顺序和因果逻辑
3. **当前格局**：各节点现在的竞争/互补关系
4. **核心争议**：跨报告看到的观点分歧
5. **未来剧本**：至少给出最可能、最危险、最乐观三个剧本

**触发条件复核**：只有同一 domain 下有 3 份以上报告时才创建。

---

*基于 hv-analysis 横纵分析法工作流设计 | 替换 `<YOUR_WIKI_ROOT>` 为你的实际知识库根目录*
