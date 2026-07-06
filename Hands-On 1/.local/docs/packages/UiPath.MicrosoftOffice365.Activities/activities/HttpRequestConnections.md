# HTTP Request

`UiPath.MicrosoftOffice365.Activities.General.HttpRequestConnections`

Sends an HTTP request to the Microsoft Graph API.

**Connector:** `uipath-microsoft-365`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `RequestMethod` | Request Method | `InArgument` | `RequestMethod` | Yes | | The HTTP method: `GET`, `POST`, `PUT`, `PATCH`, `DELETE`. |
| `RequestUrl` | Request URL | `InArgument` | `String` | Yes | | The Graph API endpoint path (e.g., "/me/messages"). |
| `ParametersVariable` | Parameters | `InArgument` | `Dictionary<String, String>` | No | | Query parameters as a dictionary. |
| `HeadersVariable` | Headers | `InArgument` | `Dictionary<String, String>` | No | | HTTP headers as a dictionary. |
| `Body` | Body | `InArgument` | `String` | No | | The request body (for POST, PUT, PATCH). |

### Configuration

| Name | Display Name | Type | Default | Description |
|------|-------------|------|---------|-------------|
| `ConnectionId` | Connection ID | `String` | | The Microsoft 365 connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `ResponseContent` | Response Content | `OutArgument` | `String` | The response body as a string. |
| `ResponseStatusCode` | Response Status Code | `OutArgument` | `Int32` | The HTTP status code. |
| `ResponseHeaders` | Response Headers | `OutArgument` | `Dictionary<String, String>` | The response headers. |
