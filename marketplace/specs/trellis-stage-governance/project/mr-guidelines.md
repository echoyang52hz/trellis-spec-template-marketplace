# MR / PR 规范

> 定义分支命名、MR / PR 标题与描述、推送和合并后清理规则。

## 分支规则

默认从主开发分支创建独立工作分支。

| 场景 | 命名示例 |
|------|----------|
| 新功能 | `feat/example-feature` |
| 缺陷修复 | `fix/example-bug` |
| 文档更新 | `docs/example-docs` |
| 工具或配置 | `chore/example-tooling` |
| 重构 | `refactor/example-module` |

分支分类使用 `/` 连接，不使用 `docs-example` 这类把分类和名称混在一起的写法。

## 阶段任务命名

已经纳入阶段计划且有明确序号的任务，应在任务标题、slug、分支和 MR / PR 标题中保留阶段与任务编号。

| 对象 | 格式 |
|------|------|
| Trellis 任务标题 | `阶段<阶段号>任务 <三位任务号>：<任务名>` |
| Trellis slug | `p<三位阶段号>-t<三位任务号>-<english-task-slug>` |
| 工作分支 | `<type>/p<三位阶段号>-t<三位任务号>-<english-task-slug>` |
| MR / PR 标题 | `[P<三位阶段号>-T<三位任务号>] <任务名>` |

执行要求：

- `T000` 用于阶段规划、任务拆分和阶段任务书 / 执行台账创建；
- `T001+` 用于实现、验证、修复、收束等阶段子任务；
- 非阶段计划任务不强行套用阶段编号；
- 阶段号和任务号建议使用三位数字，便于排序和搜索。

## MR / PR 描述

描述应以中文为主，帮助审阅者快速理解本次变更。

推荐结构：

```markdown
## 概述

本次完成 <一句话说明任务目标和结果>。

## 主要变更

- <主要变更 1>；
- <主要变更 2>；

## 验证

- `<检查命令或测试命令>`；

## 影响范围

- <影响到的模块、接口、文档或协作流程>；

## 关联

- Trellis 归档 PRD：<链接>；
- 阶段计划：<链接>；
- 主要提交：<链接>；
```

阶段任务的描述重点应放在本次任务产物上，不应把 archive commit、journal commit 当作主要变更展开。

## 合并后清理

MR / PR 合并后应清理已完成的工作分支：

```bash
git checkout <main-development-branch>
git pull --ff-only
git branch --delete <work-branch>
```

如平台未自动删除远端源分支，再手动删除：

```bash
git push origin --delete <work-branch>
```

