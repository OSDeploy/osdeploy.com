---
name: osdeploy-guide-basic
description: Create, rewrite, or review executable, step-based OSDeploy task guides in osdeploy-guide/basic. Use for basic setup and common OSDeploy tasks that need concise prerequisites, the shortest working commands, and clear steps without unnecessary parameter detail. Do not use for section landing pages or conceptual overviews.
---

# OSDeploy Guide Basic

Create concise guides that help Windows administrators complete a basic OSDeploy task without reading a full command reference.

## Scope

Use this skill for pages that guide an administrator through an executable task, such as building boot media, creating a USB drive, or testing a boot image in Hyper-V.

Do not force this structure onto section landing pages, software catalogs, conceptual explanations, or reference content that happens to be stored in `osdeploy-guide/basic`. Apply the general GitBook and repository instructions to those pages instead. If a page mixes a basic task with extensive parameter or implementation detail, keep the basic workflow here and move the additional detail to an advanced guide or command reference.

## Required Context

Before writing or editing:

1. Read `.github/skills/gitbook/SKILL.md` and follow its GitBook Markdown rules.
2. Read `.github/copilot-instructions.md` and apply the OSDeploy voice, platform, and PowerShell conventions.
3. Read the target page, its nearest task-guide neighbor, and their entries in `SUMMARY.md`.
4. Verify commands and behavior against the public function source or command help when the page makes a technical claim.
5. Check that every relative link resolves.

Treat current implementation behavior as authoritative. Do not infer requirements, defaults, or side effects from a command name.

## Reader Goal

Write for an administrator who needs to understand the task and run the recommended command.

Keep the page focused on the basic path. Move advanced configuration, alternative scenarios, complete parameter coverage, and implementation detail to advanced guides or command references.

## Page Structure

Use this order:

1. Add YAML frontmatter with a one-sentence `description`.
2. Add one clear H1 task title.
3. Introduce the result in one short paragraph and name the primary command when applicable.
4. Add an important warning or context hint only when the reader needs it before starting.
5. Add one H2 task heading followed by a `{% stepper %}` block.
6. Add one `{% step %}` for each action, using an imperative H3 title.
7. Add a final verification or next-action step only when it provides useful information beyond the expected result of the command.
8. Link to advanced guidance or a command reference only when it adds useful detail.

Do not require a separate verification step. State the expected result in the command step when that is sufficient, and do not add sections or steps merely to fill the structure.

## Write Simple Steps

Keep actions in execution order. Put requirements in the first step only when readers must check or satisfy them before continuing.

````markdown
## Complete the Task

{% stepper %}
{% step %}
### Confirm the Requirements

List only requirements the reader can verify or act on.
{% endstep %}

{% step %}
### Run the Command

```powershell
Verb-OSDeployNoun
```

Describe prompts and the expected result.
{% endstep %}
{% endstepper %}
````

Close every `{% step %}` and `{% stepper %}` tag. Use steps for actions the administrator performs, not for background explanation. Add a verification step only when the result is not already clear or an additional check materially helps the reader.

## Keep Commands Basic

- Show the shortest command that completes the task.
- Omit parameters when the command works without them.
- Include a parameter only when the reader must provide it, when it prevents a destructive mistake, or when the basic workflow requires a specific value.
- Explain interactive prompts in plain language instead of reproducing every option.
- Do not add parameter tables, syntax listings, accepted-value lists, or multiple variations to a basic guide.
- Mention defaults or automatic selection only when they materially affect what the reader receives.
- When a separate verification command is useful, keep it short and directly tied to the result.

## Content Rules

- State only actionable prerequisites and important blocking conditions.
- Put destructive or failure-prone behavior in a nearby `{% hint style="warning" %}` or `{% hint style="danger" %}` block.
- Describe major visible side effects, such as clearing a disk, creating media, starting a VM, or opening another application.
- Prefer a short paragraph or compact list over a table.
- Keep conceptual background brief and place it before the stepper only when it helps the reader complete the task.
- Do not duplicate advanced guides or command references.

## Style

- Use direct, technical language and imperative mood.
- Use PowerShell 7.6 or later unless the page explicitly covers Windows PowerShell 5.1.
- Use `powershell` for PowerShell code fences.
- Format commands, paths, file names, and literal values as inline code.
- Use relative links.
- Avoid marketing language, repetition, and exhaustive detail.

## Review Checklist

Confirm that:

- The page belongs in `osdeploy-guide/basic`.
- A qualified reader can complete the basic task from the steps.
- The shortest working command appears first.
- Parameters are omitted unless they are necessary for the basic task.
- Requirements, warnings, prompts, and visible results are accurate.
- Any separate verification or next-action step adds information that is not already clear from the command result.
- Every GitBook step and stepper tag is correctly closed.
- PowerShell fences and relative links are valid.
- Advanced detail is left to the appropriate guide or command reference.

## Local Examples

Use `osdeploy-boot.md`, `osdeploy-usb.md`, and `osdeploy-hypervm.md` in `osdeploy-guide/basic` as local examples for tone, stepper syntax, and depth. Preserve their simple execution flow while removing reference-style detail that is not needed to complete the basic task.
