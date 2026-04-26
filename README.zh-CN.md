# AGENTS.md

[![English](https://img.shields.io/badge/README-English-blue)](README.md) [![中文](https://img.shields.io/badge/README-中文-red)](README.zh-CN.md)

这是一个面向 AI Coding Agent 的两层工作规范模板，基于 [agents-md](https://github.com/TheRealSeanDonahoe/agents-md) 修改而来。

感谢 Sean Donahoe 和 agents-md 项目提供的原始结构与实用原则。agents-md 本身也综合了多位实践者和社区的优秀经验，包括 Sean Donahoe 的 IJFW 原则、Andrej Karpathy 对 LLM 编程常见问题的观察、Boris Cherny 的 Claude Code 工作流、Anthropic 官方 Claude Code 最佳实践、社区反谄媚模式，以及 AGENTS.md 开放标准。本仓库建立在这些来源之上，保留其中通用价值较高的部分：不可妥协原则、先理解再编码、简单优先、外科手术式修改、验证闭环和会话卫生；同时重新组织为更适合团队使用的「跨项目一致性 + 仓库事实」两层结构。

## 名称说明

本仓库命名为 **agents-md**，是为了贴近 AGENTS.md 生态，同时表达它仍然是一套基于 Markdown 的 Coding Agent 工作约定。

在模板正文中，统一使用 **Agent 工作规范**，而不是反复写死 `AGENTS.md`。原因是可移植性：同一份规范内容可能以 `AGENTS.md` 被读取，也可能通过 `CLAUDE.md` import，被复制到 `GEMINI.md`，或通过软链复用。如果正文一直写“AGENTS.md”，当其他工具以不同文件名读取同一份内容时会造成语义错位。

具体文件名只在安装、兼容性和工具特定行为中出现；可复用内容本身使用“Agent 工作规范”“全局工作规范”“项目工作规范”。

## 适用场景

适合以下情况：

- 希望维护一份跨项目通用的全局规范；
- 每个仓库还需要一份项目专属规范；
- 希望 Codex、Claude Code、Gemini CLI、Cursor 等工具复用同一套内容；
- 希望把工作原则和项目事实分开维护；
- 希望进一步讨论个人或本机私有规则在不同工具中的加载方式。

## 核心理念

共享原则要稳定，项目事实要留在具体仓库中。

- **全局工作规范**回答：“Agent 在所有项目中应该如何工作？”
- **项目工作规范**回答：“Agent 在这个仓库中需要知道什么？”
- **个人或机器专属说明**回答：“哪些内容只对当前个人、机器或临时流程有效？”

个人/本地说明的加载方式取决于具体工具，不能假设所有 Coding Agent 都支持同一种 `.local.md` 约定。

## 与 agents-md 的区别

agents-md 提供了一份很强的单文件 `AGENTS.md` 模板。本版本主要调整的是组织方式和分层方式：

1. **从单文件混合改为区分全局级和项目级两层。**
   - 全局工作规范：跨项目通用行为和工程原则。
   - 项目工作规范：技术栈、命令、目录结构、项目约定、禁区和项目经验。
2. **专为跨 Coding Agent 复用设计。**
   - 模板正文使用“Agent 工作规范”，避免到处写死 `AGENTS.md`。
   - 具体文件名只在安装和兼容说明中出现。
   - 支持软链复用。
   - 也支持 Claude Code 的 `@AGENTS.md` 引用复用。

## 文件结构

```text
global/AGENTS.md        # 全局模板，英文
global/AGENTS.zh-CN.md  # 全局模板，中文
project/AGENTS.md       # 项目模板，英文
project/AGENTS.zh-CN.md # 项目模板，中文
```

## 安装方法

### 1. 选择需要的模板

先选择英文或中文版本，再决定是否需要全局规范、项目规范，或两者都需要。

典型布局：

```text
~/.agents/AGENTS.md      # 全局工作规范，前提是目标工具支持这个位置
<repo>/AGENTS.md         # 项目工作规范，提交到仓库
```

### 2. 安装全局工作规范

创建全局规范目录，并复制全局模板：

```bash
mkdir -p ~/.agents
cp global/AGENTS.md ~/.agents/AGENTS.md
```

如果偏好中文版本：

```bash
mkdir -p ~/.agents
cp global/AGENTS.zh-CN.md ~/.agents/AGENTS.md
```

注意：不是所有工具都会自动读取 `~/.agents/AGENTS.md`。如果目标工具有自己的全局位置，应复制或引用到该工具支持的位置。

示例：

```bash
# Claude Code 用户级指令
mkdir -p ~/.claude
cp global/AGENTS.md ~/.claude/CLAUDE.md

# Gemini CLI 用户级上下文
mkdir -p ~/.gemini
cp global/AGENTS.md ~/.gemini/GEMINI.md
```

### 3. 安装项目工作规范

在目标仓库根目录执行：

```bash
cp /path/to/agents-md/project/AGENTS.md ./AGENTS.md
```

如果偏好中文版本：

```bash
cp /path/to/agents-md/project/AGENTS.zh-CN.md ./AGENTS.md
```

然后补齐项目专属部分：技术栈、命令、目录结构、项目约定、验证要求、风险区域和项目经验。

### 4. 复用到工具专属文件

如果某个工具需要其他文件名，可以软链到 `AGENTS.md`，也可以创建工具专属 wrapper 文件。

软链示例：

```bash
ln -s AGENTS.md CLAUDE.md
ln -s AGENTS.md GEMINI.md
```

Claude Code wrapper 示例：

```md
# CLAUDE.md

@AGENTS.md

## Claude Code 补充规则

- 如需使用 Claude Code 特有能力，可写在这里。
```

### 5. 验证 Agent 实际加载内容

安装后，先让 Agent 总结当前生效的工作规范，再开始真实任务。如果工具提供 memory/context 检查命令，应以该命令为准。

示例：

- Claude Code：根据 Claude Code memory 文档检查已加载的 `CLAUDE.md` / import 文件。
- Gemini CLI：使用 `/memory show` 检查合并后的上下文。
- 通用 `AGENTS.md` 工具：让 Agent 总结当前指令，并在工具支持时确认读取的文件路径。

## 多工具复用

建议只维护一份主规范文件，通常是 `AGENTS.md`，再让不同工具复用。

### 软链复用

```bash
ln -s AGENTS.md CLAUDE.md
ln -s AGENTS.md GEMINI.md
```

适合软链稳定可用的环境。

### Claude Code 引用复用

Claude Code 可以在 `CLAUDE.md` 中引用 `AGENTS.md`：

```md
@AGENTS.md
```

如果需要保留 Claude 专属补充，可以这样写：

```md
# CLAUDE.md

@AGENTS.md

## Claude Code 补充规则

- 如需使用 Claude Code 特有能力，可写在这里。
```

## 个人/本地覆盖：当前调研结论

目前没有确认存在跨 Coding Agent 通用的 `.local.md` 约定。

| 工具 | 已确认行为 | 本地/私有覆盖状态 |
|---|---|---|
| Claude Code | 官方文档说明 Claude Code 读取 `CLAUDE.md`，支持 `@path` import，并会加载与 `CLAUDE.md` 同级的 `CLAUDE.local.md`。 | `CLAUDE.local.md` 官方支持，用于个人项目偏好，应加入 `.gitignore`。 |
| Gemini CLI | 官方文档说明 Gemini CLI 使用层级化 `GEMINI.md` context files，并支持 `context.fileName` 配置为字符串或字符串数组。 | 未找到默认支持 `GEMINI.local.md` 的官方说明。可以通过 `context.fileName` 显式配置额外文件名，但这不是默认约定。 |
| AGENTS.md 标准 | agents.md 官网说明了 `AGENTS.md`、嵌套 `AGENTS.md`、软链，以及 Gemini 配置为读取 `AGENTS.md` 的方式。 | 未找到 `AGENTS.local.md` 的官方约定。 |
| OpenAI Codex | Codex/AGENTS.md 支持可从 AGENTS.md 生态文档确认；部分搜索结果提到 `AGENTS.override.md`，但在作为建议前还需要用当前官方 Codex 文档或本地版本进一步确认。 | 不应假设 Codex 会自动加载 `AGENTS.local.md`，除非目标版本或官方文档明确支持。 |
| Cursor | Cursor 属于 AGENTS.md 生态，也有自己的 rules/settings 机制；本次未确认官方 `AGENTS.local.md` 行为。 | 不应假设 `.local.md` 会自动加载。需要个人覆盖时，应使用 Cursor 官方 rules/settings 机制。 |

当前实用建议：

- Claude Code：使用 `CLAUDE.local.md` 存放私有项目偏好。
- Gemini CLI：使用 `~/.gemini/GEMINI.md`、项目 `GEMINI.md`，或通过 `context.fileName` 显式配置额外文件名。
- 通用 `AGENTS.md`：共享规则放在提交版 `AGENTS.md`；不要把 `AGENTS.local.md` 写成会被所有工具自动加载的约定，除非目标工具已确认支持。

## 分层方案

只分两层：

1. **全局工作规范**：跨项目规则。
2. **项目工作规范**：仓库专属事实。

不再把用户级和团队级拆成独立概念层。改用文件位置和工具支持的机制区分：

- 提交到仓库的项目规范文件是团队共享规则。
- 用户级/全局规范文件是个人跨项目偏好，前提是工具支持该位置。
- 本地/私有文件只有在目标工具官方支持或显式配置加载时才推荐使用。

## 许可与致谢

本项目基于 [agents-md](https://github.com/TheRealSeanDonahoe/agents-md) 的理念和结构修改而来。发布衍生作品前，请查看上游项目的许可证和署名要求。
