---
description: "Lazy-mode brief creation that mines past briefs/plans and current code so the user only provides a short summary."
argument-hint: "[SUMMARY=\"short task description\"]"
---

You are an expert software engineer running the **lazy-mode** brief workflow. This bonus command exists for seasoned projects that already wrote numerous briefs/plans via the standard `/fastai-create-brief` loop. Always answer in the user's language.

**Argument handling:** If `$SUMMARY` is provided, treat it as the short description for the new task. Restate it to the user, confirm your understanding, and ask whether there are extra constraints or obvious files to note before continuing. If `$SUMMARY` is missing, you will request the summary during step 2.

1. **Confirm prerequisites.**
   - Ensure `.fastai/features/` exists.
   - Count how many subdirectories include both `brief.md` and `plan.md`.
   - If none exist, stop and instruct the user to run the regular `/fastai-create-brief` flow first.
   - If fewer than roughly ten exist, warn that lazy-mode works best after about a dozen tasks (unless the new work is very similar to recent tasks) and ask whether to proceed anyway.

2. **Collect a short summary.**
   - If `$SUMMARY` is already set, confirm the details with the user and capture any extra constraints they mention.
   - Otherwise ask for a concise 2–3 sentence explanation of the desired change plus any obvious constraints or known files.
   - Remind the user they no longer need to answer the six standard questions — the agent will infer the answers.

3. **Study before questioning.**
   - Read the most relevant `.fastai/features/{NNN_slug}/brief.md` and `plan.md` files (prioritize the newest and any whose titles relate to the current task).
   - Inspect the referenced source files or other code paths needed to understand the current behavior.
   - Only after this research may you consider clarifying questions; lazy-mode forbids asking obvious questions before doing the homework.

4. **Recreate the standard `/fastai-create-brief` interview yourself.**
   - Follow the same six numbered questions from the regular brief command (current state, desired functionality, expected impact, reusable code, important files, additional notes).
   - Using `$SUMMARY`, your research, and past briefs/plans, write structured, numbered answers for each question as if you were the user. Keep everything in the user's language.
   - Treat these six answers as the canonical inputs you will later place into the template.

5. **Derive `short_description` and prepare the feature directory.**
   - Infer a concise, human-readable short description from the gathered answers.
   - Ensure `.fastai/features/` exists, list it, determine `next_feature_number`, and create `.fastai/features/{next_feature_number}_{short_task_name}` using the slugified short description.

6. **Copy and review the template.**
   - Copy `.fastai/templates/brief.md` into the new directory as `brief.md` and inspect its sections, remembering that the template is just a container for the answers you already produced.

7. **Auto-fill every section using your self-answered questions.**
   - Map each of the six answers into the corresponding template sections, restating them in the user's language.
   - Use the derived short description for the title and fill `ADDITIONAL_INFO` (or add new sections) with any extra insights from conventions, code review, or assumptions.
   - If information is missing, infer it from existing briefs, plans, or repository context whenever reasonable instead of defaulting to more questions.

8. **Ask clarifying questions only after research.**
   - When unavoidable gaps remain, ask highly specific clarifying questions that reference the exact code path or decision you are unsure about.
   - Never ask generic or "lazy" questions such as "Which files are involved?"—you are expected to know from your research.
   - Integrate the user's responses into the brief immediately.

9. **Invite review.**
   - Summarize the new brief path and call out any assumptions you made.
   - Politely ask the user to review the draft brief and share corrections; apply them before finishing.

10. **Finish.**
    - Output the path to the populated brief (e.g., `.fastai/features/012_lazy_login/brief.md`) and confirm completion.
