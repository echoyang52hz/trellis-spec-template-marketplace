# 项目级规范

> 适用于 Trellis 任务、Git 协作、阶段计划、文档状态和跨角色协作的项目级约定。

## 概览

本目录存放仓库级治理规范。在创建任务、提交、推送、创建 MR / PR、归档 Trellis 任务或同步阶段进度前，应先读取这些文件。

## 规范索引

| 规范 | 说明 |
|------|------|
| [Git 工作流总入口](./git-workflow-guidelines.md) | Git / Trellis 工作流总原则、阅读顺序和最短检查清单 |
| [提交边界](./commit-boundaries.md) | work commit、archive commit、journal commit 和 docs progress commit 的边界 |
| [MR / PR 规范](./mr-guidelines.md) | 分支命名、MR / PR 标题与描述、推送和合并后清理 |
| [Trellis 任务生命周期](./trellis-task-lifecycle.md) | 任务创建边界、规划深度、启动、收尾、归档和阶段同步顺序 |
| [阶段任务书规范](./stage-planning-guidelines.md) | 阶段规划契约、T000 任务和阶段任务拆分规则 |
| [阶段执行台账规范](./stage-ledger-guidelines.md) | 滚动状态、完成记录、MR 编号同步和主要提交记录规则 |
| [工作流反例](./workflow-anti-patterns.md) | 常见流程偏差、风险解释和纠偏做法 |

## 开发前检查清单

- [ ] 在创建分支、提交、推送、更新 MR / PR 或任务状态前，先读 [Git 工作流总入口](./git-workflow-guidelines.md)。
- [ ] 如果即将 `git add` 或起草 work commit，继续读 [提交边界](./commit-boundaries.md)。
- [ ] 如果即将推送分支、创建 / 更新 MR 或合并后清理分支，继续读 [MR / PR 规范](./mr-guidelines.md)。
- [ ] 如果当前工作使用 Trellis 任务，继续读 [Trellis 任务生命周期](./trellis-task-lifecycle.md)。
- [ ] 如果工作属于阶段计划，继续读 [阶段任务书规范](./stage-planning-guidelines.md) 和 [阶段执行台账规范](./stage-ledger-guidelines.md)。
- [ ] 如果不确定某条规则为什么存在，读 [工作流反例](./workflow-anti-patterns.md)。
- [ ] 当项目级规范和 package / layer 级规范冲突时，以项目级规范为准。

