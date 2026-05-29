# Trellis 计划单元项目治理 Spec 模板

这个模板用于初始化一组项目通用的 Trellis 协作规范。它关注的是项目治理、任务生命周期、Git / MR 协作、计划单元任务书和计划单元执行台账，而不是具体技术栈。

模板内容使用“计划单元”作为抽象模型。阶段、专项、里程碑批次等都可以作为计划单元实例使用。

## 适用场景

- 项目希望使用 Trellis 管理需求、任务、执行记录和归档；
- 团队需要统一分支、提交、MR / PR 和合并后清理规则；
- 项目按阶段、专项或里程碑批次推进，需要区分稳定规划契约和滚动执行台账；
- 多个智能体或协作者共同参与，需要把流程规则写成可复用 spec。

## 不适用内容

本模板不包含具体语言、框架、部署、CI、数据库、前端、后端或运行环境规则。目标项目应在初始化后按自身技术栈补充 package / layer 级 spec。

## 目录说明

```text
project/
  index.md
  git-workflow-guidelines.md
  commit-boundaries.md
  mr-guidelines.md
  trellis-task-lifecycle.md
  planning-unit-planning-guidelines.md
  planning-unit-ledger-guidelines.md
  tooling-guidelines.md
  workflow-anti-patterns.md
guides/
  index.md
  code-reuse-thinking-guide.md
  cross-layer-thinking-guide.md
```

`project/` 保存仓库级治理规则，`guides/` 保存通用思考指南。模板被 Trellis 初始化到目标项目后，这些目录会直接成为目标项目 `.trellis/spec/` 下的内容。

## 使用后建议

初始化后请先按项目真实情况修订：

- 主开发分支名称；
- 代码托管平台和 MR / PR 创建方式；
- commit scope 示例；
- 计划单元分类、编号前缀和文档命名格式；
- 是否需要新增技术栈相关 spec。
