---
name: pop-quiz
description: Write a 5-question true/false exam that tests the developer's understanding of a plan, PR or branch diff, and publish it as an HTML page with instant feedback and hidden solutions. Use after an agent creates a PR, or when the user asks for an exam, quiz, or understanding check on a change.
---

# Pop Quiz

Write a short true/false exam on one PR or plan. The developer must answer it to
show that they understand the change. The model for the style is a
university statistics exam: each statement looks plausible, and you must
reason through the mechanism to decide if it is true.

## 1. Read the change

- If there was code written, get the diff: `gh pr diff <number>`, or `git diff <base>...HEAD`, where `<base>` is the PR's base branch or the repo's default branch (`git symbolic-ref --short refs/remotes/origin/HEAD`).
  Read the full functions and classes that the diff touches, their callers,
  and the configs that feed them. The best questions come from code that
  the diff does not show.
- If no code written yet, use the context to get information about the change the user plans to make. 
- Find what the change does NOT do, the defaults, the config merge order,
  the branches that a new flag skips, and the cases that fail silently.

## 2. Write statements

Each statement is one claim about the behavior of the code. Make them have
increasing difficulty. 
The default number of statements is 5, unless the user asks for another number.
The statements could come from categories as the following:

1. Broad purpose. Still not obvious: include one wrong detail that a
   reader who only skimmed the PR title would accept.
2. Defaults and backward compatibility: what happens to configs, callers,
   or checkpoints that do not know about the change.
3. Interaction with code outside the diff (config inheritance, a caller, a
   validation step, a shape).
4. A mechanism: why a part of the change is needed, or what a flag really
   controls.
5. An edge case or a near-miss configuration that the change does not
   cover, or that fails silently.

Rules:
- Never ask "Is it true that the PR does X and Y?". Ask about a
  consequence that the developer must derive.
- A false statement is wrong in exactly one detail. Do not make it
  absurd.
- Use both answers. Do not make all statements true or all false.
- Each statement must be decidable with no ambiguity: from the code
  when it exists, or from the plan agreed in the conversation when no
  code is written yet. If you cannot back the answer with a `file:line`
  reference (code) or a specific point in the plan, drop the statement.
- Keep each statement to 1–3 sentences. Put names in `code` style.
- Only the last question should cover an edge case.
- Each solution: the answer in 2–4 sentences, plus the references that
  prove it: `file:line` for code, or the plan step or decision it rests
  on (e.g. `plan: step 3`).

## 3. Build the page

- Use `template-paper.html` from this skill folder: a paper exam form.
  The box you tick shows a green check or a red X, and a tally in the
  header fills in as the user answers.
- Copy the template to the scratchpad as `pop-quiz-<repo>-<pr>.html`.
- Replace only the `EXAM` object between the `EXAM DATA` markers. Keep
  the rest of the template unchanged.
- Write `statement` and `explain` as plain text. Put code names in
  backticks: the page escapes the text and renders backticks as code.
- Put each `file:line` reference as one item in `refs`. The page shows
  them under the solution text.
- Publish it with the Artifact tool (`icon: "quiz"`). Give the link to
  the user.

## 4. In the chat

Give only the link. Do not show the answers or
hints in the chat.
