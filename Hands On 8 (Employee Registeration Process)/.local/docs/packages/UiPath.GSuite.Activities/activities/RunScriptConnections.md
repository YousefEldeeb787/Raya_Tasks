# Run Script

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.RunScriptConnections`

Executes a Google Apps Script function remotely.

**Package:** `UiPath.GSuite.Activities`
**Category:** Apps Script
**Connector:** `uipath-google-workspace`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `ScriptId` | Script ID | `InArgument` | `String` | Conditional | | The deployment ID of the Apps Script project. Required when `DevMode` is `false`. |
| `DevModeScriptId` | Script ID (Dev Mode) | `InArgument` | `String` | Conditional | | The script ID for development mode execution. Required when `DevMode` is `true`. |
| `Function` | Function | `InArgument` | `String` | Yes | | The name of the Apps Script function to run. |
| `Parameters` | Parameters | `List` | `List<InArgument<Object>>` | No | | Individual parameters to pass to the function. Used when `ParametersInputMode` is `Builder`. |
| `ExistingParameters` | Existing Parameters | `InArgument` | `IEnumerable<Object>` | No | | A collection of parameters to pass. Used when `ParametersInputMode` is `UseExisting`. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `DevMode` | Dev Mode | `Boolean` | `false` | Whether to run the script in development mode. |
| `ParametersInputMode` | Parameters Input Mode | [`ParametersMode`](#parametersmode) | `Builder` | How to provide parameters: individually or as a collection. |
| `Scopes` | Scopes | `String[]` | | Additional OAuth scopes required by the script. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `Result` | Result | `OutArgument` | `IDictionary<String, Object>` | The result returned by the Apps Script function. |

## Enum Reference

### `ParametersMode`

| Value | Description |
|-------|-------------|
| `Builder` | Add parameters individually in the designer. |
| `UseExisting` | Pass an existing `IEnumerable<Object>` collection. |

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<gsuite:RunScriptConnections
    DisplayName="Run Script"
    ConnectionId="{x:Null}"
    ScriptId="[deploymentId]"
    Function="[functionName]"
    DevMode="False"
    ParametersInputMode="Builder"
    Result="[scriptResult]" />
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- This activity does not use [`DriveItemArgument`](components/DriveItemArgument.md) for script selection. Instead, provide the script/deployment ID directly.
- The Apps Script project must be deployed as an API executable for production mode (`DevMode` = `false`).
- In development mode (`DevMode` = `true`), the most recently saved (not necessarily deployed) version of the script runs.
- The `Scopes` property defines OAuth scopes required by the script function. These must match the scopes configured in the Apps Script project.
