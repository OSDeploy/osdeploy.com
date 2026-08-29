# OSDeploy Home

OSDeploy is the documentation and guidance site for the [Recast OSDeploy PowerShell Module](https://github.com/OSDeploy/RecastOSDeploy). Use these guides to prepare an OSDeploy workstation, maintain deployment content, build boot media, and automate the complete setup workflow.

OSDeploy supports creating OSDCloud boot images with the OSDeploy and OSDCloud PowerShell modules. The OSDeploy module builds and customizes the boot image on Windows, while the OSDCloud module runs from that image in WinPE to deploy Windows.

The OSDeploy PowerShell module is currently in preview, with its final release expected at Workplace Ninja Summit 2026 in Baden, Switzerland.

{% hint style="info" %}
**OSDeploy and OSDCloud are releasing new modules. Register for preview updates.**

Sign up at [recastsoftware.com/osd-preview](https://www.recastsoftware.com/osd-preview/) to be notified when new builds are released and to participate in the insider preview program.
{% endhint %}

{% embed url="https://www.recastsoftware.com/?utm_source=osdeploy&utm_medium=ad&utm_campaign=web" %}

## PowerShell Module Ecosystem

Three modules, three execution contexts. Each module owns a distinct phase of the deployment workflow.

| Module                                  | What it does                                 | Where it runs        | Status                |
| --------------------------------------- | -------------------------------------------- | -------------------- | --------------------- |
| [OSDeploy](command-reference/osdeploy/) | Builds and customizes WinPE boot images      | Windows 11 (full OS) | Preview               |
| [OSDCloud](command-reference/osdcloud/) | Deploys Windows 11 from cloud-hosted content | WinPE                | Current / Recommended |
| [OSD](command-reference/osd/)           | Provides legacy OSDCloud v1 deployment       | WinPE                | Maintained (legacy)   |

## [OSDeploy PC](osdeploy-guide/initial-setup/operating-system/)

Prepare the Windows workstation, PowerShell environment, modules, and software required to run OSDeploy.

## [OSDeploy Core](osdeploy-guide/osdeploy-core/)

Maintain the local Windows images, drivers, tools, and other source content used to build OSDeploy boot media.

## [OSDeploy Boot](osdeploy-guide/osdeploy-boot/)

Build customized WinPE boot images and bootable media from content maintained in OSDeploy Core.

## [OSDeploy Hydration](osdeploy-guide/hydration/)

Prepare an OSDeploy workstation and create current OSDCloud boot media with a coordinated end-to-end workflow.
