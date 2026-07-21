# HTTP Request

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

> **Agent instruction — connection:** Before writing XAML, use the available tooling to resolve or search for a connection ID for the connector listed in this doc. If the connection ID cannot be resolved, leave `ConnectionId="{x:Null}"`.

`UiPath.GSuite.Activities.HttpRequestConnections`

Executes an authenticated HTTP request against a Google Workspace API endpoint.

**Package:** `UiPath.GSuite.Activities`
**Category:** General
**Connector:** `uipath-google-workspace`

## Properties

### Input

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `RequestMethod` | Method | `InArgument` | [`RequestMethod`](#requestmethod) | Yes | `GET` | Indicates the request method used. |
| `RequestUrl` | URL | `InArgument` | `String` | Yes | | Indicates the endpoint path for the request. |
| `ParametersInputMode` | Parameters input mode | `Property` | [`DictionaryInputMode`](#dictionaryinputmode) | No | `Build` | How to specify the parameters: individually or as a single dictionary variable. |
| `Parameters` | Query Parameters | `Property` | `Dictionary<String, InArgument<String>>` | No | | Key-value pairs of query parameters. Visible when `ParametersInputMode` is `Build`. |
| `ParametersVariable` | Query Parameters | `InArgument` | `Dictionary<String, String>` | No | | Query parameters supplied as a single dictionary variable. Visible when `ParametersInputMode` is `ByVariable`. |
| `HeadersInputMode` | Headers input mode | `Property` | [`DictionaryInputMode`](#dictionaryinputmode) | No | `Build` | How to specify the headers: individually or as a single dictionary variable. |
| `Headers` | Headers | `Property` | `Dictionary<String, InArgument<String>>` | No | | Key-value pairs of HTTP headers. Visible when `HeadersInputMode` is `Build`. |
| `HeadersVariable` | Headers | `InArgument` | `Dictionary<String, String>` | No | | HTTP headers supplied as a single dictionary variable. Visible when `HeadersInputMode` is `ByVariable`. |
| `Body` | Body | `InArgument` | `String` | No | | Indicates the body of the request (for POST, PUT, PATCH methods). |
| `Scopes` | Scopes | `Property` | `String[]` | No | | The OAuth scopes needed for the request to go through. |
| `ConnectionId` | Connection ID | `InArgument` | `string` | No | | The Google Workspace connection to use. |

### Output

| Name | Display Name | Kind | Type | Description |
|------|-------------|------|------|-------------|
| `ResponseContent` | Response Content | `OutArgument` | `String` | The response content received from the endpoint. |
| `ResponseStatus` | Response Status | `OutArgument` | `Int32` | The response status of the executed request. |
| `ResponseHeaders` | Response Headers | `OutArgument` | `Dictionary<String, String>` | The list of response headers and their values. |

## Enum Reference

### `RequestMethod`

| Value | Description |
|-------|-------------|
| `GET` | Request data from a resource (default). |
| `POST` | Send data to the server. |
| `PUT` | Update a resource with new data. |
| `PATCH` | Apply partial modifications to a resource. |
| `DELETE` | Delete a specified resource. |

### `DictionaryInputMode`

| Value | Description |
|-------|-------------|
| `Build` | Configure key-value pairs individually in the designer (default). |
| `ByVariable` | Supply a single `Dictionary<String, String>` variable. |

## XAML Example

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<gsuite:HttpRequestConnections
    DisplayName="HTTP Request"
    ConnectionId="{x:Null}"
    RequestMethod="[RequestMethod.GET]"
    RequestUrl="[endpointUrl]"
    ResponseContent="[responseBody]"
    ResponseStatus="[statusCode]"
    ResponseHeaders="[headers]" />
```

## Notes

- Prefer using this activity **outside** of [`GSuiteApplicationScope`](GSuiteApplicationScope.md). `*Connections` activities authenticate via Integration Service independently — no scope wrapper required. Place inside the scope only when Integration Service is unavailable or when using API Key, OAuth Client ID, or Service Account credentials directly via the scope.
- Authentication is handled automatically using the active Google Workspace connection credentials.
- Use this activity for Google API calls not covered by dedicated activities in this package.
- `Parameters`/`ParametersVariable` and `Headers`/`HeadersVariable` are mutually exclusive within each pair, controlled by their respective `InputMode` property.
