# INSTALL.md —— 给 agent 的自动安装指引

本文件写给 AI agent。如果你是一个 coding agent，被用户要求安装本 skill，按下面步骤执行，装完向用户汇报。人类用户想手动安装的话，看 [README.md](README.md) 的安装一节即可。

## 你要安装什么

运行时产物只有 4 个文件。目标：把它们放进你的 harness 的 skill 目录下名为 `shuorenhua` 的文件夹，保持目录结构（SKILL.md 用相对路径引用 references/）：

```
shuorenhua/
├── SKILL.md
└── references/
    ├── vocabulary.md
    ├── model-signatures.md
    └── scenarios.md
```

README.md、INSTALL.md 不是运行时产物，不用复制。

## 第 0 步：拿到源文件

- 你在本仓库的**本地 clone** 里读到这个文件 → 源文件就在旁边，直接复制。
- 你通过 **URL** 读到这个文件 → 先 `git clone` 本仓库到临时目录；环境里没有 git 就按上面清单逐个下载 4 个文件的 raw 版本，注意保持目录结构。

## 第 1 步：确定你是谁、装到哪

先判断你运行在哪个 harness 里（看你自己的系统提示、配置目录、CLI 名称），再选安装范围。用户明确指定了范围就按用户的；否则默认装全局（对所有项目生效），Trae 例外（见下）。

| Harness | 全局目录 | 项目目录 |
|---|---|---|
| Claude Code | `~/.claude/skills/shuorenhua/` | `<项目>/.claude/skills/shuorenhua/` |
| Codex CLI | `~/.codex/skills/shuorenhua/` | `<项目>/.codex/skills/shuorenhua/` |
| OpenClaw | `~/.openclaw/skills/shuorenhua/` | `<workspace>/skills/shuorenhua/` |
| Trae Code | 国内版 `~/.trae-cn/skills/shuorenhua/`，国际版 `~/.trae/skills/shuorenhua/` | `<项目>/.trae/skills/shuorenhua/`（官方主推，优先用这个） |
| Trae Work | 无公开文档，走第 2 步探测流程 | 同左 |

**Trae 注意事项：**

- Trae Code（IDE）优先装项目级 `.trae/skills/`，这是官方文档主推路径；装完需要重启 Trae 或开新会话才生效。
- **Trae Work 的 skill 目录可能与 Trae Code 不同**，截至 2026-08 没有公开文档确认。不要想当然套用 Trae Code 的路径——按第 2 步探测：查你自己的配置目录（如 `~/.trae-work/` 之类）下是否有 skills 目录或已安装的其他 skill，找不到就问用户产品内的 skill 管理入口在哪。

## 第 2 步：不在上表里，或表中路径探测失败

按顺序尝试：

1. **查自己**：你的文档或配置里是否声明支持 Agent Skills 标准（`SKILL.md` + YAML frontmatter 的目录格式）？声明了就用它指定的目录。
2. **探测**：看这些位置是否已存在其他 skill（存在即正确目录）——你的配置目录下的 `skills/`（如 `~/.<你的名字>/skills/`）、跨 agent 共享目录 `~/.agents/skills/`、当前项目的 `.agents/skills/`。
3. **Fallback（harness 不支持 skill 机制）**：把 4 个文件放到一个稳定位置（建议 `~/agent-skills/shuorenhua/`，先征得用户同意），然后在你的指令文件（`AGENTS.md`、rules 文件等）里追加一条：

   > 当用户要求「去 AI 味 / 说人话 / 中文润色 / 去翻译腔 / 去豆包味 / 去 DeepSeek 味」时，读取 `<安装路径>/SKILL.md` 并严格按其执行。

4. **拿不准就问用户**，不要猜一个路径静默写入。

## 第 3 步：复制并验证

1. 创建目标目录，复制 4 个文件，保持 `references/` 子目录结构。
2. 验证：4 个文件都存在；`SKILL.md` frontmatter 里 `name: shuorenhua` 和 `description` 完整无损。
3. 多数 harness 在**新会话**才加载新 skill，提醒用户。
4. 向用户汇报：安装到了哪个绝对路径、属于哪个范围（全局/项目）、怎么触发——说「去 AI 味」「说人话」等触发词，或（支持 slash 命令的 harness）`/shuorenhua`。

## 卸载

删掉安装目录下的 `shuorenhua/` 文件夹。用 Fallback 方式安装的，还要删掉指令文件里追加的那行。

## 已知路径的依据

Claude Code、Codex CLI、OpenClaw、Trae 均支持同一套 Agent Skills 标准（SKILL.md 格式互通）。路径来源：Codex 官方技能目录约定（`~/.codex/skills` / `.codex/skills`）、OpenClaw 官方文档（`~/.openclaw/skills`，workspace `skills/`）、Trae 官方文档 docs.trae.cn/ide_skills（`.trae/skills/`、`~/.trae-cn/skills`）。路径信息截至 2026-08，若失效以各家最新文档为准。
