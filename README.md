<p align="center">
  <img src="https://img.shields.io/badge/agent--skill-v1.0-blue" alt="version" />
  <img src="https://img.shields.io/badge/license-MIT-green" alt="license" />
  <img src="https://img.shields.io/badge/works_with-Claude_Code | Cursor | Windsurf | Codex | Gemini_CLI-purple" alt="compatible" />
</p>

<h1 align="center">🏗️ Agent-Friendly Codebase</h1>

<p align="center">
  <strong>Stop babysitting your AI. Make it babysit your codebase.</strong>
</p>

<p align="center">
  One file. Install once. Every AI coding session — on any platform — <br/>
  works like a senior engineer who never forgets, never cuts corners, and never leaves a mess.
</p>

<p align="center">
  English | <a href="./README.zh-CN.md">中文</a>
</p>

---

## You know this feeling

You're building something with Cursor / Claude / Copilot. It's going great. Then:

😩 **Session 2**: "What framework are we using again?"  
😩 **Session 5**: There's code in 4 different folders that does the same thing  
😩 **Session 8**: You're afraid to ask for a new feature because it might break everything  
😩 **Session 12**: You'd be faster just writing the code yourself  

**This is Vibe Coding decay.** Every AI session adds a little chaos. After a while, your project is a spaghetti factory that no human or AI can navigate.

It's not the AI's fault. **Your project gives it nothing to work with** — no structure, no rules, no memory of what happened yesterday. So it improvises. Every. Single. Time.

## The fix: one file

`SKILL.md` is a single file you drop into your AI coding tool. It rewires how the AI works on your project — **automatically, permanently, without you doing anything else.**

Here's what changes:

```
BEFORE (Vibe Coding)                    AFTER (Agent-Friendly)
─────────────────────                   ──────────────────────
❌ AI forgets everything each session   ✅ Reads handoff notes from last session
❌ Code goes wherever AI feels like     ✅ Architecture enforced by lint rules
❌ No tests, or "I'll add them later"   ✅ Writes test FIRST, then code. Always.
❌ Changes break random stuff           ✅ Changes stay in 2-3 files max
❌ "fix stuff" commit messages          ✅ "domain(orders): add discount policy"
❌ You explain the project every time   ✅ AI reads project contract auto
❌ Switch AI tool = start over          ✅ Any tool reads the same contract
```

## Install (30 seconds)

It's one file: [`SKILL.md`](SKILL.md). Put it where your AI tool reads rules.

<table>
<tr><td><strong>Claude Code</strong></td><td>

```bash
mkdir -p .claude/skills && cp SKILL.md .claude/skills/agent-friendly-codebase.md
```
Or simply rename to `CLAUDE.md` in project root.
</td></tr>
<tr><td><strong>Cursor</strong></td><td>

Copy content into `.cursor/rules/agent-friendly.mdc`  
or paste into **Settings → Rules for AI**
</td></tr>
<tr><td><strong>Windsurf</strong></td><td>

Copy content into `.windsurfrules` in project root
</td></tr>
<tr><td><strong>CodeBuddy</strong></td><td>

Copy content into `.codebuddy/rules/` or create as a project rule
</td></tr>
<tr><td><strong>Codex / Gemini CLI</strong></td><td>

Place in agent's skill directory (see platform docs)
</td></tr>
<tr><td><strong>Any other AI tool</strong></td><td>

Paste content into your system prompt / project rules file
</td></tr>
</table>

**That's it. You're done.** Open a new chat and start building.

## What happens next

### First time: AI sets up your project automatically

You say: *"Build me a task management API in Python"*

The AI (without you asking) will:

```
1. Create clean folder structure     — domain/ app/ adapters/
2. Generate AGENTS.md                — project contract with REAL info
3. Create Makefile                   — make test / lint / run  
4. Add lint rules                    — blocks code in wrong folders
5. Set up .agent/                    — session handoff directory
6. Verify everything works           — make bootstrap && make test ✅
7. THEN start building your feature  — with tests first
```

### Every session after that

```
Session start:
  → AI reads project contract
  → AI reads "what happened last time"  
  → AI runs tests to make sure nothing's broken
  → AI starts working — no questions asked

Session end:
  → AI writes handoff notes for next session
  → Next time (even with different AI tool) → picks up seamlessly
```

### Every code change

```
1. Understand  → What layer does this belong to?
2. Test (RED)  → Write failing test first
3. Code (GREEN)→ Minimum code to pass
4. Verify      → make test ✅  make lint ✅  
5. Commit      → domain(pricing): add discount policy
6. Log         → Update handoff notes
```

## The Five Laws your AI will follow

| Law | What it means (in plain English) |
|-----|----------------------------------|
| ⚖️ **Test First** | AI writes a test before writing code. Every time. If it didn't → it deletes the code and starts over. |
| 📐 **Minimal Change** | Each change touches 3 files max. If more → something's wrong with the design, not the feature. |
| 🗂️ **Right Place** | Every file has exactly one correct folder. Business logic → `domain/`. Database stuff → `adapters/`. No exceptions. |
| ✅ **Prove It Works** | AI runs `make test` and `make lint` before calling anything "done". Not "I think it works" — actual proof. |
| 📝 **Leave Notes** | Before ending, AI writes what it did, what's left, and what to do next. The next session reads this first. |

## "I don't know what hexagonal architecture means"

**You don't need to.** That's the point.

You just say *"build me X"* and the AI handles the architecture. All you need to know:

- **`domain/`** = the business rules (the "what it actually does" part)
- **`app/`** = the workflows (the "in what order" part)  
- **`adapters/`** = the connections (database, APIs, web server)

The AI knows the rest. The lint rules prevent it from messing up. **You focus on what you want to build. The structure takes care of itself.**

## Works with everything

| Your stack | How it adapts |
|---|---|
| TypeScript / Node.js | npm, jest/vitest, ESLint boundary rules |
| Python | pip/poetry, pytest, import-linter rules |
| Go | go mod, go test, depguard rules |
| React / Vue / Next.js | Feature-based architecture (not hexagonal) |
| Fullstack | Backend hexagonal + Frontend feature-based |
| Existing messy project | Adds structure gradually, doesn't rewrite |
| Monorepo | Each package gets its own contract |
| Windows | npm scripts or Justfile instead of Makefile |

## How is this different from...

| Tool | What it does | How we're different |
|------|-------------|---------------------|
| [Superpowers](https://github.com/obra/superpowers) | Teaches AI **how to code** (TDD workflow, planning) | We teach AI **where to put code + how to remember** (architecture, session memory). **Use both together for best results.** |
| [AGENTS.md](https://agents.md) | A file format standard | We **auto-generate** AGENTS.md with real project info + add the complete methodology |
| `.cursorrules` / `CLAUDE.md` | Static config files you write manually | We give the AI a system that **creates and maintains** these automatically |
| Code review bots | Check code after it's written | We prevent bad code **before it's written** (architecture + TDD) |

## The test that matters

> *"If you delete all chat history and give a brand-new AI session nothing but your codebase — can it successfully add a feature?"*

- **Yes** → Your project is agent-friendly. The repo does the work.
- **No** → You're doing the work. And you'll do it again. And again. Every session.

**This skill makes the answer "yes".**

## FAQ

<details>
<summary><strong>Will this slow me down?</strong></summary>

No. Writing a test first takes 30 seconds. Not writing it costs 30 minutes of debugging — every time any AI touches that code in the future. TDD is faster. It just doesn't feel like it in the moment.
</details>

<details>
<summary><strong>What if my project already exists and it's messy?</strong></summary>

The skill handles this. It tells the AI to:
1. Add AGENTS.md + Makefile + .agent/ immediately (zero risk)
2. Put all NEW code in the correct structure
3. Refactor old code incrementally — one module per session

It doesn't try to rewrite everything on day one.
</details>

<details>
<summary><strong>What if I want a different architecture?</strong></summary>

Fork and adapt. The principles (isolation, test-first, session memory) work with any clean architecture. The defaults (hexagonal backend, feature-based frontend) are there because they're proven to be the most AI-friendly.
</details>

<details>
<summary><strong>Does this work with multiple AI tools on the same project?</strong></summary>

Yes. All context lives in files (AGENTS.md, session logs, decision logs) — not in chat history. Any AI on any platform reads the same files and follows the same rules.
</details>

<details>
<summary><strong>I'm not a programmer. Can I use this?</strong></summary>

That's exactly who this is for. You don't need to understand the architecture. Install the skill, tell the AI what you want to build, and it handles the engineering. The skill is what makes the AI reliable enough to trust with your project long-term.
</details>

## Inspired by

This skill stands on the shoulders of ideas from:

- [Agent-Friendly Codebase](https://blog.heliomedeiros.com/posts/2025-08-07-agent-friendly-codebase/) — Helio Medeiros
- [Superpowers](https://github.com/obra/superpowers) — Jesse Vincent
- [AGENTS.md Standard](https://agents.md/) — Linux Foundation
- Hexagonal Architecture — Alistair Cockburn

## Contributing

Found a bug? Have a better pattern for a specific language? PRs welcome.

The bar: **does your change make the next AI session more reliable?** If yes, ship it.

## License

[MIT](LICENSE) — use it, fork it, sell it. If it saves your project from Vibe Coding decay, that's all that matters.

---

<p align="center">
  <strong>The repo is the interface. Make it impossible to misunderstand.</strong>
</p>
