# 后台复习提示词

`COMBINED_REVIEW_PROMPT`：每 10 轮或 15 次工具调用触发一次的后台抽取，决定「什么被记住」。原文移植自 Hermes agent 的 `_COMBINED_REVIEW_PROMPT`。

## 原文

```text
Review the conversation above and consider these aspects:

**Memory**: Has the user revealed things about themselves — their persona,
desires, preferences, or personal details? Has the user expressed expectations
about how you should behave, their work style, or ways you want me to operate?
If so, save using memory_add.

**Failures & Corrections**: Did anything fail or go wrong? Extract these as
failure memories:
- [failure] What was tried but didn't work? (e.g., "Used localStorage for
  tokens — XSS vulnerability")
- [correction] Did the user correct you? (e.g., "Use pnpm, not npm")
- [insight] What was learned from the experience?
- [convention] Any project conventions discovered?
- [tool-quirk] Any tool-specific knowledge gained?

For failures, include: what was tried, why it failed, what error occurred,
and what worked instead.

**Skills**: Do NOT create or modify skills in this background review.
Procedural skills are managed explicitly by the main agent through the
skill_manage tool during normal work, not by this review subprocess.

Only act if there's something genuinely worth saving. If nothing stands out,
just say 'Nothing to save.' and stop.
```

## 读法

- 抽取目标只有三类：用户画像、失败与纠正、（技能明确禁写）——**没有任何一条要求记录「这次对话发生了什么」**。记忆库早期的会话叙事体（「用户询问…」「用户说…」）在 2026-08-29 后绝迹，机制出处即在此；
- 「Only act if there's something genuinely worth saving」是写入的真正筛子：值不值得占位由 LLM 判断，而非字符配额（policy-only 模式下 5000 上限不卡写入）；
- 该提示词看不到 `STANDING.md`——用户 pin 的规则不在其中，自动抽取与手动立规是两条互不知晓的通路，故「先迁授权、再放任合并」不可颠倒。

## 教训

- 想改变记忆的形状，改的是这条提示词的行为预期，不是记忆表结构；
- 「失败是显式触发信号」（failure 类目自带 `failure_reason` 字段）源于这里对失败的专门段落；顺利的事不会被抽取，疼的事才会。
