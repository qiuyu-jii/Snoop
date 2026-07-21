# Snoop / 思脑谱 Agent 指引中心

`.agents/` 是思脑谱所有 Agent 规则、专项技能、工作流和可复用提示词的唯一详细来源。根目录 `AGENTS.md` 只负责让工具自动发现并进入这里。

## 文件职责

- `README.md`：所有思脑谱任务都要遵守的仓库定位、读取顺序、授权边界和路由规则。
- `workflows/github.md`：Commit、Push、PR、Merge，以及合并后的本地和远程分支清理流程。
- `prompts/snoop-task.md`：发起新思脑谱任务时可直接复制的提示词。
- `skills/snoop-archive-import/SKILL.md`：从 Apple Notes 等来源导入文章时使用的专项技能，包括原文保真、分类、Temp、Upload_Log 和人工确认门槛。

## 每次任务的读取顺序

1. 阅读仓库根目录 `README.md`、`ChangeLog.md` 和当前目录结构。
2. 阅读与任务直接相关的现有文章或说明，不批量加载无关内容。
3. 阅读 `workflows/github.md`，了解本次任务允许执行到哪一步。
4. 如果任务涉及文章导入，完整阅读并遵守 `skills/snoop-archive-import/SKILL.md`。

## 通用仓库规则

- 思脑谱是长期维护的个人意识档案，保存原始想法、文章、文学创作、诗歌、直接经验和消逝世界。
- 尊重已有文本的原貌、时间、标题、标点和历史位置；未经用户明确要求，不重写、覆盖、移动或删除既有内容。
- 根据仓库根目录 `README.md` 的定义选择 `log/notes/`、`log/essays/`、`log/prose/`、`log/poems/`、`observations/` 或 `lost-world/`，不根据来源文件夹直接猜测分类。
- 只修改完成当前任务所需的文件，保留任务开始前已有的无关修改。
- 新增或移动文章时，核对标题、日期、文件名、分类、正文和目标路径。
- 提交前检查差异，确保没有意外改写旧文章，也没有混入临时文件或无关修改。
- `Temp/`、`Upload_Log/` 和 `.DS_Store` 不得进入 Git 提交。

## 授权边界

- 普通任务默认只完成本地修改和验证。
- 只有用户明确要求 Commit、Push、创建 PR 或 Merge 时，才执行对应步骤；不得从普通编辑请求中自行扩大授权。
- 文章导入还必须通过专项 Skill 的人工确认门槛。未确认前，不得把草稿复制进正式档案，也不得 Commit、Push 或创建 PR。
- 未经用户单独明确要求，不得 Merge PR。

## 任务路由

- 整理、修改或新增普通仓库说明：遵守本文件和 `workflows/github.md`。
- 导入、转换、分类或发布来源文章：额外使用 `skills/snoop-archive-import/SKILL.md`。
- 用户需要发起一个新的通用任务：可复制 `prompts/snoop-task.md`，替换其中的任务内容。
