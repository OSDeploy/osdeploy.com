---
name: osdeploy-gitbook-assistant
description: Create, rewrite, or review public GitBook instruction pages that teach AI assistants how to use an OSDeploy PowerShell function safely and accurately. Use for OSDeploy GitBook Assistant pages, function skill pages, agent contracts, request collection, preview and approval workflows, execution rules, failure handling, and returned-object verification.
---

# OSDeploy GitBook Assistant

Create public GitBook pages that give an AI assistant enough verified information to plan, preview, execute, and verify an OSDeploy PowerShell function without inventing parameters or taking unapproved actions.

These pages are operational contracts for AI assistants. They are not VS Code `SKILL.md` files, command references, or administrator tutorials. Write them so an assistant can convert a user's intent into the smallest valid command while respecting environment requirements, automatic behavior, validation rules, mutation boundaries, and approval requirements.

## Required context

Before writing or editing:

1. Read `.github/skills/gitbook/SKILL.md` and follow its GitBook Markdown rules.
2. Read `.github/copilot-instructions.md` and apply the OSDeploy voice, platform, architecture, and PowerShell conventions.
3. Read the complete public function source, comment-based help, parameter attributes, validators, argument completers, and directly called private helpers that control documented behavior.
4. Read tests when available. Use them to verify validation, errors, `ShouldProcess`, preview behavior, fallback rules, side effects, and returned objects.
5. Read the related workflow overview, detailed function guide, and command reference. Use links to delegate administrator-oriented explanation rather than duplicating it.
6. Read neighboring assistant instruction pages and relevant `SUMMARY.md` entries to preserve naming, placement, and navigation.

Treat executable behavior as authoritative. Verify every command name, parameter, type, default, accepted value, precedence rule, prerequisite, side effect, wait, external process, failure boundary, and output property. If a claim cannot be confirmed, omit it or label it as unresolved; never guess.

## Define the assistant's job

State one concrete outcome for the page. The assistant should know whether it may only recommend and preview the command or may also execute it after approval.

Identify these boundaries before drafting:

| Boundary | Question to answer |
| --- | --- |
| Read-only work | Which discovery, validation, and preview commands may run without approval? |
| Mutating work | Which invocation creates, changes, starts, removes, or restarts something? |
| Approval | What exact preview or plan must the user approve before mutation? |
| Adjacent operations | Which setup, deletion, networking, restart, or remediation tasks are outside this workflow? |
| Partial failure | After which command could resources already exist? |
| Verification | Which returned properties or follow-up queries prove what happened? |

Do not grant broader authority than the function requires. Separate prerequisite remediation from normal function execution, especially when remediation installs features, changes networking, restarts Windows, or removes resources.

## Page structure

Use the sections that apply, in this general order:

1. YAML frontmatter with a concise `description` and relevant `icon`.
2. H1 in the form `# <CommandName> Skill`.
3. A one-paragraph purpose statement describing the user outcome.
4. An info hint that states the preferred default strategy when automatic discovery or omission is important.
5. `## Agent contract` with the execution boundary and non-negotiable rules.
6. `## Requirements` with a compact requirement table, optional non-mutating preflight, and cautions about what preflight cannot prove.
7. `## Collect the request` mapping user intent to parameter decisions.
8. `## Parameter reference` documenting every public parameter needed to build a valid command.
9. `## Build the command` showing the smallest invocation and a typed customized form.
10. `## Preview and approval workflow` for mutating functions.
11. `## Execution examples` covering representative read-only and approved execution scenarios.
12. `## Automatic behavior` describing omission, discovery, precedence, fallback, and conditional configuration.
13. `## Verify the result` documenting the returned type, stable properties, and expected status values.
14. `## Handle failures` defining stop conditions, partial-resource inspection, and prohibited automatic cleanup.
15. A closing paragraph linking to the workflow overview, detailed guide, and command reference.

Adjust headings to fit the function, but preserve the distinction between collecting intent, validating values, previewing mutation, executing, verifying, and recovering from failure.

## Agent contract

Write the contract as direct rules. Include only rules that affect assistant decisions.

Cover these points when applicable:

- Required operating system, architecture, host type, elevation, PowerShell version, module, Windows features, external utilities, capacity, and restart state.
- How to confirm that the installed module exports the command.
- Whether the command is mutating and whether `-WhatIf` or another preview mode must run first.
- Whether approval applies to the exact previewed parameter set and must be renewed after changes.
- Operations the assistant must not perform as part of this workflow.
- Parameters or values that must not be invented.
- Automatic parameters that should be omitted rather than resolved and injected by the assistant.
- Required Boolean syntax, path handling, quoting, or typed values.
- The need to capture and inspect returned output.
- The prohibition on automatic retry or cleanup after a possible partial mutation.

Do not claim that `-WhatIf` is safe or useful merely because it is a common parameter. Confirm the function uses `SupportsShouldProcess` and determine which prerequisite checks, discovery steps, or other work still occur before `ShouldProcess` prevents mutation.

## Requirements and preflight

Use a `Requirement | Required state` table for executable conditions. Include only conditions supported by source, helpers, or tests.

A preflight block must be non-mutating and must not overstate readiness. Prefer native inspection commands such as `Get-Command`, version properties, role checks, and resource queries. Do not silently install or repair prerequisites.

When the function performs deeper checks than the preflight can reproduce, add a warning hint that names the checks deferred to the function or its preview invocation.

## Collect the request

Translate user outcomes into parameter decisions with a table:

| User requirement | Parameter decision |
| --- | --- |
| Use the function default | Omit the parameter |
| Override an automatic choice | Set the parameter only after validating the explicit value |
| Suppress an optional action | Pass the function's validated Boolean or switch form |

Ask only for values needed to satisfy an explicit user requirement. Preserve function-owned discovery by omitting parameters whose default is automatic. Never search for an automatic value and inject it unless the user asks to pin that exact value.

Define how to handle paths, names, credentials, secrets, selectors, and ambiguous media or platform choices. Require existence checks and extension or type validation when the function does.

## Parameter reference

State whether all parameters are optional or identify parameter sets. Use this table when it fits:

| Parameter | Type | Default | Accepted values and behavior |
| --- | --- | --- | --- |

For each parameter:

- Use the exact name, including the leading hyphen.
- Give the effective PowerShell type.
- Distinguish fixed defaults, automatic behavior, and recommendations.
- Include validation sets, ranges, path requirements, parameter-set restrictions, and conditional applicability.
- Explain omission behavior and consequential side effects compactly.
- Include relevant common parameters only when their behavior has been verified.

Long accepted-value lists may follow the table in a plain fenced block when that is easier for an assistant to parse.

## Build the command

Show the smallest valid command first. Add only parameters required by the request.

For customized execution, prefer a PowerShell splat so values remain typed and reviewable:

```powershell
$parameters = @{
	RequiredOverride = 'Value'
	OptionalAction = $false
}

$plan = Verb-OSDeployNoun @parameters -WhatIf
$plan | Format-List
```

Replace the placeholders with verified names and values. Do not add automatic defaults to the splat. Use explicit PowerShell Boolean values when the function accepts `Boolean`; do not rewrite them as switches.

## Preview and approval

For a mutating function with a verified preview mechanism, use one GitBook `{% stepper %}` with these stages:

1. Validate explicit values.
2. Run and capture the complete preview.
3. Present the resolved identity, paths, selections, resources, and actions.
4. Obtain approval for the exact previewed parameters.
5. Run the same command without preview and verify the result.

State which empty or null values are valid fallbacks and why they matter. Require a new preview whenever any parameter changes.

If the function has no reliable preview mechanism, do not fabricate one. Describe the available validation, state that execution is mutating, and require explicit approval immediately before the command.

## Examples

Order examples by assistant decision flow:

1. Preview defaults or perform the safest read-only operation.
2. Preview explicit inputs or common overrides.
3. Execute the exact approved parameters.

Add specialized examples only when they resolve a real ambiguity. Capture preview and execution output in variables and inspect it. Do not include an unapproved mutating command as the first or only example.

Use `powershell` fences and valid PowerShell 7.6 syntax. Ensure examples use compatible parameter sets and values within validation rules.

## Automatic behavior

Describe behavior in execution order:

- Discovery roots and search patterns.
- Sorting and selection precedence.
- Omitted-value and no-match fallbacks.
- Platform, architecture, generation, capability, or tool-dependent branches.
- Resource creation and configuration order.
- Optional actions such as checkpoints, external UI launches, waits, starts, or restarts.

State whether each fallback stops, warns, continues with reduced functionality, or returns an empty value. Keep internal implementation details only when they change an assistant decision or help it interpret the result.

## Verification and failures

Name the exact returned .NET or PowerShell type. Use a `Property | Verification` table for stable public properties. Explain null states, status flags, and preview-versus-execution differences.

For failure handling:

- Report the exact terminating error and last confirmed stage.
- Identify the first operation after which partial resources can exist.
- Provide non-mutating commands to inspect those resources when possible.
- Do not retry after an ambiguous partial mutation.
- Do not remove, overwrite, disconnect, or repair resources without separate approval.
- Link to the appropriate prerequisite or troubleshooting page for failures outside the function workflow.

Never imply transactional rollback unless the implementation guarantees it.

## Style controls

- Write direct instructions to an AI assistant using imperative mood.
- Use PowerShell 7.6 or later unless the function explicitly targets another host.
- Use `powershell` for PowerShell code fences.
- Format commands, parameters, paths, file names, values, types, and literals as inline code.
- Use GitBook `{% hint %}` blocks for important safety guidance; do not use Markdown blockquotes.
- Use a GitBook `{% stepper %}` only for ordered workflows.
- Keep rules atomic and testable. Avoid vague language such as "be careful" or "use an appropriate value."
- Distinguish what the assistant may infer, must validate, must ask, must preview, and must not do.
- Avoid exposing secrets in examples, output, or approval summaries.
- Use relative links and verify every target exists.
- Avoid marketing language and avoid copying administrator-oriented detail that does not alter assistant behavior.

## Review checklist

Confirm that:

- The page identifies one function and one assistant outcome.
- The agent contract clearly separates read-only and mutating operations.
- Every command, parameter, type, default, validation rule, and accepted value matches source.
- Automatic values remain omitted unless the user explicitly overrides them.
- Prerequisites and preflight checks are accurate and non-mutating.
- `-WhatIf`, approval, confirmation, and execution claims match actual implementation.
- Approval applies to an exact, displayed plan and changed parameters require a new preview.
- Examples are runnable, typed correctly, and use valid parameter combinations.
- Discovery, precedence, null fallbacks, conditional behavior, and side effects are explicit.
- Returned properties and status flags match normal and preview output.
- Failure handling does not retry, delete, or repair after possible partial mutation without approval.
- GitBook frontmatter, hints, steppers, tables, links, and code fences are well formed.
- The page appears in `SUMMARY.md` when it should be publicly navigable.

## Canonical example

Use `osdeploy-guide/osdeploy-hypervm/new-osdeployhypervm-skill.md` as the canonical example for contract language, request-to-parameter mapping, preview approval, automatic discovery, returned-object verification, and partial-failure handling. Generalize its structure to the function being documented; do not copy Hyper-V-specific requirements, parameters, or safety assumptions into unrelated function pages.
