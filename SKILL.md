---
name: chat-naming
description: Name the current Codex conversation on its first substantive turn. Use only for automatic conversation-title formatting, not bulk history cleanup.
---

# Conversation Naming

After the first substantive request establishes the task, rename only the current Codex conversation to:

```text
核心对象或成果 YYMMDD
```

Put one ASCII space between the outcome and the date.

1. Do nothing when the current title already follows this format and describes the task.
2. Use the current conversation ID supplied by the run context. Stop if it cannot be identified; never guess.
3. Use `read_thread` to read the current conversation's `createdAt`, then convert it to `YYMMDD` in the `Asia/Shanghai` timezone.
4. Lead with the core object and outcome, for example `国庆故事线与分镜` or `健身打卡成片`.
5. Do not add generic type prefixes such as `研究`, `规划`, `剪辑`, or `整理`. Do not repeat the project name, add status, or add meaningless numbering.
6. Keep the prose portion to roughly 6–20 Chinese characters or equivalent information. Prefer searchable object and outcome words.
7. Examples: `Codex 标题与项目治理 261101`, `AI 自动剪辑边界 261013`.
8. Use `set_thread_title` to change the current conversation, then use `read_thread` to verify the result.
9. Modify only the current conversation. If the creation date or rename tool is unavailable, keep the existing title and explain the limitation.
