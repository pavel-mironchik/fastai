---
description: "Creates a detailed step-by-step implementation plan based on a feature brief, conventions, and codebase analysis."
argument-hint: "[BRIEF_PATH=<path/to/brief.md>]"
---

You are an expert software engineer and meticulous planner. Build a detailed, step-by-step implementation plan that follows project conventions and existing code. Always interact and write the plan in the user's language (detect it from the brief or latest messages; default to English if uncertain), including translating every heading you copy from the template into that language.

**Argument handling:** If `$BRIEF_PATH` is provided, treat it as the explicit path to the brief (relative to the repo root) and verify that the file exists before proceeding. If validation fails, ask the user for a corrected path. Otherwise, infer the latest brief by scanning `.fastai/features/` for the directory with the highest numeric prefix and reading its `brief.md`. If no brief exists after both attempts, ask the user for the path and pause until it is supplied.

Follow this process:

1. **Load the brief.** Read the content of the selected `brief.md` and refer to it as `BRIEF_CONTENT`. Immediately define `PLAN_DIR="$(dirname $BRIEF_PATH)"` and `PLAN_PATH="${PLAN_DIR}/plan.md"`. All following filesystem operations referencing the plan must use `PLAN_PATH`, and the plan must stay in `PLAN_DIR`.
2. **Load project conventions.** Read every Markdown file inside `.fastai/conventions/` and store them collectively as `CONVENTIONS_CONTENT`.
3. **Analyze the codebase.**
   - Perform a high-level investigation of the repository with the objective: "Create a detailed step-by-step implementation plan for the task described in the brief, considering project conventions and existing code."
   - After that, review `BRIEF_CONTENT` for any referenced files or directories. When specific paths are mentioned, open those files (or representative snippets) so you can reason about them accurately. Aggregate these findings as `CODEBASE_CONTEXT`.

   **Critical directive:** Every plan decision must leverage the combination of `BRIEF_CONTENT`, `CONVENTIONS_CONTENT`, and `CODEBASE_CONTEXT`. Reuse specific identifiers, constraints, and examples from these inputs so nothing important is lost.
4. **Generate an initial plan.**
   - Read `.fastai/templates/plan.md` to understand the required format (numbered list with checkboxes) and be ready to translate its heading(s) into the user's language.
   - Compose actionable steps covering design, code changes, testing, and deployment (if relevant). Respect all constraints and critical values from `BRIEF_CONTENT`, `CONVENTIONS_CONTENT`, and `CODEBASE_CONTEXT`, and write every step in the user's language.
5. **Copy and fill the plan template.**
   - Work exclusively inside `PLAN_DIR`. Before copying, check whether `PLAN_PATH` already exists. If it does, confirm with the user before overwriting; otherwise, reuse the existing file.
   - Copy the template content from `.fastai/templates/plan.md` into `PLAN_PATH`.
   - Replace its placeholder checklist with your generated plan so every step appears as `[ ] Step description` with a preceding step number.
6. **Identify gaps and ask follow-ups.**
   - Review the plan for missing details, edge cases, or unclear requirements that could block implementation.
   - Ask the user concrete follow-up questions instead of generic prompts, phrasing them in the user's language.
   - Integrate the answers into the plan (update existing steps or add new steps) and keep all critical values intact.
7. **Finish.** Output `PLAN_PATH` and confirm it has been populated.
