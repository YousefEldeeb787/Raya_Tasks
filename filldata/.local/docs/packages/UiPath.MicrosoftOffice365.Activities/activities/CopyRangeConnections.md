# Copy Range

`UiPath.MicrosoftOffice365.Activities.Excel.CopyRangeConnections`

Copies a range from one Excel Online workbook to another.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Source Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The source workbook. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Range Location | `InArgument` | `String` | Yes | | The source sheet name or named range. |
| `Range` | Range | `InArgument` | `String` | Yes | | The source cell range to copy (e.g., "A1:D10"). |
| `DestinationItem` | Destination Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The destination workbook. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `DestinationRange` | Destination Range | `InArgument` | `String` | Yes | | The destination cell range to paste to. |
| `WhatToCopy` | What to Copy | `InArgument` | `ValuesType` | No | `Values` | Whether to copy values or formulas: `Values`, `Formulas`. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |
