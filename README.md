# ⚡️ Fast AI 🤖

Fast AI is a blazing-fast AI coding framework for solo developers and small teams. It shines on small-to-medium projects because it enforces a simple three-step rhythm **BRIEF → PLAN → EXECUTE** that keeps quality high without wasting time.

## Why Fast AI?

- **Opinionated workflow.** Every task follows the same three phases, so nothing important gets skipped.
- **Shared automation.** Commands live in your repo and can be reused (or improved) by the whole team.
- **Cross-agent support.** Works with both Gemini CLI custom commands and GPT Codex slash prompts.
- **Language-indifferent.** Templates are written in English for consistency, but every command automatically detects the user's language, asks questions in it, and writes briefs and plans in that same language.
- **Built-in self-learning.** After each plan execution, Fast AI writes new lessons to `.fastai/conventions/lessons.md`, so the workflow continuously adapts to feedback without extra configuration.

## Add Fast AI to your project

Drop the framework files straight into the root of any repo where you want to run the brief → plan → execute loop. You only need the `.fastai/` directory from this repository — everything (commands, prompts, templates, and conventions) lives inside it.

1. **Using `git clone`.**
   ```bash
   cd /path/to/your-project
   git clone https://github.com/pavel-mironchik/fastai fastai-tmp
   cp -R fastai-tmp/.fastai .
   rm -rf fastai-tmp
   ```
   Commit the copied `.fastai/` folder to your repo so teammates can reuse the workflow.

2. **Downloading an archive.**
   ```bash
   cd /path/to/your-project
   curl -L https://github.com/pavel-mironchik/fastai/archive/refs/heads/main.tar.gz | tar -xz
   cp -R fastai-main/.fastai .
   rm -rf fastai-main
   ```
   (If you prefer ZIPs, replace the `curl | tar` line with `curl -L ...main.zip -o fastai.zip && unzip fastai.zip`.)

After copying the framework folder, follow the rest of this guide to wire the CLI commands into Gemini or Codex on your machine.

---

## Getting Started

### 1. Manual installation

Run the commands below yourself (they only touch your home directory) or hand them to an agent via the prompt in the next section.

#### 1.1 Gemini CLI commands

Copy everything from `.fastai/gemini/` into your local Gemini commands folder so they show up as `/fastai-create-brief`, the optional bonus `/fastai-create-brief-lazy`, `/fastai-create-plan`, `/fastai-preview-plan`, and `/fastai-execute-plan`.

```bash
mkdir -p ~/.gemini/commands/fastai
cp .fastai/gemini/*.toml ~/.gemini/commands/fastai/
```

> Prefer symlinks? Run `ln -s "$PWD/.fastai/gemini/"* ~/.gemini/commands/fastai/` instead of copying. This keeps the commands editable inside the repo while exposing the latest versions to Gemini CLI automatically.

**Important details**

- Gemini CLI loads commands on startup. Restart the CLI (or open a new session) after copying so the new `/fastai-*` commands appear in the slash menu.
- Commands inherit their names from the file path. Placing them under `~/.gemini/commands/fastai` produces the `/fastai-create-brief` style names we want.
- If you already have commands with those names, back them up before copying to avoid overwriting them.

#### 1.2 GPT Codex prompts

Codex CLI looks for Markdown prompts directly under `~/.codex/prompts`. Copy the files from `.fastai/codex/` into that directory and add the `fastai-` prefix so they become `/fastai-create-brief`, `/fastai-create-brief-lazy` (bonus lazy-mode), `/fastai-create-plan`, `/fastai-preview-plan`, and `/fastai-execute-plan`.

```bash
mkdir -p ~/.codex/prompts
cp .fastai/codex/create-brief.md ~/.codex/prompts/fastai-create-brief.md
cp .fastai/codex/create-brief-lazy.md ~/.codex/prompts/fastai-create-brief-lazy.md
cp .fastai/codex/create-plan.md ~/.codex/prompts/fastai-create-plan.md
cp .fastai/codex/preview-plan.md ~/.codex/prompts/fastai-preview-plan.md
cp .fastai/codex/execute-plan.md ~/.codex/prompts/fastai-execute-plan.md
```

> Prefer symlinks? Run:
>
> ```bash
> ln -s "$PWD/.fastai/codex/create-brief.md" ~/.codex/prompts/fastai-create-brief.md
> ln -s "$PWD/.fastai/codex/create-brief-lazy.md" ~/.codex/prompts/fastai-create-brief-lazy.md
> ln -s "$PWD/.fastai/codex/create-plan.md" ~/.codex/prompts/fastai-create-plan.md
> ln -s "$PWD/.fastai/codex/preview-plan.md" ~/.codex/prompts/fastai-preview-plan.md
> ln -s "$PWD/.fastai/codex/execute-plan.md" ~/.codex/prompts/fastai-execute-plan.md
> ```
>
> Symlinking keeps the prompts editable in-repo while Codex always reads the latest version from `~/.codex/prompts`.

**Important details**

- Codex scans only the top level of `~/.codex/prompts`, so avoid subdirectories.
- Restart Codex (or begin a new chat) after copying so it picks up the new slash commands.
- Feel free to version-control these prompt files so the team stays in sync when they change.

### 2. Prompt to let an agent install it for you

Prefer to stay hands-off? Paste the snippet below into any agent running from the repo root (Gemini CLI or GPT Codex). It reads this README for context, asks which agent(s) you need to install Fast AI for (Gemini CLI, GPT Codex, or both), asks whether you prefer copying or symlinking, and then runs the necessary shell commands — requesting approval whenever it needs to touch directories outside the workspace. Creating directories such as `~/.gemini/commands/fastai` or `~/.codex/prompts` with `mkdir -p` is safe even when they already exist, and the agent will detect any previously installed Fast AI files before overwriting or relinking them.

> Agents occasionally fail to finish this workflow — maybe they mis-detect an existing path, forget to request elevated permissions, or simply lose context mid-run. If it stalls, start a fresh chat and try again — or fall back to the manual commands above.

```text
You are inside the Fast AI repository root. Read README.md to understand how the Gemini and Codex commands must be installed (copy vs symlink). Ask me which agent(s) I actually use (Gemini CLI, GPT Codex, or both), and ask whether I prefer copying or symlinking. Then run the required shell commands for the agent(s) I selected:

1. If Gemini CLI is selected, ensure ~/.gemini/commands/fastai exists (explain that `mkdir -p` is harmless if it already exists).
2. For Gemini CLI, either copy (.fastai/gemini/*.toml -> ~/.gemini/commands/fastai/*.toml) or create symlinks, depending on my answer. When the destination file already exists, ask whether to overwrite, relink, or skip it.
3. If GPT Codex is selected, ensure ~/.codex/prompts exists (again, make it clear that re-running `mkdir -p` is safe).
4. For GPT Codex, copy or symlink .fastai/codex/*.md into ~/.codex/prompts with the `fastai-` prefix (e.g., fastai-create-plan.md). As above, check each destination before overwriting or relinking.

Whenever a command touches files outside the repository, ask for permission so I can approve it. After running the commands, list what was installed, clarify how existing files were handled, and remind me to restart Gemini/Codex so they detect the new files.
```

### 3. Templates and conventions

The commands expect these files in your repo:

- `.fastai/templates/brief.md`
- `.fastai/templates/plan.md`
- `.fastai/conventions/*.md`

Keep them committed and up to date. If you move or rename them, update the command files accordingly.

#### How to use `.fastai/conventions/`

Every Markdown file in `.fastai/conventions/` is injected as context before plans are generated or executed. Split guidance into focused files so it stays readable. Typical files you might create:

- `agent.md` — define the agent’s persona (“Act as a senior backend lead”, “Prioritize performance over features”).
- `coding_style.md` — document project/team conventions (indentation, naming schemes, migration filename formats, when to add comments, etc.).
- `frontend_hints.md` — share UI/UX patterns (how to wire Blade to Vue, how to run asset builds, which components should be reused).
- `important.md` — list project-wide constraints (what parts of the repo are off-limits, testing expectations, how to verify external dependencies).
- `patterns.md` — explain reusable architectural recipes (how to seed reference data, when to use Jobs vs direct model calls, integration strategies).
- `project_details.md` — describe the repository structure, technology stack, entry points, translation requirements, release/versioning notes.
- `lessons.md` — an agent appends self-learning notes automatically after executing plans so agents avoid repeating mistakes.

Add new `.md` files anytime you find something worth noting (“QA checklist”, “Release process”, “API integration tips”). Convert that knowledge into concise bullets under the relevant file so future runs inherit it automatically.

> **Why briefs skip conventions**  
> The brief command intentionally ignores `.fastai/conventions/`. Its job is to capture requirements quickly, so extra code-style or architecture context would only distract it. Conventions are pulled in when the plan is generated and when the plan is executed—exactly when those rules matter for writing code. This keeps the brief lightweight while still ensuring later stages follow every guideline.

> **How self-learning works**  
> After `/fastai-execute-plan` finishes, it reviews the session for user corrections, mistakes, or emotional feedback. Each insight becomes a concise bullet appended to `.fastai/conventions/lessons.md`. Future runs automatically load that file, so the framework keeps improving without manual bookkeeping.

---

## The Fast AI 3-Step Workflow

Running each step in a **fresh CLI session** keeps context short and results crisp.

### Step 1 — `/fastai-create-brief`

- The agent asks six numbered questions, derives a human-readable short description, creates the next `.fastai/features/{NNN_slug}/` directory, copies `brief.md`, and fills it out.
- Answer the questions in a numbered list (`1. ...`, `2. ...`, …) so the agent can map them directly onto the template.
- The command double-checks for missing details and asks follow-ups before confirming the brief path.
- You never need to pass arguments to this command; it always creates the next feature directory automatically.

#### Bonus lazy-mode — `/fastai-create-brief-lazy`

- Optional cherry on top for the “lazy” days once you’ve solved roughly a dozen tasks (or earlier if the new request is similar to recent ones) and already have briefs/plans to learn from.
- Instead of interviewing you, the agent reads the newest `.fastai/features/*/brief.md` and `plan.md` files plus the relevant code, then answers the six template sections itself so you only provide a short description.
- It still follows the exact six-question interview from `/fastai-create-brief`, just answering each question on your behalf before it ever looks at the template, so the resulting brief stays consistent with the standard flow.
- Clarifying questions are allowed only after the agent has studied the repository and past docs, which prevents the obvious “what file is this?” kind of questions and saves even more time.
- Kick it off by appending a 2–3 sentence summary inline (`/fastai-create-brief-lazy Add invoice export to XLSX`) or, on Codex, with `SUMMARY="..."`. Skip it and the agent will simply ask for that short summary before studying the codebase.
- When the draft brief is ready it shows you the path, highlights the assumptions it made, and politely asks you to review and adjust before moving on.

### Step 2 — `/fastai-create-plan` (new session recommended)

- The agent loads the latest brief (or a path you provide), reads `.fastai/conventions/*.md`, inspects any referenced code files, and writes an actionable, checkbox-style plan into `plan.md`.
- It validates that the brief exists, confirms before overwriting an existing plan, and asks targeted follow-up questions to close gaps.
- Plan files always live beside their briefs. The command automatically writes `.fastai/features/{NNN_slug}/plan.md` and reuses that same file when you rerun the command.
- When done, it tells you exactly where the finished `plan.md` lives.
- **Gemini CLI invocation:** `/fastai-create-plan .fastai/features/005_payments/brief.md`. Passing the brief path pins the command to that feature; omit it entirely to auto-detect the latest brief and only get prompted when none exist.
- **GPT Codex invocation:** `/fastai-create-plan BRIEF_PATH=.fastai/features/005_payments/brief.md`. Likewise, you can run `/fastai-create-plan` with no arguments—Codex will look up the newest brief and request a path if it can’t find one.

#### Optional Step — `/fastai-preview-plan`

- This step is optional—skip it for small or straightforward tasks. Use it only when the plan feels complex enough to justify the extra check.
- Run the command before the plan execution when you want a dry run. It loads the same `plan.md`, all conventions, and `lessons.md`, then analyzes every step without touching the working tree.
- Output highlights pending steps, risks, dependencies, open questions, and suggested pre-checks/tests so you can fix issues before editing code.
- In practice, it turns potential surprises into known action items: you see narrow spots upfront, know what to ask the user, and line up the tests you’ll need. That saves time versus re-running `/fastai-execute-plan` after mistakes.
- **Gemini CLI invocation:** `/fastai-preview-plan [.fastai/features/005_payments/plan.md]` (plain path argument is optional; omit it to preview the latest plan).
- **GPT Codex invocation:** `/fastai-preview-plan [PLAN_PATH=.fastai/features/005_payments/plan.md]`. As always, leave `PLAN_PATH` blank to let the command find the newest plan automatically.

### Step 3 — `/fastai-execute-plan` (new session recommended)

- The agent opens `plan.md`, skips any steps already marked `[x]`, and executes each remaining `[ ]` item in order.
- After completing a step it toggles the checkbox, saves `plan.md`, and tracks modified files.
- When all steps are done it discovers and runs the appropriate test command, reviews every touched path, records those files inside the plan's `## Created & Modified Files` section, and appends a management-ready section titled `## Manager Report` summarizing the business impact.
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
- **Lazy-mode needs history.** `/fastai-create-brief-lazy` is a friendly bonus for “lazy” moments — use it only once you have roughly a dozen briefs/plans (or clearly repetitive work) so it has enough material to study before it starts answering the template on your behalf.
- **Make sure the repo is writable.** Commands create directories beneath `.fastai/features/`. If permissions are read-only, they will fail immediately.
- **Structured answers matter.** The brief command expects numbered replies; unstructured text makes the template harder to fill accurately.
- **Confirm before overwriting.** If you rerun `/fastai-create-plan`, decide whether to reuse the existing `plan.md` or replace it. The command will prompt, but double-check to avoid losing work.
- **Plan location parity.** Plans live in the same feature directory as their briefs. Let `/fastai-create-plan` write `plan.md` automatically and avoid manually moving or renaming it.
- **Preview only when it’s worth it.** `/fastai-preview-plan` is optional and adds overhead, so skip it for tiny tasks. Use it when you want a dry run that surfaces risks, questions for the user, and the tests you’ll need before running `/fastai-execute-plan`.
- **Resume execution safely.** `/fastai-execute-plan` intentionally skips `[x]` steps so you can restart it after a break without redoing completed work.
- **Share the management report.** The final section avoids file names and highlights business value—perfect for status updates or sprint reviews.
- **Language-indifferent behavior.** Even though the templates are in English, the commands automatically respond in the user's language (briefs, plans, and the final manager report included).

### Command invocation reference

**Gemini CLI**

- `/fastai-create-brief` — no arguments needed.
- `/fastai-create-brief-lazy [short summary]` — optional lazy-mode; add the 2–3 sentence summary right after the command (e.g., `/fastai-create-brief-lazy Add XLSX export...`) or leave it blank and the agent will ask for it before diving into research.
- `/fastai-create-plan [optional_path_to_brief.md]` — pass just the brief path if you want a specific feature; otherwise omit it and the command finds the latest brief or asks for one.
- `/fastai-preview-plan [optional_path_to_plan.md]` — review steps, risks, and test expectations without modifying files.
- `/fastai-execute-plan [optional_path_to_plan.md]` — same pattern: provide a path if you want to override the auto-detected latest plan.

**GPT Codex**

- `/fastai-create-brief` — no arguments needed.
- `/fastai-create-brief-lazy [optional SUMMARY="short description"]` — same lazy-mode bonus; pass `SUMMARY="Add XLSX export..."` to skip the prompt or leave it blank and the agent will ask for your two-sentence summary before researching.
- `/fastai-create-plan [optional BRIEF_PATH=...]` — e.g., `/fastai-create-plan BRIEF_PATH=.fastai/features/005_payments/brief.md`. Without the argument, Codex locates the latest brief and prompts you only if it can’t find one.
- `/fastai-preview-plan [optional PLAN_PATH=...]` — e.g., `/fastai-preview-plan PLAN_PATH=.fastai/features/005_payments/plan.md`. Skipping the argument previews the latest plan automatically.
- `/fastai-execute-plan [optional PLAN_PATH=...]` — e.g., `/fastai-execute-plan PLAN_PATH=.fastai/features/005_payments/plan.md`. As above, omitting `PLAN_PATH` lets Codex auto-discover the current plan.

---

## What’s Next?

- Customize the templates in `.fastai/templates/` to match your internal brief/plan style.
- Enrich `.fastai/conventions/` with your own coding guides, release checklists, and the ever-growing `lessons.md` so every run benefits from accumulated knowledge.
- Add more project-specific commands (e.g., regression test suites or QA checklists) alongside the core trio.
- Open an issue or PR if you discover improvements—the framework keeps evolving through real-world use.
- Know when to scale up: Fast AI is intentionally lightweight. If your work grows into multi-team, multi-service programs with large release trains, switch to a heavier framework (or bespoke automation) that matches the new complexity. Two popular next steps are [Spec Kit](https://github.com/github/spec-kit) for full spec-driven development (briefs, specs, test flow) and [BMAD Method](https://github.com/bmad-code-org/BMAD-METHOD) when you need a multi-agent, workflow-rich system with built-in orchestration.

Happy shipping! Let the BRIEF → PLAN → EXECUTE cadence help you deliver fast, consistent results.
