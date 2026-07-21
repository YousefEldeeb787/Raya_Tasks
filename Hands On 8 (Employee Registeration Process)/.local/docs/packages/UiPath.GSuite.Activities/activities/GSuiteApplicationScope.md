# Google Workspace Application Scope

> **Agent instruction — read all linked docs before proceeding:** Follow and read every hyperlinked reference document on this page in full before generating XAML. XAML structural patterns (BackupSlots, StoredValue, x:Reference, all-null attributes, namespace declarations) are defined in the linked component, type, and filter docs — not repeated here. If those linked docs also contain hyperlinks to other reference docs, follow those too.

`UiPath.GSuite.Activities.GSuiteApplicationScope`

Authentication scope for Google Workspace. Opens an authenticated session against Google APIs using API Key, OAuth Client ID, or Service Account credentials and exposes it to every Google Workspace activity placed in its body — classic activities **and** modern `*Connections` activities alike. Activities nested in the scope **do not require Integration Service**; the scope is the single source of authentication for all of them. Authentication scopes are calculated automatically at runtime from the child activities' `RequiredScopes`.

**Package:** `UiPath.GSuite.Activities`
**Category:** General
**Platform:** Windows only

## Properties

### General

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `ConfigLocation` | Connection method | `Property` | [`ActivityConfigLocation`](#activityconfiglocation) | No | `PropertiesPanel` | Whether credentials come from the properties panel or from an Orchestrator asset selected via `BrowserItemId`. |
| `AuthenticationType` | Authentication Type | `Property` | [`GoogleAuthenticationType`](#googleauthenticationtype) | Yes | `OAuthClientID` | The authentication method to use. Drives which sub-fields are required. |
| `Connector` | Select connector | `Property` | `String` | No | | Studio-side connector selector exposed by the base connection-service plumbing. Not user-meaningful on this scope — the scope authenticates every Google Workspace activity in its body regardless of their individual connector. |

### API Key Authentication

Required when `AuthenticationType = ApiKey`.

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `ApiKey` | API Key | `InArgument` | `String` | Conditional | | Google API key. Limited to read-only public data operations. |

### OAuth Client ID Authentication

Required when `AuthenticationType = OAuthClientID`.

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `OAuthClient` | OAuth Client | `Property` | [`OAuthClient`](#oauthclient) | No | `UIPATH` | Choose between the public UiPath OAuth application (`UIPATH`) or a custom one (`CUSTOM`). |
| `CredentialID` | Client ID | `InArgument` | `String` | Conditional | | OAuth 2.0 Client ID. Required when `OAuthClient = CUSTOM`. |
| `CredentialSecret` | Client Secret | `InArgument` | `String` | Conditional | | OAuth 2.0 Client Secret as plain string. Mutually exclusive with `SecureCredentialSecret`. |
| `SecureCredentialSecret` | Secure Client Secret | `InArgument` | `SecureString` | Conditional | | OAuth 2.0 Client Secret as a `SecureString`. Prefer this over `CredentialSecret`. |
| `User` | User | `InArgument` | `String` | No | | Hint identifying which user account to authenticate (used to disambiguate multi-account caches). |
| `DataStoreLocation` | Data Store Location | `Property` | [`DataStoreLocation`](#datastorelocation) | No | `DISK` | Where to persist the OAuth refresh token cache. |
| `Folder` | Folder | `InArgument` | `String` | No | | Orchestrator folder path for the credential asset when `DataStoreLocation = ORCHESTRATOR`. |

### Service Account Authentication

Required when `AuthenticationType = ServiceAccountKey`.

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `KeyType` | Key Type | `Property` | [`ServiceAccountKeyType`](#serviceaccountkeytype) | No | `JSON` | Key file format. `JSON` requires only `KeyPath`; `P12` additionally requires `ServiceAccountEmail` and `Password`. |
| `KeyPath` | Key Path | `InArgument` | `String` | Conditional | | File path to the service account key. Required for both `JSON` and `P12` key types. |
| `ServiceAccountEmail` | Service Account Email | `InArgument` | `String` | Conditional | | Service account email. Required when `KeyType = P12`. |
| `Password` | Password | `InArgument` | `String` | No | `notasecret` | Password for the P12 key file. |
| `HasDomainWideAccesss` | Has Domain Wide Access | `Property` | `Boolean` | No | `false` | Whether the service account is configured for domain-wide delegation. |
| `UserEmail` | User Email | `InArgument` | `String` | Conditional | | Email of the user to impersonate via domain-wide delegation. Required when `HasDomainWideAccesss = true`. |
| `BucketName` | Bucket Name | `InArgument` | `String` | No | | Orchestrator storage bucket name that contains the key file (alternative to `KeyPath`). |

### Orchestrator Asset Configuration

Required when `ConfigLocation = OrchestratorAsset`. In this mode the activity reads its authentication settings from an Orchestrator asset selected at design time; the per-`AuthenticationType` sections above are ignored.

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `BrowserItemId` | GSuite asset ID | `InArgument` | `String` | Yes | | ID of the Orchestrator asset that holds the connection configuration. Set by the Studio asset browser. |
| `BrowserParentItemId` | GSuite connection method | `InArgument` | `String` | No | | Parent folder/asset identifier emitted by the Studio browser. |
| `BrowserItemFriendlyName` | Connection assets | `InArgument` | `String` | No | | Human-readable name of the selected asset. |
| `BrowserItemFullPath` | GSuite Connection Asset Full Path | `InArgument` | `String` | No | | Full Orchestrator path of the selected asset. |

### Runtime Browser Configuration (Robot-side asset selection)

These properties let the robot resolve an Orchestrator asset at runtime rather than at design time. `RuntimeItemInputMode` selects whether to use the values picked in Studio (`Browse`) or a path provided dynamically (`FullPath`).

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `RuntimeItemInputMode` | Runtime connection asset input mode | `Property` | `EDriveItemMode` | No | `Browse` | `Browse` uses the Studio-picked runtime asset; `FullPath` uses `ManualRuntimeItemFullPath` to resolve at runtime. |
| `BrowserRuntimeItemId` | Runtime connection assets | `InArgument` | `String` | No | | Runtime asset ID emitted by the Studio browser. |
| `BrowserRuntimeParentItemId` | Runtime parent ID | `InArgument` | `String` | No | | Runtime parent asset ID emitted by the Studio browser. |
| `BrowserRuntimeItemFriendlyName` | Runtime connection assets | `InArgument` | `String` | No | | Runtime asset friendly name. |
| `BrowserRuntimeItemFullPath` | Runtime connection asset full path | `InArgument` | `String` | No | | Full Orchestrator path of the runtime asset (Studio-resolved). |
| `ManualRuntimeItemFullPath` | Runtime connection asset full path | `InArgument` | `String` | Conditional | | Manually-entered runtime asset path. Required when `RuntimeItemInputMode = FullPath`. |
| `BucketInputMode` | Bucket input mode | `Property` | `EDriveItemMode` | No | `FullPath` | Selection mode for `BucketName`/`BucketId` when the service-account key is stored in an Orchestrator storage bucket. |
| `BucketId` | Bucket ID | `InArgument` | `String` | No | | Orchestrator storage bucket ID (browsable=false; populated by the Studio bucket browser). |

### Authentication Scopes

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `AuthenticationScopes` | Authentication Scopes | `Property` | `List<InArgument<String>>` | No | empty | OAuth scope strings to request. Hidden in the designer — at runtime the scope set is auto-computed from the `RequiredScopes` of every child activity via `IScopeCalculatorService`. |

### Common

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `TimeoutMS` | Timeout (ms) | `InArgument` | `Int32` | No | | Per-call HTTP timeout in milliseconds. Also exposed as the project setting `UiPath.Sdk.Activities.GSuiteScope.TimeoutMS`. |
| `ContinueOnError` | Continue On Error | `InArgument` | `Boolean` | No | `false` | Whether failures inside the scope propagate or are swallowed. Also exposed as the project setting `UiPath.Sdk.Activities.GenericGSuite.ContinueOnError`. |
| `DisplayName` | Display name | `Property` | `String` | No | `Google Workspace Scope` | Activity name shown in the designer. |

### Body

| Name | Display Name | Kind | Type | Required | Default | Description |
|------|-------------|------|------|----------|---------|-------------|
| `Body` | (designer-only) | `Property` | `ActivityDelegate` (`ActivityAction<Object>`) | Yes | Empty `Sequence` named "Do" | The Do-sequence that holds child activities. Backwards-compatible: older XAMLs that used `ActivityAction<SimpleObjectContainer>` are upgraded transparently to `ActivityAction<Object>`. The delegate argument is named `GSuiteScope`. |

### Obsolete

| Name | Description |
|------|-------------|
| `Services` (`GoogleService`) | Obsolete. Use `AuthenticationScopes` instead. |

## Enum Reference

### `ActivityConfigLocation`

| Value | Description |
|-------|-------------|
| `PropertiesPanel` | Credentials are configured directly on the activity's properties. |
| `OrchestratorAsset` | Credentials are read from an Orchestrator asset selected via `BrowserItemId`. |

### `GoogleAuthenticationType`

| Value | Description |
|-------|-------------|
| `ApiKey` | Authenticate with a Google API Key. Read-only access to public data only. |
| `OAuthClientID` | Authenticate with OAuth 2.0 Client ID/Secret. First run launches an interactive consent flow. |
| `ServiceAccountKey` | Authenticate with a service account JSON or P12 key. Set `HasDomainWideAccesss = True` and `UserEmail` to impersonate a workspace user. |
| `IntegrationService` | Authenticate via a UiPath Integration Service connection. |

### `OAuthClient`

| Value | Description |
|-------|-------------|
| `UIPATH` | Use the UiPath-provided public OAuth application. No `CredentialID`/`CredentialSecret` required. |
| `CUSTOM` | Use your own OAuth application. Requires `CredentialID` and `CredentialSecret` (or `SecureCredentialSecret`). |

### `ServiceAccountKeyType`

| Value | Description |
|-------|-------------|
| `JSON` | JSON service account key. Requires `KeyPath`. |
| `P12` | P12/PKCS12 service account key. Requires `KeyPath`, `ServiceAccountEmail`, and `Password`. |

### `DataStoreLocation`

| Value | Description |
|-------|-------------|
| `DISK` | Store OAuth refresh tokens on the local disk under the robot user profile. |
| `ORCHESTRATOR` | Store OAuth refresh tokens as an Orchestrator credential asset (use `Folder` to target a folder). |
| `NO_STORAGE` | Never persist tokens; re-authenticate each run. |

## Valid Configurations

The scope has two top-level configuration paths driven by `ConfigLocation`:

**Mode A — Properties Panel (`ConfigLocation = PropertiesPanel`):** authentication fields on the activity itself are required, gated by `AuthenticationType`:

- `ApiKey` → set `ApiKey`.
- `OAuthClientID` + `OAuthClient = UIPATH` → no client credential fields required.
- `OAuthClientID` + `OAuthClient = CUSTOM` → set `CredentialID` and (`CredentialSecret` or `SecureCredentialSecret`).
- `ServiceAccountKey` + `KeyType = JSON` → set `KeyPath`. If `HasDomainWideAccesss = True`, also set `UserEmail`.
- `ServiceAccountKey` + `KeyType = P12` → set `KeyPath`, `ServiceAccountEmail`, and `Password`. If `HasDomainWideAccesss = True`, also set `UserEmail`.
- `IntegrationService` → no per-type fields required (handled by the base scope).

**Mode B — Orchestrator Asset (`ConfigLocation = OrchestratorAsset`):** set `BrowserItemId` (and usually the other `BrowserItem*` properties populated by Studio). Per-`AuthenticationType` fields are ignored. The runtime browser group (`BrowserRuntime*` / `ManualRuntimeItemFullPath`) further controls how the asset is resolved on the robot.

## XAML Examples

### Custom OAuth Client (Properties Panel)

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<gsuite:GSuiteApplicationScope
    DisplayName="Google Workspace Scope"
    ConfigLocation="PropertiesPanel"
    AuthenticationType="OAuthClientID"
    OAuthClient="CUSTOM"
    CredentialID="[clientId]"
    CredentialSecret="[clientSecret]"
    DataStoreLocation="DISK"
    User="[userHint]"
    TimeoutMS="[30000]"
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities">
    <gsuite:GSuiteApplicationScope.Body>
        <ActivityAction x:TypeArguments="x:Object">
            <ActivityAction.Argument>
                <DelegateInArgument x:TypeArguments="x:Object" Name="GSuiteScope" />
            </ActivityAction.Argument>
            <Sequence DisplayName="Do">
                <!-- Child GSuite activities here -->
            </Sequence>
        </ActivityAction>
    </gsuite:GSuiteApplicationScope.Body>
</gsuite:GSuiteApplicationScope>
```

### Service Account with Domain-Wide Delegation

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<gsuite:GSuiteApplicationScope
    DisplayName="Google Workspace Scope"
    ConfigLocation="PropertiesPanel"
    AuthenticationType="ServiceAccountKey"
    KeyType="JSON"
    KeyPath="[Path.Combine(projectFolder, &quot;sa-key.json&quot;)]"
    HasDomainWideAccesss="True"
    UserEmail="[impersonatedUser]"
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities">
    <gsuite:GSuiteApplicationScope.Body>
        <ActivityAction x:TypeArguments="x:Object">
            <Sequence DisplayName="Do" />
        </ActivityAction>
    </gsuite:GSuiteApplicationScope.Body>
</gsuite:GSuiteApplicationScope>
```

### API Key (read-only public data)

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<gsuite:GSuiteApplicationScope
    DisplayName="Google Workspace Scope"
    ConfigLocation="PropertiesPanel"
    AuthenticationType="ApiKey"
    ApiKey="[publicApiKey]">
    <gsuite:GSuiteApplicationScope.Body>
        <ActivityAction x:TypeArguments="x:Object">
            <Sequence DisplayName="Do" />
        </ActivityAction>
    </gsuite:GSuiteApplicationScope.Body>
</gsuite:GSuiteApplicationScope>
```

### Orchestrator Asset (auth config stored in Orchestrator)

```xml
<!--
    Namespace declarations for the enclosing root <Activity> element:
    xmlns:gsuite="clr-namespace:UiPath.GSuite.Activities;assembly=UiPath.GSuite.Activities"
-->
<gsuite:GSuiteApplicationScope
    DisplayName="Google Workspace Scope"
    ConfigLocation="OrchestratorAsset"
    BrowserItemId="687031"
    BrowserItemFriendlyName="GSuite-SAC-Local"
    BrowserItemFullPath="uipath.settings.config"
    BrowserParentItemId="934580"
    RuntimeItemInputMode="Browse">
    <gsuite:GSuiteApplicationScope.Body>
        <ActivityAction x:TypeArguments="x:Object">
            <Sequence DisplayName="Do" />
        </ActivityAction>
    </gsuite:GSuiteApplicationScope.Body>
</gsuite:GSuiteApplicationScope>
```

## Project Settings

The scope reads these defaults from project-level settings (`Project Settings` in Studio):

| Property | Setting Key | Default | Description |
|----------|-------------|---------|-------------|
| `TimeoutMS` | `UiPath.Sdk.Activities.GSuiteScope.TimeoutMS` | (`AuthConstants.TimeoutMs`) | Default HTTP timeout for child Google API calls. |
| `ContinueOnError` | `UiPath.Sdk.Activities.GenericGSuite.ContinueOnError` | `false` | Default value of `ContinueOnError` for child activities. |

## Notes

- **Windows only** — not available in cross-platform robots.
- **Provides full functionality without Integration Service for every Google Workspace activity — classic and `*Connections` alike.** Classic GSuite activities always rely on this scope for authentication. Modern `*Connections` activities (e.g., `CopyFileConnections`, `SendEmailConnections`, `HttpRequestConnections`) normally resolve their own `ConnectionId` via Integration Service when used at the top level — but when nested *inside* a `GSuiteApplicationScope`, they auto-detect the parent (`HasAuthProviderParent`), hide their `ConnectionId` in the designer, set `UseConnectionService = False`, and delegate authentication to the scope. The scope therefore authenticates **every Google Workspace activity in its body** with API Key / OAuth / Service Account credentials — useful for offline scenarios, custom OAuth clients, service-account auth, domain-wide delegation, or any flow that Integration Service can't (or shouldn't) handle.
- **Authentication scopes are auto-computed.** `AuthenticationScopes` is browsable=false; the runtime walks the body, collects `RequiredScopes` from every `IScopedClientActivity` child, and asks `IScopeCalculatorService` for the minimum union. Do not set this property manually unless you also disable scope calculation.
- `CredentialSecret` and `SecureCredentialSecret` are mutually exclusive — set exactly one when `OAuthClient = CUSTOM`.
- `User` is only a disambiguation hint for the on-disk token cache; it does not change which Google identity authenticates.
- The `Body` argument name is `GSuiteScope`. Older XAMLs serialized as `ActivityAction<SimpleObjectContainer>` are upgraded transparently to `ActivityAction<Object>` by the `Body` setter.
- The internal `IGoogleWorkspaceTarget` (the runtime payload passed to children) is not part of the design-time contract and is not exposed via XAML.
