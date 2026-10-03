# pi-hermes-memory 案例

pi-hermes-memory 扩展的典型文件：提示词原文与实际产出，供[手册规范](../../handbook/pi-hermes-memory/index.md)对照实例。

## 清单

- [memory-pin 提示词](./memory-pin.md)：`/memory-pin` 的用法、八条 pin 草案全文、`STANDING.md` 注入块的真实渲染格式。
- [memory-policy 提示词](./memory-policy.md)：注入 system prompt 的记忆策略原文——「便条优先」条款的出处。
- [后台复习提示词](./review-prompt.md)：`COMBINED_REVIEW_PROMPT` 原文——决定「什么被记住」的抽取器。

## 状态与出处

- 八条 pin 草案为**待批准**状态，`STANDING.md` 尚未写入；批准与写入后需回改本页。
- 验收基线：迁移前 user 层投诉 3 条（2026-08-26「谁让你改spec文件的」、08-29「你自己想，不要问我」、09-13「你为什么还问我？？？」），目标为归零。
- 完整工作台（核验记录、验证设计、清理处置表、可复现脚本）在 iGuo 仓库 `apps/thera/examples/pi-hermes-memory/`，本仓只收规范与案例。

## 读法

三份文件回答同一个问题的三个侧面：**什么会被记住**（review-prompt）、**记住的东西效力多低**（memory-policy）、**怎样把效力提上来**（memory-pin）。与手册的四层存放规范对读。
