---
name: osdeploy-overview-guide
description: Create, rewrite, or review concise OSDeploy workflow overview guides modeled on osdeploy-guide/create-a-hyper-v-vm/README.md. Use whenever an OSDeploy GitBook README or overview page must introduce a command-driven workflow, state prerequisites and defaults, summarize how it works, and direct readers to a detailed guide or command reference.
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
4. `## Requirements` with a brief lead-in and a compact bullet list.
5. A warning hint for a prerequisite that commonly blocks or invalidates the workflow.
6. `## Basic Usage` with the simplest useful command.
7. A sentence explaining the default result, followed by a compact defaults table when several defaults matter.
8. An info hint for important sizing or operational guidance that differs from the function default.
9. `## How It Works` with a numbered, chronological summary of the major actions.
10. A warning hint for a successful-but-incomplete fallback state when one exists.
11. A final paragraph linking to the detailed guide and command reference.

Omit a section or hint when it has no useful content. Do not add sections merely to fill the template.

## Content rules

### Opening

Keep the introduction to one short paragraph. Name the command in inline code and describe the observable outcome, including major side effects such as creating files, mounting media, starting a VM, or opening another application.

### Requirements

List only prerequisites that readers can verify or act on. Include applicable operating system, architecture, PowerShell version, module, Windows features or external tools, elevation, restart state, and resource capacity.

Link prerequisite names to existing setup pages. Put nuanced failure conditions in a nearby warning hint instead of bloating a bullet.

### Basic usage and defaults

Show the shortest recommended invocation first:

```powershell
Verb-OSDeployNoun
```

Use a two-column `Setting | Default` table when the default invocation controls several meaningful settings. Keep values compact and distinguish fixed defaults from automatic discovery.

If operational guidance recommends a value different from the code default, state both explicitly. Never silently replace the actual default with the recommendation.

### How it works

Use a chronological numbered list. Summarize major validation, discovery, selection, creation, configuration, security, checkpoint, output, and startup phases as applicable.

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
- The action list follows execution order.
- Advanced material is delegated to the detailed guide.
- The detailed guide and command-reference links resolve.
- GitBook hints and PowerShell fences are well formed.

## Canonical example

Use `osdeploy-guide/create-a-hyper-v-vm/README.md` as the canonical example for scope, pacing, and section depth. Generalize its documentation pattern; do not copy Hyper-V-specific requirements or behavior into unrelated guides.
