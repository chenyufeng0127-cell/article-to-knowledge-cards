# make-knowledge-cards

把文章转成知识卡片的开源 Agent Skill：将用户提供的文章自动提炼成 5~8 张可复习的知识卡片。

## 项目解决了什么问题

读完文章常常"读了就忘"，长文抓不住重点、想复习却无从下手。本项目把零散文章提炼成结构化知识卡片——一张卡片只讲一个知识点，每张含**标题、核心知识、简明解释、例子或自测**四要素，让知识更容易记住、回顾和掌握，而不是一读了之、过过"脑瘾"。

## 主要功能

- **输入灵活**：直接粘贴文章文本，或提供本地 `.md` / `.txt` 文件路径。
- **结构化输出**：每张卡片包含标题、核心知识、简明解释、例子或自测四个要素。
- **一张卡片一个知识点**：卡与卡之间不重叠。
- **自动去重**：同一知识点重复出现只保留一张。
- **宁少勿多**：有效知识点不足 5 个时按实际数量输出并说明，不强行凑数。
- **忠实原文**：不添加原文没有的信息、数据、结论。
- **语言跟随原文**：原文是中文则输出中文。
- **边界**：不支持网页抓取、PDF、Anki 导入、图形界面。

## 安装方法

本项目是一个符合 Anthropic/OpenAI Skills 规范的 Agent Skill，纯指令、不含脚本，无需编译或运行服务。只需把技能目录放进 agent 的 skills 库：

```bash
# 复制到 agent 的 skills 库（以 DeepSeek Harness 的 ~/.agents/skills 为例）
cp -r skills/make-knowledge-cards ~/.agents/skills/

# 或使用软链接，便于跟随仓库更新
ln -s "$PWD/skills/make-knowledge-cards" ~/.agents/skills/make-knowledge-cards
```

其他 agent（Claude Code / Codex 等）同样适用：把 `make-knowledge-cards/` 复制到对应的 skills 目录（如 `~/.claude/skills/`、`$CODEX_HOME/skills`）。依赖：无。

## 使用方法

在支持 `$skill` 调用的 agent 里，说「用 $make-knowledge-cards 把这篇文章整理成知识卡片」，然后提供内容：

1. 直接粘贴文章文本；或
2. 提供本地 `.md` / `.txt` 文件路径。

agent 会按 `SKILL.md` 的工作流程输出 5~8 张 Markdown 知识卡片，可直接粘贴到笔记软件。

## 输入输出示例

**输入**（粘贴文本或文件路径）：

> 番茄工作法：把工作切成 25 分钟一个的「番茄」，一个番茄内只做一件事；结束后休息 5 分钟，每完成四个番茄休息 15~30 分钟。它解决的核心问题是专注力分散——把「要专注」变成一个可执行的短周期承诺。

**输出**：

```markdown
## 卡片 1｜番茄工作法：25 分钟专注一个周期

**核心知识**：把工作切成 25 分钟一个的「番茄」，一个番茄内只做一件事；结束后休息 5 分钟，每完成四个番茄休息 15~30 分钟。

**简明解释**：把「要专注」变成可执行的短周期承诺，降低分心门槛。

**例子/自测**：完成 4 个番茄后应休息多久？答案：15~30 分钟。
```

## 项目结构

```
skills/make-knowledge-cards/
├── SKILL.md           # 技能指令（工作流程、硬性规则、输出格式）
└── agents/
    └── openai.yaml    # UI 元数据与默认提示词
```

## 开发与验证

- 初始化（官方 skill-creator 工具）：`init_skill.py make-knowledge-cards --path skills`
- 结构验证：`quick_validate.py skills/make-knowledge-cards`（输出 "Skill is valid!"）
- 已用 4 类文章测试（技术概念、实用方法、短文信息不足、重复内容去重），并另经独立子代理前向测试验证。
