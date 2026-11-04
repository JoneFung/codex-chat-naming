# Codex 对话自动命名

[English README](README.md)

这是一个小型 Codex Skill，用统一、便于搜索的标题整理新对话。

## 作用

新对话的首轮任务明确后，自动将**当前 Codex 对话**改名为：

```text
核心对象或成果 YYMMDD
```

前半部分描述对象和结果，末尾六位数字是对话创建日期，按 `Asia/Shanghai` 时区计算。

例如：

```text
Codex 标题与项目治理 261101
AI 自动剪辑边界 261013
健身打卡成片 261028
素材索引与全片预览 261011
```

## 行为规则

- 只修改当前对话。
- 标题已经合规时不重复修改。
- 读取真实创建日期，不猜测日期。
- 改名后重新读取并验证。
- 无法识别当前对话或没有改名工具时安全停止。
- 没有真正改名时，不声称已经成功。

## 使用条件

- 支持本地 Skill 的 Codex 桌面版。
- 可以访问当前对话 ID。
- 提供 `read_thread` 和 `set_thread_title` 工具。

本项目已在 Codex 中验证。没有相同标题工具的其他平台不能直接复用完整的自动改名流程。

## 安装

将仓库克隆为 `chat-naming` Skill：

```bash
mkdir -p "$HOME/.agents/skills"
git clone https://github.com/JoneFung/codex-chat-naming.git \
  "$HOME/.agents/skills/chat-naming"
```

在 `$HOME/.codex/AGENTS.md` 中加入一行触发规则：

```text
新对话首轮主题明确后，调用 `$chat-naming` 命名当前对话；标题已合规或主题不明确时不处理。
```

然后新建 Codex 对话。已有对话不会被自动追溯修改。

## 使用方法

按平时习惯使用 Codex：

1. 在对应项目中新建对话。
2. 直接描述具体任务。
3. 首轮任务明确后，Skill 会生成标题、读取创建日期、修改当前对话并回读验证。
4. 后续继续对话，不需要重复要求命名。

如枟第一条消息还不足以判断任务，标题会暂时保持不变，不会生成模糊名称。

## 安全范围

这个 Skill 只修改对话标题信息，不修改项目文件夹、源代码、Git 仓库或其他对话。

历史对话批量整理不包含在此 Skill 中。需要先输出新旧标题对照，确认后再批量修改。

## 平台兼容性

| 平台 | 状态 |
|---|---|
| Codex | 支持并已测试 |
| Claude Code | 不支持：模型没有等价的线程标题工具 |
| 豆包 | 不支持：自定义 Skill 无法批量修改对话名称 |
| WorkBuddy | 未验证，需要平台提供等价的标题接口 |

## 常见问题

- **标题没有变化：**确认 Skill 已加载、对话属于项目，并且平台提供 `read_thread` 和 `set_thread_title`。
- **日期缺失：**创建时间无法读取。此时应保留原标题，不要编造日期。
- **标题太模糊：**首轮请求中写清对象和预期结果。
- **标题没有重复修改：**这是正常行为，避免产生无意义的重复写入。

## 仓库内容

```text
SKILL.md
agents/openai.yaml
README.md
README.zh-CN.md
LICENSE
```

## 许可证

MIT
