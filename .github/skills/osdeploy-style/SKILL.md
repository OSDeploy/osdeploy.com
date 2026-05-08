# Skill: osdeploy-style

## Description

Apply the OSDeploy documentation page style to prerequisite overview pages and other reference pages in the `osdeploy.com` GitBook site. Use this skill when:

- Writing or updating a `README.md` under `osdeploy-prerequesites/`
- Adding a new overview/landing page for a tool, module, or concept
- Ensuring existing pages conform to the site's common structure and GitBook shortcodes

---

## Canonical Section Order

Every prerequisite overview page follows this section order:

1. **H1 title** — tool or concept name (matches the GitBook sidebar label)
2. **One-sentence introduction** — what the tool is and why it matters for OSDeploy workflows
3. **Key facts table** — `Property | Value` two-column table
4. **Official documentation embed** — GitBook `{% embed %}` shortcode, one link per embed block
5. **`## Overview`** — 2–4 sentences of expanded description; covers purpose, versioning context, architecture notes
6. **`## Why OSDeploy Requires [Tool]`** — bullet list; each bullet is one concrete reason the tool is a dependency
7. **`{% hint %}` blocks** — warning or info callouts where the reader needs an important caveat (retirement, package-format constraint, etc.)
8. **`## In This Section`** — bullet list of direct child pages with a one-line description per page

Sections 5–7 may be reordered when a warning hint is so important that it should appear before the embed (e.g., a retired tool). In that case, place the `{% hint style="warning" %}` block immediately after the H1 introduction, before the facts table.

---

## Key Facts Table Convention

Use a two-column `Property | Value` Markdown table. Suggested properties (use the ones that apply):

| Property | Notes |
|---|---|
| Publisher | Microsoft, open source, etc. |
| Version | Pin the specific version or minimum required version |
| Architecture | `amd64`, `arm64`, or `amd64 / arm64` |
| Platform | `Windows 11`, `WinPE`, `Windows (x64)`, etc. |
| Required by | Which OSDeploy commands or workflows need this tool |
| Status | `Current` / `Retired` — use `Retired` for officially deprecated tools |
| Executable | For runtimes (e.g., `pwsh` for PowerShell 7) |

---

## GitBook Shortcodes

### Embed (external documentation)

```
{% embed url="https://learn.microsoft.com/..." %}
{% endembed %}
```

- One URL per embed block.
- Always close with `{% endembed %}`.
- Do **not** use standard Markdown blockquotes (`>`) as replacements for hint blocks.

### Hint blocks

```
{% hint style="info" %}
Informational callout text.
{% endhint %}

{% hint style="warning" %}
Warning callout text.
{% endhint %}

{% hint style="danger" %}
Critical / destructive action callout.
{% endhint %}
```

---

## Writing Style Rules

- **Imperative mood** for instructions: "Run the following command", not "You should run…"
- **Technical and direct** — no marketing language, no filler phrases
- **PowerShell code blocks** always use the `powershell` language identifier
- All PowerShell targets **PowerShell 7.6 or later** (`pwsh`) unless explicitly noting Windows PowerShell 5.1
- Windows target is **amd64 and arm64** — note architecture differences when relevant
- First-person is acceptable in blog/archive content; avoid it in reference docs

---

## Skeleton Template

Use this skeleton for any new prerequisite overview page:

```markdown
# [Tool Name]

[One sentence: what this tool is and its role in OSDeploy.]

| Property | Value |
|---|---|
| Publisher | [Publisher] |
| Version | [Version] |
| Platform | [Platform] |
| Required by | [OSDeploy commands or workflows] |

{% embed url="https://[official-docs-url]" %}
{% endembed %}

## Overview

[2–4 sentences covering what the tool does, the specific component OSDeploy uses, and any version or architecture context.]

## Why OSDeploy Requires [Tool]

- [Reason 1]
- [Reason 2]
- [Reason 3]

## In This Section

- [Child Page Title](child-page.md) — [One-line description]
- [Child Page Title](child-page.md) — [One-line description]
```

For retired tools, insert a `{% hint style="warning" %}` block immediately after the H1 introduction (before the table):

```markdown
# [Tool Name]

[One sentence intro.]

{% hint style="warning" %}
[Tool] has been officially retired by Microsoft. Install it only if you have an existing workflow that requires it.

[Retirement Notice](https://learn.microsoft.com/...)
{% endhint %}

| Property | Value |
...
```

---

## Well-Formed Style Examples

These existing pages in the `osdeploy.com` repo demonstrate correct style and can be used as reference:

- [`osdeploy-prerequesites/microsoft-deployment-toolkit/install-mdt.md`](../../osdeploy-prerequesites/microsoft-deployment-toolkit/install-mdt.md) — best example of hint blocks, property tables, and Related section
- [`osdeploy-prerequesites/powershell/install-powershell.md`](../../osdeploy-prerequesites/powershell/install-powershell.md) — embed shortcode + code block style + architecture-aware scripting
- [`osdeploy-prerequesites/powershell/psmodules.md`](../../osdeploy-prerequesites/powershell/psmodules.md) — H2-per-topic pattern with install blocks
