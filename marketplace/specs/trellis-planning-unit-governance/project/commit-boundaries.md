# 提交边界规范

> 定义 work commit、archive commit、journal commit 和 docs progress commit 的文件边界。

---

## 适用范围

当你准备 `git add`、分组提交、检查脏文件归属，或判断某个 spec / Trellis 文件应不应该进入工作提交时，读取本文件。

---

## 提交格式

### 格式模板

```text
📝docs(project): 添加项目 Git 工作流规范

- 沉淀分支、提交和 MR / PR 操作约定；
- 明确任务完成后的计划单元同步要求；
- 补充分支合并后的本地和远端清理流程；
```

详情分点之间应连续排列，不插入空行。命令行提交时不要为每条详情分别使用一个 `-m`，因为 Git 会把多个 `-m` body 视作多个段落并自动插入空行。推荐使用单个 body 字符串或提交信息文件，例如：

```bash
git commit -m "📝docs(project): 添加项目 Git 工作流规范" -m $'- 沉淀分支、提交和 MR / PR 操作约定；\n- 明确任务完成后的计划单元同步要求；\n- 补充分支合并后的本地和远端清理流程；'
```

### 格式要求

| 序号 | 规则 | 说明 |
|------|------|------|
| 1 | **Gitmoji 开头** | 根据提交内容选择合适的 emoji，如 `✨`、`🐛`、`📝`、`🔧` |
| 2 | **类型标识** | 使用 `feat`、`fix`、`docs`、`chore`、`refactor`、`test`、`style` 等 |
| 3 | **影响范围** | 在括号中注明影响模块或范围，如 `(backend)`、`(frontend)`、`(project)`、`(docs)` |
| 4 | **冒号空格** | scope 后紧跟英文冒号和一个英文空格，即 `: ` |
| 5 | **标题摘要** | 标题简洁概括变更内容，语言跟随目标项目约定 |
| 6 | **详细说明** | 如需补充，标题与详情之间留一空行；详情使用连续分点说明，分点之间不加空行，每点以分号结尾 |
| 7 | **任务关联** | 如已知关联 issue、任务、文档或工单链接，可在详细说明后空一行补充 |
| 8 | **禁止无关工具描述** | 不写“由某工具生成”等与项目功能无关的描述；Trellis 框架相关内容除外 |

### 常用类型与 Gitmoji

| 类型 | Gitmoji | 用途 |
|------|---------|------|
| `feat` | ✨ | 新功能开发 |
| `fix` | 🐛 | 缺陷修复 |
| `docs` | 📝 | 文档更新 |
| `chore` | 🔧 | 构建、工具、配置、依赖等杂项 |
| `refactor` | ♻️ | 不改变行为的代码重构 |
| `test` | ✅ | 测试相关变更 |
| `style` | 💄 | 代码格式或样式调整，不影响功能 |

---

## 文件类型与修改时机

| 文件类型 | 什么时候修改 | 是否属于 `trellis-update-spec` |
|----------|--------------|:---:|
| `.trellis/spec/**` | 发现新的代码约定、流程约束、反例或防错规则后 | ✅ |
| `.trellis/tasks/**` | 任务创建、启动、归档或 Trellis 任务脚本处理 | ❌ |
| `.trellis/workspace/**` | finish-work / journal 记录 session 时处理 | ❌ |
| 计划单元执行台账 | finish-work 已完成、分支已推送且 MR / PR 编号确定后，作为 docs progress commit 追加 | ❌ |
| 计划单元任务书 | `T000` 规划任务产出；后续仅在用户或团队明确要求修订规划契约时修改 | ❌ |
| README、需求、架构等产品 / 协作文档 | 任务 PRD 明确要求，或专门的文档任务中修改 | ❌ |

计划单元任务书和执行台账都不是 code-spec。即使任务实现已经完成，也不要在 `trellis-update-spec` 阶段把计划单元内任务标为已完成。

---

## Trellis 文件边界

Trellis 任务驱动的工作必须把“工作产物”和“流程记录”分开提交。工作提交可以包含业务代码、测试、任务要求的产品 / 协作文档，以及与本次实现强绑定的 `.trellis/spec/**` code-spec；但不得包含活动任务目录 `.trellis/tasks/<active-task>/`。

提交分组规则：

- **work commit**：业务代码、测试、相关 `.trellis/spec/**`、任务 PRD 明确要求同步的 README / 需求 / 架构等协作文档；
- **archive commit**：由 finish-work 或任务归档脚本生成的 `.trellis/tasks/archive/**`；
- **journal commit**：由 finish-work 或 journal 脚本生成的 `.trellis/workspace/**`；
- **docs progress commit**：MR / PR 编号确定后追加的计划单元执行台账进度同步。

禁止把 `.trellis/tasks/<active-task>/prd.md`、`task.json`、`implement.jsonl`、`check.jsonl` 或类似活动任务过程文件加入 work commit。这些文件只用于当前任务执行过程，应在 finish-work 阶段由归档流程移动到 `.trellis/tasks/archive/**` 并单独提交。

`.trellis/spec/**` 的处理不同。spec 是可执行代码契约：如果本次业务实现新增了稳定接口、错误边界、测试要求或提交流程规则，相关 spec 修改应和对应 work commit 一起提交，便于审阅者同时看到“代码行为”和“约束解释”。不要把 spec 修改留给 archive / journal 自动提交兜底。

### 纯 spec 工作提交

当 Trellis 任务本身的主要产出就是 `.trellis/spec/**` 更新时，work commit 可以只包含 spec 文件。例如计划单元中的经验沉淀任务、工具规范任务或工作流规则补充任务。

执行要求：

- commit 前用路径限定 staging，例如 `git add .trellis/spec/project`；
- `git status --short` 中可以存在未跟踪的活动任务目录，但这些文件不得进入 work commit；
- work commit 不包含 `.trellis/tasks/<active-task>/`、`.trellis/workspace/**`、计划单元执行台账或未经明确授权的计划单元任务书修订；
- 如果纯 spec 任务属于计划单元，仍按 `work commit -> archive commit -> journal commit -> MR / PR -> docs progress commit` 的顺序处理；
- 不要因为本次只改 spec，就跳过 Trellis 归档、journal 或 MR / PR 后执行台账同步。

### 正例

- work commit 只包含代码、测试和相关 `.trellis/spec/**`，活动任务目录未进入 work commit；
- 随后的 archive commit 只新增 `.trellis/tasks/archive/**` 归档文件；
- journal commit 只记录 `.trellis/workspace/**`；
- 计划单元执行台账等 MR / PR 编号确定后再追加 docs progress commit。

### 反例

- work commit 把 `.trellis/tasks/<active-task>/prd.md`、`task.json`、`implement.jsonl` 或 `check.jsonl` 一起提交；
- 归档后 Git 继续跟踪原活动目录的删除，于是必须额外产生清理活动目录提交；
- 审阅者难以判断哪些文件才是本次真正产物。

---

## 计划单元任务提交前检查清单

- [ ] work commit 只包含代码、测试、必要 code-spec 和任务 PRD 明确要求的文档；
- [ ] work commit 不包含 `.trellis/tasks/<active-task>/` 下的 PRD、task.json、implement.jsonl 或 check.jsonl；
- [ ] 如本次实现沉淀了 `.trellis/spec/**`，该 spec 修改已随相关 work commit 一起提交；
- [ ] 如本次是纯 spec 任务，work commit 只包含 `.trellis/spec/**`，活动任务目录留给归档提交；
- [ ] 没有把计划单元执行台账误放进 work commit 或 `trellis-update-spec` 提交；
- [ ] 没有在普通子任务收尾中修改计划单元任务书；如确需修订，已有明确授权并使用独立 commit；
- [ ] finish-work 已执行；
- [ ] archive commit 已经在当前工作分支；
- [ ] journal commit 已经在当前工作分支；
- [ ] 工作分支已经推送并创建 MR / PR；
- [ ] MR / PR 编号已经确定；
- [ ] 只有在 MR / PR 编号确定后，才提交计划单元执行台账 docs progress commit；
- [ ] 计划单元台账“主要提交”只记录主要 work commit，不记录 archive、journal 或 docs progress commit。
