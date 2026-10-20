# REPL Works

**AI-Native Product Development Framework**

Keep your AI-built projects from going off the rails.

Planning with a conversational AI and coding with a coding AI is already the
right approach. REPL Works is the document methodology that keeps a project
from falling apart — through completion, and through every fix and improvement
after.

Six standard documents in Git hold your project's purpose, tech rules, and
structure, so a new chat, a different AI, or a new teammate can read them and
pick up exactly where work left off.

## The Problem

- Every new chat, you re-explain the project from scratch.
- The AI writes code in the wrong version or folder because it doesn't know your rules.

## How It Works

Humans and a conversational AI (ChatGPT, Claude) write the documents together.
A coding AI (Codex, Claude Code) verifies them before development, then updates
code and docs together, one task per commit. You review and commit.

| Stage      | Document             | Purpose             |
| ---------- | -------------------- | ------------------- |
| For humans | `IDEAS.md`           | Idea validation     |
|            | `PITCHING_SCRIPT.md` | Project positioning |
| For AI     | `PRODUCT_SPEC.md`    | Product definition  |
|            | `TECH_STACK.md`      | Technical rules     |
|            | `ARCHITECTURE.md`    | System design       |
|            | `TASKS.md`           | Execution plan      |

Git is the single source of truth: documents describe the current state,
commits record how it got there.

## Tools

| Tool                                                    | What it does                                     | Install                                         |
| ------------------------------------------------------- | ------------------------------------------------ | ----------------------------------------------- |
| [repl-cli](https://github.com/replworks/repl-cli)       | Keeps AI work on track against your documents    | `brew install --cask replworks/tap/repl-cli`    |
| [ai-issue](https://github.com/replworks/ai-issue)       | Turn AI conversations into GitHub Issues         | `brew install --cask replworks/tap/ai-issue`    |
| [coolrestore](https://github.com/replworks/coolrestore) | Restore Coolify backups safely after an incident | `brew install --cask replworks/tap/coolrestore` |

Linux users: see [apt.repl.net](https://apt.repl.net).

## Built with REPL Works

[WIFI Note](https://wifinote.net) · [Image Forge](https://if.repl.net) · [repl.net](https://www.repl.net)

## Core Philosophy

**Models change. Projects survive.**

What overcomes the limits of AI models isn't a bigger context window. It's
durable project memory, kept in Git and standard documents.

---

[Docs](https://www.repl.net) · [한국어](https://www.repl.net) · works@repl.net · MIT License
