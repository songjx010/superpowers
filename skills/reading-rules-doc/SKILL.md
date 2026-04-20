---
name: reading-rules-doc
description: Use when docs/rules/ directory exists in the project and the agent is about to write or modify source code, create new files, refactor, or fix bugs — ensures compliance with project-specific coding rules
---

# 按需读取项目规范文档

## Overview

项目可能包含编码规范文档（`docs/rules/*.md`）。本技能确保 Agent 在开发代码前**主动发现并遵守**这些规范，而非凭默认习惯编码。

**核心原则：** 先读规范，再写代码。

## When to Use

**前置条件：** 工作目录下存在 `docs/rules/` 目录且包含 `.md` 文件。

**触发场景：**
- 编写新代码（新文件、新函数、新类）
- 修改现有源码
- 重构代码
- 修复 bug
- 编写测试

**不适用：**
- 仅阅读/搜索代码（不修改）
- 修改非代码文件（README、CI 配置等）
- trivial 改动（拼写修正）

## 发现规范文档

1. 使用 Glob 工具扫描 `docs/rules/*.md`，获取所有规范文件列表
2. 如果文件较多，按文件名判断与当前任务的相关性（见下方映射表）
3. 优先读取与当前任务直接相关的规范文件

## 任务→规范文件映射

根据任务类型和文件名关键词，选择优先读取的规范：

| 任务类型 | 优先匹配的文件名关键词 |
|----------|----------------------|
| 编写实现代码 | coding, style, naming, conventions, 编码, 命名, 规范 |
| 新增 API/接口 | api, interface, design, 接口, 设计 |
| 错误处理 | error, exception, handling, 错误, 异常 |
| 测试 | test, testing, 测试 |
| 安全相关 | security, auth, 安全 |
| 性能优化 | performance, perf, 性能 |
| 日志/监控 | logging, telemetry, observability, 日志 |
| 数据库操作 | database, sql, migration, 数据库 |
| 任何代码修改 | 全部（若文件数 ≤ 3）或 coding/style 相关 |

**如果不确定读哪些，读取所有文件。** 规范文档通常较短，宁可多读不可遗漏。

## 关键规则

1. **先读后写** — 在生成任何代码之前，先读取相关规范文件
2. **全文遵守** — 规范文件通常不长，应完整读取并严格遵守
3. **不确定就全读** — 规范文档数量少时（≤ 5 个），直接全部读取
4. **冲突处理** — 多个规范文件间有冲突时，以更具体的为准；仍不确定则询问用户
5. **读后引用** — 代码变更应体现对规范的理解，必要时在说明中引用具体规范条目
