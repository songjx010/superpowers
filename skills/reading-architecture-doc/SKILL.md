---
name: reading-architecture-doc
description: Use when the project has an architecture document under docs/ and the agent is modifying source code, adding components, debugging, or writing tests — enables indexed reading of the architecture document
---

# 按需读取项目架构文档

## Overview

项目可能存在架构探索文档。本技能帮助 Agent 在开始开发前**主动发现并按需读取**架构文档，而非盲目动手或一次性加载全部内容。

**核心原则：** 发现文档 → 扫描目录 → 按需读取。

## When to Use

**触发条件（满足任一即激活）：**
- 即将修改项目源码（非 trivial 改动）
- 新增组件、模块、类、函数
- 调试复杂问题（数据流、线程安全、状态管理）
- 为项目编写测试
- 评估架构影响（重构、耦合分析）

**不适用：**
- README 级别的快速查看
- 仅修改配置文件（不涉及代码逻辑）
- trivial 改动（拼写修正、注释）

## 发现架构文档

在项目根目录下，按以下优先级检查文件是否存在：

| 优先级 | 路径 | 说明 |
|--------|------|------|
| 1 | `docs/architectures/architecture.md` | long-task-explore 生成的架构报告 |
| 2 | `docs/architecture.md` 或 `docs/arch.md` | 通用架构文档 |
| 3 | `ARCHITECTURE.md` | 根目录架构文档 |

使用 Glob 工具扫描 `docs/**/*.md`，查找包含 architecture / arch 关键词的文件。

## 读取策略

**不要一次性加载整个文档。** 按以下步骤操作：

1. **先读目录结构** — 读取文档前 100 行，获取章节标题和行号
2. **提取章节索引** — 从目录/标题中构建「章节名 → 起始行」映射
3. **按任务选读** — 根据当前任务，仅读取相关章节（使用 offset 参数）
4. **交叉引用** — 复杂任务可能需要多个章节，按依赖顺序阅读

### 任务→章节关键词映射

根据任务类型，优先搜索包含以下关键词的章节：

| 开发任务 | 搜索关键词 |
|----------|-----------|
| 新增/修改组件 | architecture, overview, modules, 模块, 总览 |
| 修改核心逻辑 | data flow, 状态, 流程, 算法 |
| 新增实体/类 | domain model, 实体, 领域, ER, class diagram |
| 添加外部集成 | dependencies, integration, 依赖, 集成, 外部 |
| 编写测试 | test, coverage, 健康, 健康度, 复杂度 |
| 调试问题 | data flow, 状态管理, 线程, queue, 异步 |
| 修改配置 | configuration, 配置, 编译时, 运行时 |
| 任何代码修改 | overview, 总览, 架构（始终先读） |

## 关键规则

1. **先发现再动手** — 修改源码前，先检查是否有架构文档
2. **按需读取** — 用 offset 只读相关章节，不要加载全文
3. **尊重文档结构** — 按文档自身的章节组织来读取，不要跳读
4. **读后引用** — 修改代码时引用文档中的具体位置（文件:行号）作为依据
