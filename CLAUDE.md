# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## 这是什么仓库

个人 2026 秋招准备知识库，不是软件项目——**没有构建、测试或 lint 命令**，全部内容为 Markdown。用于沉淀刷题、计算机基础（八股）笔记、简历迭代、投递进度和面试复盘。内容随求职进度持续更新。

## 目录结构

- `algorithms/leetcode-hot-100/README.md` — 刷题记录（单文件，约 5300 行），按专题组织（数组、滑动窗口、链表、二叉树、栈、二分、回溯、动态规划、图论等）
- `cs-fundamentals/` — 八股笔记，含 `computer-network`、`operating-system`、`database`、`cpp` 四个专题
- `resume/` — 脱敏简历（`游戏研发工程师.md`）与版本记录、PDF 导出
- `applications/` — 投递看板（`README.md` 汇总表格）与单岗位模板
- `interviews/` — 面试复盘索引与复盘模板
- `reviews/` — 周/阶段复盘（`weekly-template.md` + 按周编号的实际文件）
- `tmp/` — 生成的临时文件（已被 git 忽略，不提交）

## 内容约定

新增/更新笔记时遵循：

- **刷题条目**格式：`###### [题号. 题名](leetcode链接)`，随后依次为 难度 / 自主解答（含二刷、三刷情况）/ 思路 / 时间复杂度 / C++ 代码（部分题附 Python）。文件顶部有 `## 更新记录`，新增题目时按日期同步追加一条。
- **八股笔记**按「结论 → 原理 → 追问 → 示例 → 易错点」整理，优先写自己的理解；模板见各专题 README 的「笔记模板」。
- 新条目以对应 `template.md` / `weekly-template.md` 为起点复制后填写。

## 隐私与脱敏（重要）

- 手机号、邮箱、候选人编号、面试官姓名、内部材料等敏感信息**不得提交**到公开仓库。
- 私密内容放入各模块 `private/` 目录（`resume/private`、`applications/private`、`interviews/private`），或使用 `*.private.md` / `*.private.csv` 命名——这些已被 `.gitignore` 忽略。
- 投递与面试复盘使用匿名公司代号；如需公开公司名称，先移除敏感信息。
- 投递链接中的个人追踪参数需删除。

## 状态约定

- 投递状态流转：`准备中 → 已投递 → 笔试 → 面试 → Offer / 结束`。
- 面试复盘建议在每场面试后 24 小时内完成。

## 环境注意

- `.git` 目录归属另一个 Windows 用户，直接执行 git 命令会报 `detected dubious ownership`。需先运行：`git config --global --add safe.directory 'C:/Users/Bitte/Desktop/2026秋招'`。
