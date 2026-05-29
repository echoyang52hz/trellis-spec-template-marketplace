# 工作流反例

> 用反例说明为什么任务、提交、归档和计划单元台账的顺序不能混在一起。

---

## 反例：提前同步计划单元执行台账

**偏差**：work commit 后立即把计划单元执行台账标记为完成；此时任务尚未归档，journal 尚未记录，MR / PR 编号尚未确定。

**为什么不对**：执行台账会先于 Trellis 生命周期记录“已完成”状态，导致历史审计时难以确认文档、归档、journal 和 MR / PR 是否处于同一完成链路中，也违背了“MR / PR 编号确定后再同步执行台账”的约定。

**正确顺序**：

```text
work commit -> finish-work -> archive commit -> journal commit -> push
-> create MR / PR -> docs progress commit -> push
```

**错误顺序**：

```text
work commit -> docs progress commit -> finish-work -> archive commit
-> journal commit -> push -> create MR / PR
```

---

## 反例：把半成品状态写进执行台账

**偏差**：work commit 之后、finish-work 之前，提前把任务标记为已完成，并写入活动任务 PRD 路径和“MR / PR 待创建”。

**为什么不对**：计划单元执行台账的完成记录应记录最终可追溯信息。此时 Trellis 任务尚未归档，journal 尚未记录，MR / PR 编号尚未确定，因此写入的是半成品状态。

**正确做法**：

```text
trellis-update-spec 阶段只更新 .trellis/spec/**
-> work commit
-> finish-work
-> archive commit
-> journal commit
-> push
-> create MR / PR
-> docs progress commit
-> push
```

---

## 反例：活动任务目录混入 work commit

**偏差**：work commit 包含 `.trellis/tasks/<active-task>/` 下的 `prd.md`、`task.json`、`design.md`、`implement.md`、`implement.jsonl` 或 `check.jsonl`。

**为什么不对**：这些文件属于活动任务过程记录，不属于工作产物。归档后 Git 可能继续跟踪原目录删除，只能再补一个清理活动目录提交，制造额外历史噪音。

**正确做法**：

- work commit 只提交代码、测试、产品文档和相关 `.trellis/spec/**`；
- 活动任务目录交给归档脚本或 finish-work 处理；
- archive commit 只新增 `.trellis/tasks/archive/**`，不再额外制造“清理活动目录”提交。

---

## 反例：把任务级警戒留在单个 implement.md 中

**偏差**：多个任务反复在 `implement.md` 中写入同一组警戒，例如 inline 模式主会话执行、work commit 不包含活动任务目录、计划单元台账必须等 MR / PR 编号确定后同步、主要提交不记录 archive / journal / docs progress，但没有沉淀回项目级 spec。

**为什么不对**：任务级 `implement.md` 是一次性执行计划，归档后主要用于审计，不是后续任务的默认注入上下文。可复用的流程规则如果只留在历史任务里，新任务在上下文压缩、换平台或换智能体后容易漏读。

**正确做法**：

- 任务级 `implement.md` 继续写本任务的具体提交边界、验证命令和风险回滚点；
- 已被多个任务重复验证的流程规则，应更新到 `.trellis/spec/project/` 对应专题文件；
- 进入新任务前读取项目级 spec，而不是复制历史任务的整份 implement；
- 如果计划单元内任务暴露了新的可复用防错规则，在 work commit 前评估是否需要随本次产物更新 spec。

---

## 反例：把纯 spec 计划单元任务当成普通小修

**偏差**：`.trellis/spec/**` 的小范围补充有时可以直接修改，但计划单元中的 spec 经验沉淀任务可能需要回顾 journal、历史任务、提交链路和 MR / PR 规范执行情况。如果因为“只改 spec”就跳过任务规划、归档、journal 或 MR / PR 后执行台账同步，会让计划单元显示的完成状态无法追溯到对应 MR / PR 和归档 PRD。

**为什么不对**：是否需要 Trellis 任务，取决于工作是否已纳入计划单元、是否需要过程留痕和验收判断，而不取决于最终改的是代码、文档还是 spec。纯 spec 也可能是一个完整计划单元任务。

**正确做法**：

- 单条已确认规则补充可以直接作为小修提交；
- 计划单元内的 spec 沉淀任务应创建 Trellis 任务，写清证据来源、目标 spec 文件、验收标准和提交边界；
- work commit 可以只包含 `.trellis/spec/**`，但活动任务目录、journal 和计划单元进度必须按各自提交边界处理；
- MR / PR 编号确定后，再把计划单元执行台账中对应任务标为已完成并记录归档 PRD、MR / PR、主要 work commit 和分支。

---

## 反例：专项任务继续套用阶段编号

**偏差**：某个单一能力域的小型推进已经更适合做成专项，却继续使用阶段编号，导致它看起来像项目主线新阶段，外围文档也容易被误同步为主线阶段状态。

**为什么不对**：阶段和专项流程一致，但应用范围不同。阶段可使用 `Pxxx`，专项可使用 `Sxxx`；二者同属计划单元，不能因为流程相同就混用编号前缀。

**正确做法**：

- 项目主线演进周期使用阶段编号，例如 `P010-T000`；
- 单一能力域或问题域的小型阶段化推进使用专项编号，例如 `S001-T000`；
- 后续规则统一按计划单元执行：`T000` 产出任务书和执行台账，MR / PR 编号确定后同步执行台账；
- 外围 README、需求、架构、路线图只同步对应层级入口，不把专项误写成主线阶段。

---

## 反例：修改目录结构时只更新自动路径

**偏差**：一个功能有两条路径会产出同一批文件，例如初始化路径使用递归目录复制，更新路径使用手工文件清单。目录迁移时只更新了自动复制路径，手工清单没有同步。

**为什么不对**：初始化看起来正常，但更新会把文件写到旧路径、漏掉新文件，或造成同一模板在不同命令下输出不一致。

**正确做法**：

- 修改目录、文件名、模板路径或生成规则前，搜索所有旧路径和旧结构引用；
- 如果一个路径自动派生、另一个路径手工列举，必须同步更新手工清单；
- 增加回归检查，对比不同机制输出的文件集合。
