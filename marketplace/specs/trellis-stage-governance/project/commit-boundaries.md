# 提交边界规范

> 定义 Trellis 驱动任务中的提交分组和文件归属。

## 提交类型边界

| 提交类型 | 内容 |
|----------|------|
| work commit | 任务的主要产物，例如代码、测试、产品文档、协作文档，以及与本次产物强绑定的 `.trellis/spec/**` |
| archive commit | Trellis 任务归档后进入 `.trellis/tasks/archive/**` 的文件 |
| journal commit | Trellis workspace / journal 中记录本次会话或任务过程的文件 |
| docs progress commit | MR / PR 编号确定后，对阶段执行台账追加的进度同步 |

## 活动任务目录

活动任务目录通常位于 `.trellis/tasks/<active-task>/`。这些文件记录当前任务过程，不属于工作产物。

work commit 不应包含：

- `.trellis/tasks/<active-task>/prd.md`；
- `.trellis/tasks/<active-task>/design.md`；
- `.trellis/tasks/<active-task>/implement.md`；
- `.trellis/tasks/<active-task>/task.json`；
- `.trellis/tasks/<active-task>/*.jsonl`。

这些文件应在任务收尾阶段由 Trellis 归档流程处理。

## Spec-only 工作

当任务的主要产物就是 `.trellis/spec/**` 时，work commit 可以只包含 spec 文件。此时仍应遵守：

- 不把活动任务目录加入 work commit；
- 不把 journal 文件加入 work commit；
- 不把阶段执行台账提前加入 work commit；
- 如该任务属于阶段计划，仍按阶段执行台账规则在 MR / PR 编号确定后同步进度。

## 提交信息建议

推荐使用 Gitmoji + type + scope + 中文标题：

```text
📝docs(project): 添加项目治理规范

- 明确 Trellis 任务提交边界；
- 补充阶段执行台账同步规则；
- 增加工作流反例说明；
```

要求：

- 标题简洁说明本次变更；
- 详情分点连续排列，每点以中文分号结尾；
- 不写与项目产物无关的工具生成说明；
- 如有关联任务、issue 或 MR / PR，可在详情后单独补充。

## 提交前检查

- [ ] 已用路径限定方式 staging，避免误加无关文件；
- [ ] work commit 只包含本次主要产物；
- [ ] 活动任务目录未进入 work commit；
- [ ] archive、journal、docs progress 按各自时机单独提交；
- [ ] 阶段计划中的“主要提交”只记录 work commit。

