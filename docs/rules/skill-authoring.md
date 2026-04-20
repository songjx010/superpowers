# Superpowers Skill Authoring Conventions

## Skill Structure

- 每个技能一个目录，放在 `skills/` 下，kebab-case 命名
- 主文件为 `SKILL.md`，大写固定名称
- 辅助文件仅当内容超过 ~50 行或提供可复用工具时拆分

## Frontmatter (YAML)

```yaml
---
name: skill-name        # kebab-case，与目录名一致
description: Use when [触发条件和症状]  # 仅描述何时触发，不描述流程
---
```

- description 以 "Use when..." 开头
- 仅描述触发条件，**绝不能**总结技能流程
- 保持在 500 字符以内

## 核心结构段

按以下顺序组织（非每段都必须）：

1. 标题（H1，Title Case）
2. `<HARD-GATE>`（设计/brainstorming 类技能）
3. Anti-Pattern 段
4. Checklist（有序列表）
5. Process Flow（Graphviz dot 图）
6. 详细步骤说明
7. Key Principles
8. Red Flags / Rationalizations 表
9. Integration 段

## 约束

- 零依赖：不引入第三方依赖
- 技能是行为塑造代码，修改需评估证据
- 核心技能必须通用，领域特定技能放独立插件
- 禁止占位符（TBD/TODO）
- 每个步骤必须有实际内容

## 跨技能链式调用

```markdown
## Integration
**Called by:** skill-a
**Chains to:** next-skill
**Requires:** 前置条件
**Produces:** 输出产物路径
```

## 写作风格

- 直接、祈使语气，无废话
- 第二人称指令（"You MUST..."）
- Bold 强调核心原则
- ALL-CAPS 用于铁律
- 使用 "your human partner" 而非 "the user"
