# catagent-skills — 官方种子技能集

catAgent 的技能包集合仓库。每个一级目录一个技能（`SKILL.md` + 可选附件）。

## 安装

在 catAgent 任意前端（CLI / 聊天窗口 Skills 面板）：

```
/skill install github:<owner>/catagent-skills
```

或聊天窗口 → Skills 标签 → 输入框粘贴 `github:<owner>/catagent-skills` → Install。
集合仓库的全部技能会一次装齐；更新用 `Update`（按来源幂等重装）。

## 技能清单

| 技能 | 用途 |
|---|---|
| **grill-me** | 完成后无情拷问自己的 diff——最挑剔 reviewer 视角，逐条 🔴🟡🟢 |
| **commit** | 分析 staged 改动生成 conventional commit（中文描述、建议拆分、拒绝 secrets） |
| **explain** | 白话讲解代码——外行/入门/细节三层，比喻贴切，术语必解释 |
| **daily-report** | 从 git 历史生成日报——同类合并、量化规模、推断标注 |

## 写一个技能

```
my-skill/
└── SKILL.md
```

```markdown
---
name: my-skill
description: 一句话说清什么时候用（模型靠这句话决定何时加载）
---

# 标题

具体指令（流程/规则/输出格式，写给模型看）
```

要点：`description` 是触发器，写"干什么+何时用"；正文是加载后注入的指令，直接、可执行、有输出格式约定。目录名=技能名。

## 发布

任意公开 git 仓库都可以是技能源（根 SKILL.md 或集合目录结构均可），catAgent 不做中央审核——GitHub 即索引。
