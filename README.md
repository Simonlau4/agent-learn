# agent-learn

给同时用 Claude Code、Codex、Antigravity、Cursor 等多个 Agent 的人准备的**跨工具会话学习与收尾 Skill**（安装后在对话里用 `/agent-learn` 或直接说“收尾 / 记住这条”即可触发）。

如果你用过 Antigravity 的 `/learn` 或 Claude 的 `/reflect`，很快会遇到三个现实问题：

1. **单端失忆**：`/learn` 把学到的规矩存进 `.gemini/` 或某个工具的私有记忆里，换到 Claude Code 或 Codex 开新窗口照样踩一遍同一个坑。
2. **打断节奏**：每记一条都要停下来批草稿，日常开发嫌烦，干脆关掉不用。
3. **越用越肿**：聊天流水和报错截图全往一个文件末尾堆，两周后变成几百行互相打架的垃圾场，模型根本抓不到重点。

`agent-learn` 把一套跑了几个月的个人沉淀工作流整理出来，只用一份 59 行的指令解决这三件事：

- **写进所有工具都认的共享正本**：按当前工作区自动匹配，有知识库路由就按路由落盘，普通代码库就写进根 `AGENTS.md` / `CLAUDE.md` 或对应 `README.md`，不往各家私有隐藏目录里塞第二份规则。
- **日常直写读回，高风险才停下来**：业务结论、个人偏好、排障坑点写前先读现行版本、写后立刻读回核对，不弹窗打断；修改正在用的 Skill、核心判断或执行 Git 操作时，先给 `diff` 或一句话改法等你拍板。
- **试错只留根因，新条覆盖旧条**：调试试错好几次才跑通时，对比失败尝试提炼出真正起作用的参数或前置条件；新结论直接覆盖被替代的旧条并检查索引入口，用再久也只保留薄薄的有效规则。

## 安装

把仓库里的 `skills/agent-learn/` 放到对应 Agent 的技能目录即可生效；如果你同时在用多个 Agent，直接把本仓库地址贴进 [Skill Pilot](https://github.com/Simonlau4/skill-pilot) 一次勾选启用更省事。

```bash
git clone https://github.com/Simonlau4/agent-learn.git /tmp/agent-learn

# Antigravity 全局技能目录
mkdir -p ~/.gemini/config/skills && cp -R /tmp/agent-learn/skills/agent-learn ~/.gemini/config/skills/agent-learn

# Claude Code 全局技能目录
mkdir -p ~/.claude/skills && cp -R /tmp/agent-learn/skills/agent-learn ~/.claude/skills/agent-learn

# Codex / 多工具通用目录
mkdir -p ~/.agents/skills && cp -R /tmp/agent-learn/skills/agent-learn ~/.agents/skills/agent-learn

# 或者只装进当前项目仓库，跟团队共享
mkdir -p .agents/skills && cp -R /tmp/agent-learn/skills/agent-learn .agents/skills/agent-learn
```

## 怎么用

1. **任务做完顺手收尾**：直接说“收尾”或输入 `/agent-learn`，Agent 会扫一遍这轮对话里的纠正、排障转折和未完结进度，把值得保留的写进对应文件并读回；没东西要留就静默结束。
2. **中途随口点名记一条**：直接敲 `/agent-learn 记住以后这个项目跑单测必须加 --pool=forks`，跳过翻聊天记录，去重后直接落到对应规则文件。
3. **空目录从零搭知识库**：在空白文件夹里说“用 agent-learn 帮我建一个最小本地知识库骨架”，会按内置模板生成 `AGENTS.md` 和 `Knowledge/`、`Memory/`、`Workbench/` 三个分区。

## 仓库结构

```text
agent-learn/
├── skills/agent-learn/
│   ├── SKILL.md                           # 跨 Agent 通用收尾与会话学习指令
│   ├── agents/openai.yaml                 # 接口元数据与默认触发提示
│   └── references/bootstrap-template.md   # 从零起步的最小三分区 AGENTS.md 模板
├── evals/test-prompts.json                # 12 条典型场景回归测试用例
├── LICENSE                                # MIT License
└── README.md
```

## License

[MIT](LICENSE)
