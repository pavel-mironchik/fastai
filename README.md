# ⚡️ Fast AI 🤖

Fast AI is a blazing-fast AI coding framework for solo developers and small teams. It shines on small-to-medium projects because it enforces a simple three-step rhythm—**brief → plan → execute**—that keeps quality high without wasting time.

## Why Fast AI?

- **Opinionated workflow.** Every task follows the same three phases, so nothing important gets skipped.
- **Shared automation.** Commands live in your repo and can be reused (or improved) by the whole team.
- **Cross-agent support.** Works with both Gemini CLI custom commands and GPT Codex slash prompts.
- **Language-indifferent.** Templates are written in English for consistency, but every command automatically detects the user's language, asks questions in it, and writes briefs and plans in that same language.

---

## Getting Started

### 1. Install the Gemini CLI commands

Copy everything from the `gemini/` folder into your local Gemini commands folder so they show up as `/fastai-create-brief`, `/fastai-create-plan`, and `/fastai-execute-plan`.

```bash
mkdir -p ~/.gemini/commands/fastai
cp gemini/*.toml ~/.gemini/commands/fastai/
```

> Prefer symlinks? Run `ln -s "$PWD/gemini/"* ~/.gemini/commands/fastai/` instead of copying. This keeps the commands editable inside the repo while exposing the latest versions to Gemini CLI automatically.

**Important details**

- Gemini CLI loads commands on startup. Restart the CLI (or open a new session) after copying so the new `/fastai-*` commands appear in the slash menu.
- Commands inherit their names from the file path. Placing them under `~/.gemini/commands/fastai` produces the `/fastai-create-brief` style names we want.
- If you already have commands with those names, back them up before copying to avoid overwriting them.

### 2. Install the GPT Codex prompts

Codex CLI looks for Markdown prompts directly under `~/.codex/prompts`. Copy the files from `codex/` into that directory and add the `fastai-` prefix so they become `/fastai-create-brief`, `/fastai-create-plan`, and `/fastai-execute-plan`.

```bash
mkdir -p ~/.codex/prompts
cp codex/create-brief.md ~/.codex/prompts/fastai-create-brief.md
cp codex/create-plan.md ~/.codex/prompts/fastai-create-plan.md
cp codex/execute-plan.md ~/.codex/prompts/fastai-execute-plan.md
```

> Prefer symlinks? Run:
>
> ```bash
> ln -s "$PWD/codex/create-brief.md" ~/.codex/prompts/fastai-create-brief.md
> ln -s "$PWD/codex/create-plan.md" ~/.codex/prompts/fastai-create-plan.md
> ln -s "$PWD/codex/execute-plan.md" ~/.codex/prompts/fastai-execute-plan.md
> ```
>
> Symlinking keeps the prompts editable in-repo while Codex always reads the latest version from `~/.codex/prompts`.

**Important details**

- Codex scans only the top level of `~/.codex/prompts`, so avoid subdirectories.
- Restart Codex (or begin a new chat) after copying so it picks up the new slash commands.
- Feel free to version-control these prompt files so the team stays in sync when they change.

### 3. Templates and conventions

The commands expect these files in your repo:

- `.fastai/templates/brief.md`
- `.fastai/templates/plan.md`
- `.fastai/conventions/*.md`

Keep them committed and up to date. If you move or rename them, update the command files accordingly.

---

## The Fast AI 3-Step Workflow

Running each step in a **fresh CLI session** keeps context short and results crisp.

### Step 1 — `/fastai-create-brief`

- The agent asks six numbered questions, derives a human-readable short description, creates the next `.fastai/features/{NNN_slug}/` directory, copies `brief.md`, and fills it out.
- Answer the questions in a numbered list (`1. ...`, `2. ...`, …) so the agent can map them directly onto the template.
- The command double-checks for missing details and asks follow-ups before confirming the brief path.
- You never need to pass arguments to this command; it always creates the next feature directory automatically.

### Step 2 — `/fastai-create-plan` (new session recommended)

- The agent loads the latest brief (or a path you provide), reads `.fastai/conventions/*.md`, inspects any referenced code files, and writes an actionable, checkbox-style plan into `plan.md`.
- It validates that the brief exists, confirms before overwriting an existing plan, and asks targeted follow-up questions to close gaps.
- When done, it tells you exactly where the finished `plan.md` lives.
- **Gemini CLI invocation:** `/fastai-create-plan .fastai/features/005_payments/brief.md` (just pass the path; no `KEY=value`). If you omit the argument entirely, the command auto-detects the latest brief and only asks for a path if none exist.
- **GPT Codex invocation:** `/fastai-create-plan BRIEF_PATH=.fastai/features/005_payments/brief.md`. Likewise, you can run `/fastai-create-plan` with no arguments—Codex will look up the newest brief and request a path if it can’t find one.

### Step 3 — `/fastai-execute-plan` (new session recommended)

- The agent opens `plan.md`, skips any steps already marked `[x]`, and executes each remaining `[ ]` item in order.
- After completing a step it toggles the checkbox, saves `plan.md`, and tracks modified files.
- When all steps are done it reviews the changed files, discovers and runs the appropriate test command, and appends a management-ready section titled `## Manager Report` summarizing the business impact.
- **Gemini CLI invocation:** `/fastai-execute-plan .fastai/features/005_payments/plan.md` (plain path argument). You can also run `/fastai-execute-plan` without arguments; it will open the most recent plan or ask you for one if none exist.
- **GPT Codex invocation:** `/fastai-execute-plan PLAN_PATH=.fastai/features/005_payments/plan.md`. As with Gemini, omitting the argument makes the command search for the latest plan and prompt you only when it can’t find one.

### Example run

```text
# Session 1 – Gemini CLI
/fastai-create-brief
...answer 6 questions in a numbered list...

# Session 2 – Codex (clean chat)
/fastai-create-plan BRIEF_PATH=.fastai/features/005_payments/brief.md
...respond to follow-up questions...

# Session 3 – Gemini CLI (fresh session again)
/fastai-execute-plan .fastai/features/005_payments/plan.md
...agent edits files, runs tests, and appends the manager report...
```

This rhythm minimizes token usage, creates natural checkpoints for code review, and keeps each session tightly scoped.

---

## Tips & Gotchas

- **Restart after installation.** Both CLIs read custom commands only when they start.
- **Make sure the repo is writable.** Commands create directories beneath `.fastai/features/`. If permissions are read-only, they will fail immediately.
- **Structured answers matter.** The brief command expects numbered replies; unstructured text makes the template harder to fill accurately.
- **Confirm before overwriting.** If you rerun `/fastai-create-plan`, decide whether to reuse the existing `plan.md` or replace it. The command will prompt, but double-check to avoid losing work.
- **Resume execution safely.** `/fastai-execute-plan` intentionally skips `[x]` steps so you can restart it after a break without redoing completed work.
- **Share the management report.** The final section avoids file names and highlights business value—perfect for status updates or sprint reviews.
- **Language-indifferent behavior.** Even though the templates are in English, the commands automatically respond in the user's language (briefs, plans, and the final manager report included).

### Command invocation reference

**Gemini CLI**

- `/fastai-create-brief` — no arguments needed.
- `/fastai-create-plan [optional_path_to_brief.md]` — pass just the path if you want a specific brief; otherwise omit it and the command finds the latest brief or asks for one.
- `/fastai-execute-plan [optional_path_to_plan.md]` — same pattern: provide a path if you want to override the auto-detected latest plan.

**GPT Codex**

- `/fastai-create-brief` — no arguments needed.
- `/fastai-create-plan [optional BRIEF_PATH=...]` — e.g., `/fastai-create-plan BRIEF_PATH=.fastai/features/005_payments/brief.md`. Without the argument, Codex locates the latest brief and prompts you only if it can’t find one.
- `/fastai-execute-plan [optional PLAN_PATH=...]` — e.g., `/fastai-execute-plan PLAN_PATH=.fastai/features/005_payments/plan.md`. As above, omitting `PLAN_PATH` lets Codex auto-discover the current plan.

---

## What’s Next?

- Customize the templates in `.fastai/templates/` to match your internal brief/plan style.
- Add more project-specific commands (e.g., regression test suites or QA checklists) alongside the core trio.
- Open an issue or PR if you discover improvements—the framework keeps evolving through real-world use.

Happy shipping! Let the brief → plan → execute cadence help you deliver fast, consistent results.***
