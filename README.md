# OSDeploy Home

OSDeploy is the dedicated site for the [Recast OSDeploy PowerShell Module](https://github.com/OSDeploy/RecastOSDeploy), which is used to install software requirements, maintain Windows RE sources, manage WinPE Drivers,  and to create Boot Media used for OSDCloud.

The OSDeploy PowerShell module is currently in preview, with its final release expected mid-September at Workplace Ninja Summit 2026 in Baden, Switzerland.

{% hint style="warning" %}
A valid free Recast Software Community License is required while the OSDeploy module is in preview. Complete [Community Registration](guide/registration.md) before running OSDeploy commands. This requirement does not apply to standalone use of the OSDCloud or legacy OSD modules.
{% endhint %}

{% hint style="info" %}
**Optional: Sign up for preview updates**

Sign up at [recastsoftware.com/osd-preview](https://www.recastsoftware.com/osd-preview/) to be notified when new builds are released and to participate in the insider preview program. This signup is separate from required Community License registration.
{% endhint %}

## PowerShell Module Ecosystem

OSDeploy is one of three PowerShell modules, with each module owns a distinct phase of the deployment workflow. This is the recommended starting point if you are new to OSDCloud or creating Boot Media.

| Module                                         | What it does                                 | Where it runs        | Status                |
| ---------------------------------------------- | -------------------------------------------- | -------------------- | --------------------- |
| [OSDeploy](command-reference/osdeploy/)        | Builds and customizes WinPE boot images      | Windows 11 (full OS) | Preview               |
| [OSDCloud](/broken/pages/xATFEheG9Abfxu0ZXNXF) | Deploys Windows 11 from cloud-hosted content | WinPE                | Current / Recommended |
| [OSD](command-reference/osd/)                  | Provides legacy OSDCloud v1 deployment       | WinPE                | Maintained (legacy)   |
