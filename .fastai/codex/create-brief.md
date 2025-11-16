---
description: "Creates a new feature brief by asking a series of questions, populating a template, and proactively identifying missing information."
---

You are an expert software engineer. Your task is to create a detailed feature brief based on user input. Always speak the user's language (detect it from recent messages; default to English) and translate all section headings and content into that language instead of leaving anything in English.

1. **Collect primary context.** Ask the user the following numbered questions all at once, translating each question into the user's language. Remind them to answer in a structured way (e.g., by numbering their replies):
   1. "What is the current state of the area that needs to change or receive the new functionality?"
   2. "What exactly has to be changed or added? What should the final result look like?"
   3. "What benefits or improvements will this new/modified functionality bring compared to today?"
   4. "Are there code samples or existing patterns in the project that we should reuse?"
   5. "Which important files will likely be helpful while working on this task?"
   6. "What else should we keep in mind? Any constraints, edge cases, or tips?"

2. **Derive a short description.** From the user's answers, automatically determine a concise, human-readable short_description of the task (e.g., "Add user authentication"). This will be used for the brief's main title.

3. **Create the feature directory.**
   - Ensure `.fastai/features/` exists (create it if necessary) before listing its contents.
   - List the contents of `.fastai/features/`.
   - Identify the highest numeric prefix (e.g., from `001_feature_name`).
   - Increment this number for `next_feature_number` (start at `001` if no directories exist).
   - Convert the short_description into a slug (lowercase, replace spaces with underscores, remove special characters) to form `short_task_name`.
   - Create `.fastai/features/{next_feature_number}_{short_task_name}`.

4. **Copy the template.**
   - Confirm `.fastai/templates/brief.md` exists and is readable. If it is missing, pause and ask the user for the correct template path before proceeding.
   - Copy its content into `.fastai/features/{next_feature_number}_{short_task_name}/brief.md`.

5. **Fill the brief intelligently.**
   - Analyze the structure of the copied template and map each heading to the most relevant part of the user's answers.
   - Fill the title/summary with the derived short_description.
   - Adapt gracefully if the template structure changes; place information where it fits best or add clearly labeled sections when necessary.

6. **Identify missing information.**
   - Review the partially filled brief and determine what critical technical or product details are missing.
   - Ask targeted follow-up questions (avoid generic "anything else?") to cover these gaps, phrasing them in the user's language.
   - Integrate the answers into the most appropriate sections (or create new sections) and keep all specific values (IDs, URLs, amounts, etc.) accurate.

7. **Finish.** Output the path to the created brief file and confirm it has been populated.
