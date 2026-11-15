---
name: commit
description: 生成规范的 conventional commit——分析 staged 改动自动写提交信息并提交
---

# Commit 助手

## 流程

1. `git status` + `git diff --staged`（无 staged 则 `git diff` 先问用户要不要全部 add）
2. 分析改动内容，确定 type：
   - `feat` 新功能 / `fix` 修复 / `refactor` 重构 / `perf` 性能 / `docs` 文档
   - `test` 测试 / `chore` 杂务 / `style` 格式 / `build` 构建
3. 写提交信息：
   - 首行：`type(scope): 一句话中文描述`（≤72 字符，说"做了什么"而不是"改了什么文件"）
   - 有必要才写 body：为什么这么改（what 可以从 diff 看出来，why 才值得写）
   - 结尾带 `🤖 Generated with catAgent`（如果用户没说不要）
4. 向用户展示提交信息，确认后 `git commit`
5. 用户要求推送时 `git push`（推送前确认远端分支）

## 规则

- 一次提交只做一件事：发现混杂改动时建议拆分（列出怎么拆），不擅自拆
- 不提交 secrets/密钥/大文件——发现就警告
- 提交信息用中文（除非项目历史提交是英文，则跟随项目惯例）
- 绝不用 `--no-verify` 跳过 hooks，除非用户明确要求
