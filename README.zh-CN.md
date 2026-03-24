<p align="center">
  <img src="https://img.shields.io/badge/agent--skill-v1.0-blue" alt="version" />
  <img src="https://img.shields.io/badge/license-MIT-green" alt="license" />
  <img src="https://img.shields.io/badge/兼容-Claude_Code | Cursor | Windsurf | Codex | Gemini_CLI-purple" alt="compatible" />
</p>

<h1 align="center">🏗️ Agent-Friendly Codebase</h1>

<p align="center">
  <strong>别再给 AI 当保姆了。让 AI 帮你管项目。</strong>
</p>

<p align="center">
  一个文件，装一次。以后每次 AI 对话——不管用什么工具——<br/>
  都像同一个靠谱的高级工程师在持续工作：不忘事、不偷懒、不留烂摊子。
</p>

<p align="center">
  <a href="./README.md">English</a> | 中文
</p>

---

## 你一定经历过这种事

你用 Cursor / Claude / Copilot 做一个项目。一开始挺好，然后：

😩 **第 2 次对话**："我们用的什么框架来着？"  
😩 **第 5 次对话**：4 个文件夹里有 4 份功能差不多的代码  
😩 **第 8 次对话**：你不敢加新功能了，怕加一个崩三个  
😩 **第 12 次对话**：你发现自己还不如直接手写代码  

**这就是 Vibe Coding 熵增。** 每次 AI 对话都给项目加一点混乱。几轮下来，你的项目变成一锅意大利面——人看不懂，AI 也看不懂。

这不是 AI 的锅。**是你的项目什么都没给它**——没有结构、没有规矩、不记得昨天干了什么。所以它每次都在即兴发挥。每一次。

## 解法：一个文件

`SKILL.md` 是一个你丢进 AI 工具里的文件。它从根本上改变 AI 处理你项目的方式——**自动的、永久的、不需要你再做任何事。**

装完之后的变化：

```
装之前（Vibe Coding）                    装之后（Agent-Friendly）
──────────────────                      ────────────────────
❌ 每次对话 AI 都失忆                    ✅ 自动读上次的交接记录
❌ 代码想放哪放哪                        ✅ Lint 规则强制放对位置
❌ 不写测试，或者"回头再补"              ✅ 必须先写测试，再写代码
❌ 改一个功能崩三个                      ✅ 每次改动最多 2-3 个文件
❌ git log 全是 "fix" "update"          ✅ "domain(订单): 新增折扣策略"
❌ 每次都要重新解释项目                   ✅ AI 自动读项目合同
❌ 换个 AI 工具就得重来                  ✅ 任何工具都读同一份合同
```

## 安装（30 秒）

就一个文件：[`SKILL.md`](SKILL.md)。放到你 AI 工具读规则的地方。

| 工具 | 怎么装 |
|------|--------|
| **Claude Code** | `mkdir -p .claude/skills && cp SKILL.md .claude/skills/agent-friendly-codebase.md`<br/>或者直接重命名为项目根目录的 `CLAUDE.md` |
| **Cursor** | 把内容复制到 `.cursor/rules/agent-friendly.mdc`<br/>或者粘贴到 **设置 → Rules for AI** |
| **Windsurf** | 把内容复制到项目根目录的 `.windsurfrules` |
| **CodeBuddy** | 把内容复制到 `.codebuddy/rules/` 或通过界面创建项目规则 |
| **Codex / Gemini CLI** | 放到对应平台的 skill 目录（看平台文档） |
| **其他 AI 工具** | 把内容粘贴到系统提示词 / 项目规则文件里 |

**完事了。** 开一个新对话，开始干活。

## 装完以后会发生什么

### 第一次：AI 自动搭好你的项目

你说：*"用 Python 帮我做一个任务管理 API"*

AI 会自动（不需要你吩咐）：

```
1. 创建干净的文件夹结构     — domain/ app/ adapters/
2. 生成 AGENTS.md           — 项目合同，写的是真实信息
3. 创建 Makefile            — make test / lint / run
4. 添加 Lint 规则           — 代码放错位置直接报错
5. 创建 .agent/ 目录        — 会话交接记录
6. 验证一切正常             — make bootstrap && make test ✅
7. 然后才开始写你要的功能    — 先写测试，再写代码
```

### 以后每次对话

```
对话开始：
  → AI 读项目合同（知道这是什么项目）
  → AI 读交接记录（知道上次干了什么）
  → AI 跑测试（确认项目是好的）
  → 直接开始干活——不废话

对话结束：
  → AI 写交接记录（下次接着干）
  → 下一次对话（哪怕换了 AI 工具）→ 无缝衔接
```

### 每次改代码

```
1. 理解   → 这个改动属于哪一层？
2. 测试   → 先写一个会失败的测试
3. 实现   → 写刚好够通过测试的代码
4. 验证   → make test ✅  make lint ✅
5. 提交   → domain(定价): 新增折扣策略
6. 记录   → 更新交接记录
```

## AI 会遵守的五条铁律

| 铁律 | 大白话解释 |
|------|-----------|
| ⚖️ **先写测试** | AI 写任何代码之前必须先写测试。如果不小心先写了代码 → 删掉，从测试重来。 |
| 📐 **最小改动** | 每次改动最多动 3 个文件。超过 5 个 → 说明设计有问题，先修设计。 |
| 🗂️ **放对位置** | 每个文件只有一个正确的位置。业务逻辑 → `domain/`。数据库 → `adapters/`。没有例外。 |
| ✅ **拿证据说话** | AI 必须跑 `make test` 和 `make lint` 才能说"做完了"。"我觉得没问题"不算。 |
| 📝 **写交接记录** | 结束前必须写：做了什么、没做完什么、下次该做什么。下一个 AI 读了就能接着干。 |

## "我不懂什么六边形架构"

**你不需要懂。** 这正是这个 Skill 的意义。

你只管说"帮我做 XXX"，AI 来处理架构。你只需要知道：

- **`domain/`** = 业务规则（"这东西实际上是干什么的"）
- **`app/`** = 工作流程（"按什么顺序干"）
- **`adapters/`** = 连接件（数据库、API、网页服务器）

剩下的 AI 知道。Lint 规则会阻止它搞错。**你只管想要什么功能，结构的事交给系统。**

## 什么语言都行

| 你的技术栈 | 它怎么适配 |
|-----------|-----------|
| TypeScript / Node.js | npm, jest/vitest, ESLint 边界规则 |
| Python | pip/poetry, pytest, import-linter |
| Go | go mod, go test, depguard |
| React / Vue / Next.js | 功能模块化架构（不是六边形） |
| 全栈 | 后端六边形 + 前端功能模块化 |
| 已有的混乱项目 | 渐进式添加结构，不会一次性重写 |
| Monorepo | 每个包有自己的合同 |
| Windows | 用 npm scripts 或 Justfile 代替 Makefile |

## 和其他工具有什么区别

| 工具 | 它做什么 | 我们的区别 |
|------|---------|-----------|
| [Superpowers](https://github.com/obra/superpowers) | 教 AI **怎么写代码**（TDD 流程、计划、调试） | 我们教 AI **代码放哪 + 怎么记住上次做了什么**。**两个一起用效果最好。** |
| [AGENTS.md](https://agents.md) | 一个文件格式标准 | 我们**自动生成**真实项目信息的 AGENTS.md + 完整方法论 |
| `.cursorrules` / `CLAUDE.md` | 你手写的静态配置 | 我们给 AI 一个**自动创建和维护**这些文件的系统 |
| 代码审查机器人 | 代码写完了再检查 | 我们在代码写出来**之前**就阻止错误（架构 + TDD） |

## 终极检验

> *"如果你删掉所有聊天记录，给一个全新的 AI 只看你的代码库——它能成功加一个功能吗？"*

- **能** → 你的项目是 Agent-Friendly 的。项目本身在承担工作。
- **不能** → 你在承担工作。而且每次对话你都得重新承担一遍。

**这个 Skill 就是让答案变成"能"。**

## 常见问题

<details>
<summary><strong>会不会拖慢开发速度？</strong></summary>

不会。先写测试多花 30 秒。不写测试，以后每次 AI 碰这段代码都要花 30 分钟调试。TDD 更快，只是当下感觉不像。
</details>

<details>
<summary><strong>我的项目已经很乱了怎么办？</strong></summary>

这个 Skill 处理了这种情况。它告诉 AI：
1. 立刻添加 AGENTS.md + Makefile + .agent/（零风险，纯新增文件）
2. 所有新代码按正确结构写
3. 旧代码每次对话重构一个模块

不会试图一天之内重写所有东西。
</details>

<details>
<summary><strong>我想用不同的架构怎么办？</strong></summary>

Fork 了改。核心原则（隔离、先写测试、会话记忆）适用于任何干净的架构。默认方案（后端六边形、前端功能模块化）是因为它们被证明是对 AI 最友好的。
</details>

<details>
<summary><strong>能同时用多个 AI 工具吗？</strong></summary>

能。所有上下文都在文件里（AGENTS.md、会话日志、决策记录）——不在聊天历史里。任何平台的 AI 都读同一套文件，遵循同一套规则。
</details>

<details>
<summary><strong>我不是程序员，能用吗？</strong></summary>

这正是为你设计的。你不需要理解架构。装上 Skill，告诉 AI 你想做什么，它来处理工程部分。这个 Skill 就是让 AI 靠谱到你可以长期信任它来管你的项目。
</details>

## 核心理念

> **代码库是一个接口。让它不可能被误解。**

AI 不需要变聪明。你的代码库不再模糊就够了。

当架构是显式的、测试是强制的、每次对话都留交接记录——AI 就不再即兴发挥了。它在执行一个系统。这就是 demo 和真正产品之间的区别。

## 灵感来源

- [Agent-Friendly Codebase](https://blog.heliomedeiros.com/posts/2025-08-07-agent-friendly-codebase/) — Helio Medeiros
- [Superpowers](https://github.com/obra/superpowers) — Jesse Vincent
- [AGENTS.md 标准](https://agents.md/) — Linux Foundation
- 六边形架构 — Alistair Cockburn

## 贡献

发现问题？有更好的方案？欢迎 PR。

标准很简单：**你的改动能让下一次 AI 对话更靠谱吗？** 能就合。

## 许可证

[MIT](LICENSE) — 随便用、随便改、随便卖。能让你的项目免于 Vibe Coding 熵增，就够了。

---

<p align="center">
  <strong>代码库是接口。让它不可能被误解。</strong>
</p>
