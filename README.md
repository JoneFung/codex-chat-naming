# Codex Chat Naming

[简体中文说明](README.zh-CN.md)

A small Codex skill that gives new conversations consistent, searchable titles.

## What it does

After the first substantive request, the skill renames **the current Codex conversation** to:

```text
核心对象或成果 YYMMDD
```

The first part names the object and result. The final six digits are the conversation creation date in `Asia/Shanghai`.

Examples:

```text
Codex 标题与项目治理 261101
AI 自动剪辑边界 261013
健身打卡成片 261028
素材索引与全片预览 261011
```

## Behavior

- Changes only the current conversation.
- Skips a title that already follows the format.
- Reads the actual creation date rather than guessing it.
- Verifies the title after changing it.
- Stops safely if the current conversation cannot be identified or the rename tool is unavailable.
- Never claims a rename succeeded when it did not.

## Requirements

- Codex Desktop with local Skill support.
- Access to the current thread ID.
- Tools named `read_thread` and `set_thread_title`.

This package was tested with Codex. The workflow is not portable to hosts that lack equivalent thread-title tools.

## Install

Clone this repository as the `chat-naming` Skill:

```bash
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/JoneFung/codex-chat-naming.git \
  "$HOME/.agents/skills/chat-naming"
```

Add this one-line trigger to `$HOME/.codex/AGENTS.md`:

```text
新对话首轮主题明确后，调用 `$chat-naming` 命名当前对话；标题已合规或主题不明确时不处理。
```

Start a new Codex conversation. Existing conversations are not renamed automatically.

## Usage

Use Codex normally.

1. Create a conversation in the appropriate project.
2. Describe a concrete task.
3. After the first substantive turn, the Skill derives the title, reads the conversation creation time, renames the current thread, and verifies the result.
4. Continue the conversation without repeating the naming request.

The title will not change before there is enough context to identify the task.

## Safety and scope

This Skill changes conversation-title metadata only. It does not modify project folders, source files, repositories, or unrelated conversations.

Historical cleanup is outside this Skill. Generate an old-to-new title mapping first, review it, and obtain confirmation before making bulk changes.

## Compatibility

| Host | Status |
|---|---|
| Codex | Supported and tested |
| Claude Code | Not supported: its model does not expose the same thread-title tools |
| Doubao | Not supported: custom Skills cannot batch-rename conversations |
| WorkBuddy | Unverified; requires equivalent title tools |

## Troubleshooting

- **The title does not change:** confirm the Skill is loaded, the conversation is in a project, and the host exposes `read_thread` and `set_thread_title`.
- **The date is missing:** the creation timestamp could not be read. Keep the original title rather than inventing a date.
- **The title is too vague:** provide the object and expected result in the first request.
- **The title is already correct:** this is expected; the Skill avoids unnecessary writes.

## Repository contents

```text
SKILL.md
agents/openai.yaml
README.md
README.zh-CN.md
LICENSE
```

## License

MIT
