# Write Cell

`UiPath.MicrosoftOffice365.Activities.Excel.WriteCellConnections`

Writes a value to a single cell in an Excel Online workbook.

**Connector:** `uipath-microsoft-onedrive`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Item` | Workbook | `Property` | [`DriveItemArgument`](components/DriveItemArgument.md) | Yes | | The workbook to write to. See [DriveItemArgument](components/DriveItemArgument.md) for input modes. |
| `RangeLocation` | Range Location | `InArgument` | `String` | Yes | | The sheet name or named range containing the cell. |
| `Cell` | Cell | `InArgument` | `String` | Yes | | The cell address to write to (e.g., "A1"). |
| `WhatToWrite` | Value | `InArgument` | `Object` | Yes | | The value to write to the cell. |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |
