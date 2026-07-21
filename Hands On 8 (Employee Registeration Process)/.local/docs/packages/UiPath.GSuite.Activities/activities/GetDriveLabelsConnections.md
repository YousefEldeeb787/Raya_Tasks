# Get Drive Labels

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.GetDriveLabelsConnections`

Retrieves the available Google Drive labels in the workspace organization, optionally filtered.

**Package:** `UiPath.GSuite.Activities`
**Category:** Drive
**Connector:** `uipath-google-drive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `DriveLabelsType` | Drive Labels Type | `InArgument` | `DriveLabelType` | Yes | `All` | The type of drive labels to retrieve. |
| `Filter` | Filter | `Property` | `LabelFilterCollection` | No | | Filter conditions applied to the drive labels retrieved. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Labels` | Labels | `OutArgument` | `List<GDriveLabel>` | The list of drive labels retrieved. |

## Enum Reference

### `DriveLabelType`
| Value | Description |
|-------|-------------|
| `Badged` | Badged (admin-created) labels |
| `Standard` | Standard (user-created) labels |
| `All` | All label types |

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<gsuite:GetDriveLabelsConnections
    DisplayName="Get Drive Labels"
    ConnectionId="{x:Null}"
    DriveLabelsType="[DriveLabelType.All]"
    Labels="[driveLabels]" />
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- Requires a Google Workspace connection with Drive and Drive Labels scopes.
- This activity does not use a [`DriveItemArgument`](components/DriveItemArgument.md) for file selection; it retrieves workspace-level labels.
- `DriveLabelsType` is required; the activity will fail validation if not provided.
