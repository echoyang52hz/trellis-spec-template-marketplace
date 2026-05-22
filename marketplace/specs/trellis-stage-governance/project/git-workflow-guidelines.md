# Git 工作流总入口

> 先读这里，再按场景进入专题文件。

## 核心原则

1. 禁止直接向生产主分支提交或推送。
2. 默认基于主开发分支创建独立工作分支；只有用户或团队规则明确允许时，才直接在现有分支上修改。
3. 合并优先通过代码托管平台的 MR / PR 完成。
4. 分支名、commit 信息、MR / PR 标题和描述不得写与项目产物无关的工具生成说明；Trellis 框架相关内容可以按事实描述。
5. Trellis 驱动的任务应把 work commit、archive commit、journal commit 和 docs progress commit 分开处理。
6. 活动任务目录属于过程文件，不应混入 work commit；可复用的 spec 产物可以随 work commit 提交。

## 阅读路径

| 场景 | 继续读取 |
|------|----------|
| 准备提交，判断哪些文件该进 work commit | [提交边界](./commit-boundaries.md) |
| 准备推分支、写 MR / PR 标题和描述、合并后清理分支 | [MR / PR 规范](./mr-guidelines.md) |
| 准备创建、启动、结束或归档 Trellis 任务 | [Trellis 任务生命周期](./trellis-task-lifecycle.md) |
| 准备规划阶段任务 | [阶段任务书规范](./stage-planning-guidelines.md) |
| 准备同步阶段进度 | [阶段执行台账规范](./stage-ledger-guidelines.md) |
| 想理解常见错误顺序 | [工作流反例](./workflow-anti-patterns.md) |

## 最短检查清单

提交前：

- [ ] commit 信息符合项目约定；
- [ ] 当前分支符合项目工作流，或已有明确授权；
- [ ] work commit 不包含活动任务目录；
- [ ] 如本次沉淀了 `.trellis/spec/**`，相关 spec 已随本次 work commit 提交。

创建 MR / PR 前：

- [ ] 工作分支已推送；
- [ ] MR / PR 标题能对应任务或阶段编号；
- [ ] 描述包含概述、主要变更、验证和影响范围。

合并后：

- [ ] 本地主开发分支已更新；
- [ ] 已删除本地已合并分支；
- [ ] 远端源分支已删除或确认由平台自动删除。

