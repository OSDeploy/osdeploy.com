---
name: osdeploy-overview-guide
description: Create, rewrite, or review concise, step-based OSDeploy workflow overview guides modeled on osdeploy-guide/initial-setup/registration/README.md. Use whenever an OSDeploy GitBook README or overview page must introduce a command-driven workflow, state prerequisites and defaults, summarize how it works, and direct readers to a detailed guide or command reference.
---

# OSDeploy Overview Guide

Create practical overview pages that help an administrator understand a workflow and run its default command without reproducing the full function reference.

## Required context

Before writing or editing:

1. Read `.github/skills/gitbook/SKILL.md` and follow its GitBook Markdown rules.
2. Read the repository-level `.github/copilot-instructions.md` and apply the OSDeploy voice, platform, and PowerShell conventions.
3. Read the public function source and its comment-based help. Treat implementation behavior as authoritative.
4. Read the related detailed guide and command reference when they exist. Keep their roles distinct from the overview.
5. Read neighboring pages and the relevant `SUMMARY.md` entries to preserve naming and navigation patterns.

Do not infer defaults, prerequisites, side effects, fallback behavior, or supported environments from the command name alone. If the source and existing documentation disagree, document the source behavior and flag the discrepancy.

## Reader goal

Write for Windows administrators and deployment engineers who need to answer these questions quickly:

- What does this workflow do?
- What must be installed or configured first?
- What happens when I run the default command?
- What defaults and automatic selections matter?
- Where can I find advanced examples and parameter details?

## Page structure

Use this order unless the workflow clearly requires a small adjustment:

1. YAML frontmatter with a one-sentence `description`.
2. One H1 workflow title, usually an imperative task such as `# Create a Hyper-V VM`.
3. A short opening paragraph naming the primary command and summarizing its end-to-end result.
4. An info hint when the workflow is optional or needs important context before the reader starts.
5. One H2 that states the task, followed by a `{% stepper %}` block.
6. A `{% step %}` for each action the administrator performs, with an imperative H3 title.
7. Requirements and blocking conditions in the first relevant step, including a warning hint when needed.
8. The shortest useful command in its execution step, followed by defaults or automatic selections that affect the result.
9. A final verification or next-action step when the result can be checked or requires follow-up work.
10. A closing paragraph linking to the detailed guide and command reference.

Close every `{% step %}` and the surrounding `{% stepper %}` block. Omit a step or hint when it has no useful content. Do not add steps merely to fill the template.

## Content rules

### Opening

Keep the introduction to one short paragraph. Name the command in inline code and describe the observable outcome, including major side effects such as creating files, mounting media, starting a VM, or opening another application.

### Step design

Use steps for actions the administrator performs, not for background facts. Start each step with `{% step %}`, add an imperative H3 title, and close it with `{% endstep %}`. Keep the steps in execution order.

```markdown
{% stepper %}
{% step %}
### Confirm the Requirements

List the actionable requirements.
{% endstep %}

{% step %}
### Run the Command

Run the shortest recommended command.
{% endstep %}

{% step %}
### Verify the Result

Confirm the expected output or side effect.
{% endstep %}
{% endstepper %}
```

List only prerequisites that readers can verify or act on. Include applicable operating system, architecture, PowerShell version, module, Windows features or external tools, elevation, restart state, and resource capacity. If requirements are informational and require no separate action, include them at the start of the first execution step instead of creating a requirements step.

Link prerequisite names to existing setup pages. Put nuanced failure conditions in a nearby warning hint instead of bloating a bullet.

### Basic usage and defaults

Show the shortest recommended invocation first:

```powershell
Verb-OSDeployNoun
```

Use a two-column `Setting | Default` table when the default invocation controls several meaningful settings. Keep values compact and distinguish fixed defaults from automatic discovery.

If operational guidance recommends a value different from the code default, state both explicitly. Never silently replace the actual default with the recommendation.

### Workflow detail

Within the relevant steps, summarize major validation, discovery, selection, creation, configuration, security, checkpoint, output, and startup phases as applicable. Keep the sequence chronological and distinguish actions performed by the administrator from actions performed automatically by the command.

Include fallback precedence when it materially changes the result. Keep parameter-by-parameter behavior, long accepted-value lists, exhaustive output schemas, and numerous examples in the detailed guide.

### Closing links

End with direct links to:

- The detailed guide for scenarios, advanced behavior, and examples.
- The command reference for compact syntax and parameter definitions.

Use natural link text that names the command or workflow.

## Style controls

- Use direct, technical language and imperative mood for instructions.
- Use PowerShell 7.6 or later unless the page explicitly covers Windows PowerShell 5.1.
- Use `powershell` for PowerShell code fences.
- Prefer short paragraphs, compact tables, and concrete verbs.
- Use one GitBook `{% stepper %}` block for the sequential workflow and imperative H3 titles for its steps.
- Use GitBook `{% hint %}` blocks for cautions and important guidance; do not use Markdown blockquotes.
- Format commands, parameters, paths, file names, values, and PowerShell literals as inline code.
- Use relative links and verify every target exists.
- Avoid marketing language, repeated explanations, and exhaustive reference material.

## Review checklist

Confirm that:

- The page lets a qualified reader run the default workflow.
- Every stated default and behavior matches the current implementation.
- Requirements include the conditions that cause the function to stop.
- Automatic selection and fallback behavior are summarized accurately.
- Recommendations are clearly distinguished from defaults.
- The stepper contains administrator actions in execution order.
- Every step and stepper tag is correctly closed.
- Advanced material is delegated to the detailed guide.
- The detailed guide and command-reference links resolve.
- GitBook hints and PowerShell fences are well formed.

## Canonical example

Use `osdeploy-guide/initial-setup/registration/README.md` as the canonical example for stepper syntax, pacing, and section depth. Generalize its documentation pattern; do not copy registration-specific requirements or behavior into unrelated guides.
