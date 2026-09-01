---
name: osdeploy-cmdlets
description: Create, rewrite, or review OSDeploy public-function usage guides under osdeploy-guide/cmdlets. Use whenever documenting an OSDeploy cmdlet, refreshing these guides from PowerShell source, writing the Cmdlets or Install-OSDeploySoftware landing pages, or documenting an Install-OSDeploySoftware component. Covers requirements, parameters, operational examples, discovery and fallback behavior, side effects, WhatIf and confirmation boundaries, and output.
---

# OSDeploy Cmdlets

Write source-grounded guides that show Windows administrators how to use OSDeploy public functions safely and predictably. Use `osdeploy-guide/cmdlets/build-osdeployboot.md` as the canonical example for structure, scenario depth, and behavioral precision, while keeping simpler commands proportionate.

## Required context

Before writing or editing a page:

1. Read `.github/copilot-instructions.md`, `AGENTS.md`, and `.github/skills/gitbook/skill.md` in the `osdeploy.com` repository.
2. Read the complete matching function in `RecastOSDeploy/OSDeploy/public`.
3. Follow directly called private helpers only when they control user-visible selection, precedence, prompts, fallback, side effects, or output.
4. Read relevant entries in `RecastOSDeploy/OSDeploy/core/module.json` when the function obtains versions, URLs, component names, architecture rules, or installer behavior from module data.
5. Read the generated module help in `RecastOSDeploy/OSDeploy/docs`, the compact page in `command-reference/osdeploy`, related workflow pages, and neighboring cmdlet guides as cross-checks and link targets.
6. Read tests when available. Use them to verify validation, failure behavior, output, and `ShouldProcess` boundaries.

Treat executable behavior as authoritative. Resolve conflicts in favor of the public function and the helpers or data it executes. Do not infer behavior from names, stale examples, or neighboring pages.

## Choose the page mode

### Direct command guide

Use for a page that documents one public function. Include the sections that apply in this order:

1. YAML frontmatter with a one-sentence `description`.
2. H1 containing the exact command name.
3. A concise introduction defining the command and its primary controls.
4. `## Requirements` with blocking prerequisites and links to setup pages.
5. `## Parameters` with a complete parameter table.
6. `## Examples` with progressive operational scenarios.
7. Function-specific behavior sections that explain consequential branches.
8. `## WhatIf and Confirmation` when `SupportsShouldProcess` affects the experience.
9. `## Output` with the actual pipeline result.
10. A final paragraph linking to an applicable workflow page and compact command reference.

Add a domain-specific guidance section before `## Parameters` when readers must choose resources, architectures, sources, editions, networks, media, or another consequential option.

### Composite or section landing page

Use for `osdeploy-guide/cmdlets/README.md` and pages that coordinate multiple commands. Explain the section or workflow, show execution order when relevant, and link to its command guides. Do not invent a parameter or output contract for a landing page.

### Install-OSDeploySoftware component guide

Use for child pages under `osdeploy-guide/cmdlets/install-osdeploysoftware`. Document:

- The exact `-Name` value.
- What is installed or enabled and why OSDeploy uses it.
- Platform, architecture, elevation, network, package-manager, and reboot requirements.
- Default, `-Force`, and `-WhatIf` behavior that applies to the component.
- How to verify the result.
- A relevant next step.

Keep the parent command contract in `install-osdeploysoftware/README.md`. Do not repeat its full parameter table on every component page.

## Parameter contract

Use this table when documenting a direct command:

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |

For each parameter:

- Use the exact name, including the leading hyphen.
- Use the effective PowerShell type.
- State the actual default, `None`, or `Automatic` when omission triggers discovery.
- Include validation sets, ranges, path requirements, parameter-set constraints, pipeline binding, and architecture or platform applicability.
- Explain consequential omitted-value behavior.

Include `-WhatIf` and `-Confirm` only after verifying `SupportsShouldProcess`. Describe the actual placement of each `ShouldProcess` call; do not imply that earlier prompts, discovery, downloads, or writes are suppressed.

## Examples

Order examples from the simplest supported invocation to specialized scenarios. Each example must contain:

1. An H3 title phrased as an operational task.
2. One or two sentences describing the expected result.
3. A complete, runnable `powershell` code block.
4. A short caveat only when needed.

Cover meaningful scenarios such as default use, explicit selection, automatic discovery overrides, force or replacement behavior, preview, and output inspection. Use only valid parameter-set combinations. Do not create examples that merely substitute arbitrary values.

## Behavior sections

Explain the branches readers need to predict results or diagnose a failure. Use function-specific headings such as `Source Selection`, `Catalog Selection`, `USB Layout`, `MDT Stages`, or `Virtual Machine Configuration`.

Document these details when applicable:

- Discovery roots, filtering, sorting, and precedence.
- No-match, cancellation, and fallback behavior.
- Blocking checks versus best-effort enhancements.
- Prompts and whether they can be bypassed.
- Files, directories, disks, virtual machines, registry values, services, and external tools created or changed.
- Meaningful execution order and partial-completion states.
- Architecture, host, source-image, or environment-variable differences.

Do not expose private helper names unless the name itself is required for troubleshooting or extension. Describe the user-visible behavior instead.

## Output

State the actual returned .NET or PowerShell type. Use a property table for structured output:

| Property | Description |
| --- | --- |

Document stable public properties, null or empty states, and differences between preview and successful execution. If the function intentionally writes no pipeline object, say so directly. Distinguish pipeline output from host messages and process-wide state.

## GitBook and OSDeploy style

- Use direct, technical language and imperative mood for instructions.
- Target PowerShell 7.6 or later unless explicitly documenting a Windows PowerShell 5.1 requirement.
- Identify `amd64` and `arm64` differences when relevant.
- Use `powershell` for PowerShell code fences and `text` for plain values or directory trees.
- Format commands, parameters, paths, file names, values, types, environment variables, and PowerShell literals as inline code.
- Use GitBook `{% hint %}` blocks for warnings and important information. Do not use Markdown blockquotes as callouts.
- Use relative links and verify every target.
- Preserve existing frontmatter and GitBook metadata unless a factual correction requires a change.
- Keep recommendations distinct from defaults.
- Avoid marketing language, duplicated command-reference syntax, and unsupported version claims.

## Review checklist

Confirm that:

- The page mode matches its role.
- The H1 exactly matches the function or navigation title.
- Every meaningful public parameter is covered and every documented parameter exists.
- Defaults, validators, parameter sets, and accepted combinations match source.
- Requirements include checks that stop execution.
- Examples are runnable and operationally useful.
- Discovery, precedence, cancellation, fallback, prompts, and side effects are explicit where consequential.
- `-WhatIf`, `-Confirm`, and destructive warnings match the actual `ShouldProcess` boundaries.
- Output claims match executable behavior rather than host formatting.
- Component pages use valid `Install-OSDeploySoftware -Name` values and current module metadata.
- Related workflow and command-reference links resolve.
- Frontmatter, tables, code fences, and GitBook shortcodes are well formed.

## Canonical example

Use `osdeploy-guide/cmdlets/build-osdeployboot.md` as the canonical example. Generalize its evidence standard and reader-focused structure; do not copy its WinPE-specific headings, length, or assumptions into unrelated pages.
