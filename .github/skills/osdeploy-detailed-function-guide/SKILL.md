---
name: osdeploy-detailed-function-guide
description: Create, rewrite, or review comprehensive OSDeploy function guides modeled on osdeploy-guide/create-a-hyper-v-vm/new-osdeployhypervm.md. Use whenever a GitBook page must explain an OSDeploy command in depth, including requirements, parameters, examples, automatic selection and fallback rules, configuration details, side effects, WhatIf behavior, and returned objects.
---

# OSDeploy Detailed Function Guide

Create detailed, scenario-oriented function guides that bridge the gap between a short workflow overview and generated command reference help.

## Required context

Before writing or editing:

1. Read `.github/skills/gitbook/SKILL.md` and follow its GitBook Markdown rules.
2. Read the repository-level `.github/copilot-instructions.md` and apply the OSDeploy voice, platform, and PowerShell conventions.
3. Read the complete public function source, comment-based help, validators, argument completers, and directly called private helpers that control documented behavior.
4. Read tests when available. Use them to verify edge cases, errors, `WhatIf`, output, and fallback behavior.
5. Read the related overview page and command reference. Avoid duplicating either page without adding scenario or behavioral value.
6. Read neighboring pages and relevant `SUMMARY.md` entries to preserve naming and navigation.

Treat executable behavior as authoritative. Verify every default, range, accepted value, precedence rule, side effect, output property, and platform requirement. If behavior cannot be confirmed, do not present it as fact.

## Reader goal

Write for administrators who understand the basic workflow and now need to configure it safely. The guide should answer:

- Which parameters apply to each scenario?
- What does each parameter accept and default to?
- What automatic discovery or fallback rules run when a value is omitted?
- Which settings vary by platform, architecture, generation, or host capability?
- What does `-WhatIf` do?
- What object or status does the function return?

## Page structure

Use the sections that apply, in this general order:

1. YAML frontmatter with a one-sentence `description`.
2. H1 using the exact command name.
3. A concise introduction that defines the command and the controls covered by the page.
4. `## Requirements` with links to prerequisite setup pages and a warning hint for blocking conditions.
5. A domain-specific guidance section such as `## Resource Guidance`, when readers need help choosing values.
6. `## Parameters` with a complete parameter table.
7. `## Examples` containing progressive, scenario-based H3 examples.
8. Behavior sections for major domains such as selection, networking, security, media, generation, configuration, or startup.
9. `## Output` documenting the returned type and properties.
10. A final paragraph linking back to the overview and to the compact command reference.

Behavior-section names must reflect the function. Do not force unrelated commands into Hyper-V-specific headings.

## Parameter table

State whether parameters are optional or identify parameter sets before the table. Use these columns when they fit:

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |

For each parameter:

- Use the exact PowerShell parameter name, including the leading hyphen.
- Use the effective PowerShell type.
- State the actual default or `Automatic` when omission triggers discovery.
- Include validation ranges, validation sets, path requirements, parameter-set constraints, and generation or platform applicability.
- Explain meaningful omitted-value behavior compactly.

Include relevant common parameters such as `-WhatIf` and `-Confirm` when `SupportsShouldProcess` changes the user experience. Do not claim support based only on convention; verify it in the function.

## Examples

Order examples from simplest to most specialized. Each example must include:

1. An H3 scenario title using an imperative phrase.
2. One or two sentences explaining the expected result and why the options are useful.
3. A complete, runnable `powershell` code block.
4. A short follow-up note only when a caveat or result needs clarification.

Cover applicable scenarios such as:

- Default invocation.
- Recommended sizing or configuration.
- Explicit path or input selection.
- Custom naming and resources.
- Suppressing an optional action.
- Alternate platform, generation, mode, or security choice.
- Automatic selection overrides.
- `-WhatIf` preview.
- Capturing and inspecting returned output.
- A fully customized invocation that combines compatible options.

Do not create trivial examples that merely substitute arbitrary values. Each example should answer a real operational question and remain consistent with parameter sets.

For multiline commands, use PowerShell backticks and preserve the repository's indentation style.

## Behavior sections

After the examples, explain the branches readers must understand to predict the function's behavior.

### Automatic selection and precedence

Document discovery roots, search patterns, sorting, precedence, and no-match fallbacks in execution order. State whether a fallback stops the command, emits a warning, or allows a partially configured result.

### Conditional configuration

Explain behavior controlled by environment, architecture, host capability, parameter set, generation, or optional tooling. Clearly separate required steps from best-effort enhancements.

### Side effects and ordering

Describe files and resources created, settings changed, external programs opened, checkpoints made, and services or objects started. Preserve meaningful ordering, especially when startup, cleanup, or rollback implications depend on it.

Use additional sections only for behavior substantial enough to help readers make a decision or troubleshoot an outcome.

## WhatIf and output

Document `-WhatIf` only when the function supports it. State what discovery still occurs, which mutations do not occur, and how the preview result differs from a successful result.

In `## Output`, name the returned .NET or PowerShell type. Use a `Property | Description` table for structured output. Document:

- Every stable public property.
- Null or empty states.
- Status flags and exactly when they become true.
- Differences between normal and preview output.

Do not promise incidental formatting, transient internal values, or properties that the source does not return.

## Style controls

- Use direct, technical language and imperative mood for instructions.
- Use PowerShell 7.6 or later unless explicitly documenting Windows PowerShell 5.1.
- Use `powershell` for PowerShell code fences and `text` only for plain value lists.
- Format commands, parameters, paths, file names, values, types, and PowerShell literals as inline code.
- Use GitBook `{% hint %}` blocks for warnings and important guidance; do not use Markdown blockquotes.
- Keep paragraphs focused and use tables for dense contracts, not narrative explanations.
- Distinguish code defaults, recommendations, automatic choices, and conditional behavior precisely.
- Use relative links and verify every target exists.
- Avoid marketing language and avoid restating the same fact in requirements, parameters, examples, and behavior sections unless the context adds value.

## Review checklist

Confirm that:

- The H1 exactly matches the function name.
- All documented parameters exist and all meaningful public parameters are covered.
- Defaults, validation ranges, accepted values, and parameter-set combinations match source code.
- Examples are runnable and use valid combinations.
- Discovery, precedence, fallback, and conditional branches are explicit.
- Requirements include environment checks that stop execution.
- Side effects and their order are accurate.
- `-WhatIf`, `-Confirm`, and output claims match implementation.
- Recommendations are distinct from defaults.
- The overview and command-reference links resolve.
- GitBook shortcodes, tables, and code fences are well formed.

## Canonical example

Use `osdeploy-guide/create-a-hyper-v-vm/new-osdeployhypervm.md` as the canonical example for depth, progression, and behavioral precision. Generalize its structure to the function being documented; do not copy Hyper-V-specific headings, parameters, or assumptions into unrelated guides.
