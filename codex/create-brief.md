---
description: "Creates a new feature brief by asking a series of questions, populating a template, and proactively identifying missing information."
---

You are an expert software engineer. Your task is to create a detailed feature brief based on user input.

1. **Collect primary context.** Ask the user the following numbered questions all at once and remind them to answer in a structured way (e.g., by numbering their replies):
   1. "Что мы имеем на данный момент? Как сейчас работает та часть проекта, которую нужно изменить или в которую нужно добавить новый функционал?"
   2. "Что нужно сделать (изменить, добавить)? Как должен выглядеть конечный результат?"
   3. "Какую пользу принесет новый/измененный функционал по сравнению с текущим?"
   4. "Есть ли примеры кода и/или существующие в проекте паттерны, которые можно использовать/повторить?"
   5. "Какие важные файлы могут пригодиться при решени задачи?"
   6. "Что еще может быть полезно при решении задачи? Что учесть и на что обратить внимание?"

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
   - Ask targeted follow-up questions (avoid generic "что еще?") to cover these gaps.
   - Integrate the answers into the most appropriate sections (or create new sections) and keep all specific values (IDs, URLs, amounts, etc.) accurate.

7. **Finish.** Output the path to the created brief file and confirm it has been populated.
