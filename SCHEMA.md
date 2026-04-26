# ⚠️ 核心配置文件 — 请勿删除或随意改动
#
# 本文件是 LLM Wiki 的核心配置文件，定义了知识库的结构、规范和标签体系。
# 修改前请确保理解每个部分的作用，错误的修改可能导致知识库混乱。
#
# ================================================================
# Wiki Schema

## Domain
来回商贸业务知识库 — 安防设备安装与维修、办公设备服务、工程项目管理

## Conventions
- 文件名：中文描述，简洁明了（如 `石柱县新型文化空间建设项目.md`）
- 每个 wiki 页面以 YAML frontmatter 开头（见下方）
- 使用 `[[wikilinks]]` 链接页面（每页至少 2 个外部链接）
- 更新页面时必须更新 `updated` 日期
- 新页面必须添加到 `index.md` 相应版块
- 所有操作必须追加到 `log.md`

## Directory Structure（目录结构）

```
wiki/
├── SCHEMA.md              ← 本文件：定义结构、惯例、标签分类
├── index.md               ← 内容目录：所有页面索引（按类型分类）
├── log.md                 ← 操作日志：按时间记录所有动作
│
├── raw/                   ← Layer 1：原始资料（不可修改）
│   ├── articles/          # 网页文章、剪藏
│   ├── papers/            # PDF原始解析结果（_parsed.json）
│   ├── transcripts/       # 会议记录、访谈
│   └── assets/            # 图片、图表（被源文件引用）
│
├── entities/              ← Layer 2：通用实体（跨项目参与方）
├── concepts/              ← Layer 2：概念页（主题、制度、方法）
├── comparisons/           ← Layer 2：对比分析页（暂无内容时保持空目录）
├── queries/               ← Layer 2：查询结果页（暂无内容时保持空目录）
│
├── 客户公司/              ← 客户公司资料
│   └── <客户名>/
│       └── 基础信息.md
│
├── 报价单/                ← 报价单（按客户/日期分类）
│   └── <客户>/
│       └── <日期>_<询价内容>/
│           ├── _报价汇总.md
│           └── <供应商>_报价.md
│
├── 概念/                  ← 中文概念页（等于 concepts/）
│
├── 教程/                  ← 维修教程（视频解析入库）
│   └── <教程名>/
│       ├── <教程名>.md
│       ├── source/        ← 源视频文件
│       └── keyframes/    ← 关键帧图片
│
└── projects/              ← 项目目录（按项目隔离）
    └── <项目名>/
        ├── entities/      ← 项目实体页
        ├── source/       ← 源文件（PDF/Excel等）
        ├── parsed/       ← 解析后Markdown + _parsed.json
        └── parsed-images/← 解析出的印章/签名图片
```

> **Layer 说明：**
> - Layer 1（raw/）：原始不可修改的来源，机器生成
> - Layer 2（entities/concepts/comparisons/queries/）：Agent 可写的 wiki 页面
> - comparisons/ 和 queries/ 为标准 LLM Wiki 结构保留目录，暂无内容时保持空目录

## Frontmatter
```yaml
---
title: 页面标题
created: YYYY-MM-DD
updated: YYYY-MM-DD
type: entity | concept | comparison | query | summary
tags: [标签1, 标签2]
sources: [raw/papers/文件名.md]
---
```

## Tag Taxonomy（标签分类）
- **项目**：项目、工程、合同、招投标
- **设备**：安防、监控、门禁、复印机、打印机
- **维修**：维修、保养、故障、配件
- **客户**：移动、农业公司、来回商贸
- **文档**：合同、报告、清单、报价
- **组织**：公司、政府部门、学校
- **地点**：石柱县、重庆
- **财务**：报价、成本、付款、结算

## Page Thresholds（页面阈值）
- **创建页面**：实体/概念在 2+ 来源中出现，或在一个来源中处于核心地位
- **追加现有页面**：来源提到已覆盖的内容
- **不创建页面**：一次性提及、次要细节、或超出领域范围
- **拆分页面**：超过 ~200 行时拆分为子主题
- **归档页面**：内容完全被取代时移至 `_archive/`，从 index.md 移除

## Entity Pages（实体页）
每个重要实体一页，包括：
- 概述 / 是什么
- 关键事实和日期
- 与其他实体的关系（[[wikilinks]]）
- 来源引用

## Concept Pages（概念页）
每个概念或主题一页，包括：
- 定义 / 解释
- 当前知识状态
- 开放问题或争议
- 相关概念（[[wikilinks]]）

## Update Policy（更新策略）
新信息与现有内容冲突时：
1. 检查日期 — 新来源通常取代旧来源
2. 确有矛盾时，同时记录两种立场并注明日期和来源
3. 在 frontmatter 中标记：`contradictions: [page-name]`
4. 在 lint 报告中标记待用户审查

## Source File Management（源文件管理）
- PDF 源文件 → `projects/<项目名>/source/`
- 解析后的 Markdown → `projects/<项目名>/parsed/`
- 解析出的图片 → `projects/<项目名>/parsed-images/`
- 原始解析 JSON → `raw/papers/`
- source/、parsed/、parsed-images/ 必须物理隔离
