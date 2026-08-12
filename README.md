# 说人话（shuorenhua）

去除中文文本里的 AI 写作痕迹，让文字读起来像一个具体的人写的。一个纯 Markdown 的 agent skill，适用于 Claude Code、OpenCode 及任何支持 skill 式指令的 harness。

「说人话」检测并改写 33 种中文 AI 写作模式：官腔套话、意义拔高、虚假引用、假具体、排比堆砌、AI 高频词（赋能/深耕/抓手）、机械连接词、翻译腔、格式模板化、聊天客套等。也可用于英译中去翻译腔。

## 为什么再造一个轮子

英文的 [blader/humanizer](https://github.com/blader/humanizer) 很好，但它的词表、标点规则和示例全是英文本位的——直接对中文使用，弯引号规则会把正确的中文标点改错，em dash 禁令会误伤合法的中文破折号，而中文真正的重灾区（赋能体、报告腔、四字格滥用、「的」字连珠）它一个都不认识。

社区已有的中文版各有所长，也各有硬伤：

| 项目 | 可取之处 | 硬伤 |
|------|----------|------|
| [op7418/Humanizer-zh](https://github.com/op7418/Humanizer-zh) | 传播最广，基础模式的中文译法 | 译自旧版（24/33 条），示例改写会凭空编造事实 |
| [kevintsai1202/Humanizer-zh-TW](https://github.com/kevintsai1202/Humanizer-zh-TW) | 繁中/台湾用语本地化 | 同上（op7418 分支） |
| [0xtresser/cn-humanizer](https://github.com/0xtresser/cn-humanizer) | 中文原生词表分级、统计信号、翻译模式 | 示例编造更激进（发明来源和数字） |
| [ZeddYu/humanizer_zh](https://github.com/ZeddYu/humanizer_zh) | 不编造原则、假具体检测、分场景策略 | 词表广度不足，缺误报防线 |

「说人话」以 ZeddYu 的纪律为骨架，融合 0xtresser 的词表思路，并从 blader v2.9 回补了所有中文版都缺失的部分：

- **不编造纪律**：改写不得添加原文没有的事实、名字、数字、日期、引用。缺细节就问用户，不许补一个像样的。所有示例本身遵守这条规则。
- **误报防线**：明确列出什么*不该*标记——单独的连接词、正常的四字格、公文文体的套话、引用原文里的特征词。找聚集，不看孤例。
- **人类信号保护**：罕见细节、未解决的情绪、真实的跑题——看到这些要少动，过度编辑会毁掉文章里最像人的部分。
- **中文标点立场**：全角标点、「」引号、合法的破折号是正确的中文，不是 AI 痕迹。这条和英文规则相反。
- **草稿 → 审计 → 定稿**流程：审计问两个问题——「哪里还是一眼 AI」和「有没有编造」。
- **分场景策略**：技术报告、调研论文、博客、项目介绍、简历分别怎么改。

## 安装

### Claude Code

```bash
mkdir -p ~/.claude/skills/shuorenhua
cp SKILL.md ~/.claude/skills/shuorenhua/
cp -r references ~/.claude/skills/shuorenhua/
```

或克隆后软链：

```bash
git clone https://github.com/<owner>/shuorenhua.git
ln -s "$(pwd)/shuorenhua" ~/.claude/skills/shuorenhua
```

### 其他 harness

运行时产物就是 `SKILL.md`（外加 `references/` 词表），放进你的 harness 找 skill 的目录即可。

## 用法

```
/shuorenhua

[贴上要改的文本]
```

```
把 docs/发布公告.md 里的 AI 味去掉
```

带上你自己的写作样本，可以让改写贴近你的文风而不是通用的「干净」：

```
/shuorenhua

这是我平时的写作风格：
[你自己写的两三段]

帮我改这段：
[AI 生成的文本]
```

英译中：

```
把这段英文翻成自然的中文，不要翻译腔：
[英文原文]
```

## 三种输出

- **只要结果**：说「直接改」，只给定稿
- **改写 + 说明**（默认）：定稿加 3–5 条关键改动
- **诊断 + 评分**：说「诊断」或「打分」，附五维评分（信息密度/自然度/中文语感/去模板化/风格一致性）

## 致谢

- [blader/humanizer](https://github.com/blader/humanizer)（MIT）—— 方法论源头，基于 [Wikipedia:Signs of AI writing](https://en.wikipedia.org/wiki/Wikipedia:Signs_of_AI_writing)
- [ZeddYu/humanizer_zh](https://github.com/ZeddYu/humanizer_zh)（MIT）—— 本项目骨架
- [0xtresser/cn-humanizer](https://github.com/0xtresser/cn-humanizer) —— 词表分级与统计信号思路（内容为本项目重写）
- [op7418/Humanizer-zh](https://github.com/op7418/Humanizer-zh)（MIT）、[kevintsai1202/Humanizer-zh-TW](https://github.com/kevintsai1202/Humanizer-zh-TW)（MIT）—— 基础模式中文译法参考

## 版本

- **0.1.0** —— 首次发布：33 种模式、分级词表、误报防线、人类信号保护、不编造纪律、分场景策略、三种输出模式

## License

MIT
