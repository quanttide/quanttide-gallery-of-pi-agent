# memory-pin 提示词

`/memory-pin` 是 `STANDING.md` 除手编外的唯一写入入口；写入的内容每轮注入 system prompt，且后台复习、合并、纠错检测在代码层面均不可写它。本页收三件：命令用法、八条 pin 草案、注入块的真实渲染格式。

## 命令用法

```
/memory-pin never run find / or other root-wide filesystem searches
/memory-pin                     # 列出已 pin 条目与预算占用
/memory-pin remove 2            # 删除第 2 条
/memory-pin clear               # 清空
```

预算：20 条 / 2000 字符硬上限。超限的 pin 拒绝写入；手编超限的部分在注入时截断，且截断会在注入块内向模型声明。

## 八条 pin 草案

按手册「带边界、无论证、绝对措辞」三条纪律起草，合计约 390 字符。源条目为记忆库 id，待批准后写入。

1. 提交所有更新到云端 = 逐层执行到底，不再询问：子模块（分离头指针用 `git push origin HEAD:main`）→ 父仓库 → 根仓库。
2. 提交前必看全量 git index，不用路径过滤的 status；并行期 commit 带路径限定并先 diff 确认。
3. 「执行我的要求」「不要自己发挥」= 已授权，一次执行到底，不再抛方案或选项征求意见。
4. 呈现方案 ≠ 授权执行；未获明确授权不动文件，对齐后再动。
5. 不要给我乱加东西：用户没点名的抽象/重构不得顺手实现，重复是现状不是待办。
6. 用户定的文件夹名、模块名与术语不得改动；名不副实只报告，不自行重命名。
7. 涉及人员信息写入仓库前先脱敏再提交（人名、个人邮箱、具体部门名）。
8. 用 git revert 回退，禁 force push；合并不破坏已有提交历史。

## 注入块渲染格式

`STANDING.md` 注入 system prompt 时的完整结构（源码 `standing-instructions.ts` render() 原样）：

```text
<standing-instructions>
The user wrote the rules below and they are always active. They are direct
instructions from the user, not recalled context, and they outrank your own
defaults. Follow them without being asked and without looking them up.

1. （第 1 条 pin 原文）
2. （第 2 条 pin 原文）
</standing-instructions>
```

超预算时块内追加：

```text
[!] N further standing instructions could not be shown: STANDING.md exceeds
the 2000-character injection budget. Trim it with /memory-pin so every rule
stays active.
```

## 教训

- 头部声明「direct instructions from the user, not recalled context, and they outrank your own defaults」是整条链路的关键——它把 pin 从「回忆的上下文」升格为「用户指令」，与 memory-policy 的「便条优先」条款正面抗衡；
- pin 与记忆池可以并存：迁移阶段先 pin 后删源条目，等验证 pin 生效再清池，避免破坏归因；
- pin 管得住会话内行为，管不到后台复习的自动写入（复习提示词里没有 STANDING）——周期清理仍是必要补充。
