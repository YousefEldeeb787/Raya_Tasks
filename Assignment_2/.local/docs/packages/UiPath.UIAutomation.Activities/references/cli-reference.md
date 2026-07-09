# UIAutomation CLI Reference

Complete CLI reference for the UiPath UIAutomation tool. The CLI entry point is `uip rpa uia` (e.g., `uip rpa uia object-repository get-apps`).

## Common Options

All commands accept the following option:

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `project-dir` | `--project-dir <project-dir>` | The project directory. | No (defaults to current directory) |

---

## Object Repository

Commands for managing UIA entries in Object Repository.

**Base command:** `uip rpa uia object-repository`

### Applications

#### Create Application

Creates an application entry in the Object Repository.

```bash
uip rpa uia object-repository create-app [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `name` | `--name <name>` | The display name for the application entry in the Object Repository. | **Yes** |
| `description` | `--description <description>` | An optional description for the entry. | No |
| `version` | `--version <version>` | The version for the application entry. Defaults to "1.0.0" if not provided. | No |

**Example:**

```bash
# Create an application with a name (version defaults to "1.0.0")
uip rpa uia object-repository create-app --name "My Web App"

# Create an application with a name and description
uip rpa uia object-repository create-app --name "My Web App" --description "The main web application"

# Create an application with a specific version
uip rpa uia object-repository create-app --name "My Web App" --version "2.0.0"
```

#### Get Applications

Gets all applications from the Object Repository.

```bash
uip rpa uia object-repository get-apps [options]
```

No additional options beyond the common options.

**Example:**

```bash
# List all applications in the current project
uip rpa uia object-repository get-apps

# List all applications in a specific project
uip rpa uia object-repository get-apps --project-dir "C:\MyProject"
```

#### Get Application

Gets an application from the Object Repository by reference ID.

```bash
uip rpa uia object-repository get-app [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `reference-id` | `--reference-id <reference-id>` | The reference ID of the application entry. | **Yes** |

**Example:**

```bash
uip rpa uia object-repository get-app --reference-id "app-123"
```

#### Delete Application

Deletes an application entry from the Object Repository.

```bash
uip rpa uia object-repository delete-app [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `reference-id` | `--reference-id <reference-id>` | The reference ID of the application entry to delete. | **Yes** |

**Example:**

```bash
uip rpa uia object-repository delete-app --reference-id "app-123"
```

### Screens

#### Create Screen

Creates a screen entry in the Object Repository from a definition file.

```bash
uip rpa uia object-repository create-screen [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `definition-file-path` | `--definition-file-path <definition-file-path>` | The path to the definition file. | **Yes** |
| `app-reference-id` | `--app-reference-id <app-reference-id>` | The reference ID of the application under which the screen should be created. If not provided, a new application will be created automatically. | No |

**Example:**

```bash
# Create a screen from a definition file (a new application is created automatically)
uip rpa uia object-repository create-screen --definition-file-path "path/to/login.xaml"

# Create a screen under an existing application
uip rpa uia object-repository create-screen --definition-file-path "path/to/login.xaml" --app-reference-id "app-123"
```

#### Get Screens

Gets screens from the Object Repository, optionally filtered by the provided options.

```bash
uip rpa uia object-repository get-screens [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `definition-file-path` | `--definition-file-path <definition-file-path>` | The path to the application definition file containing the window selector. | No |
| `app-reference-id` | `--app-reference-id <app-reference-id>` | The reference ID of the application to filter screens by. | No |

**Example:**

```bash
# Get all screens
uip rpa uia object-repository get-screens

# Get screens matching a definition file's window selector
uip rpa uia object-repository get-screens --definition-file-path "path/to/definition.xaml"

# Get screens filtered by application
uip rpa uia object-repository get-screens --app-reference-id "app-123"
```

#### Get Screen

Gets a screen from the Object Repository by reference ID.

```bash
uip rpa uia object-repository get-screen [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `reference-id` | `--reference-id <reference-id>` | The reference ID of the screen entry. | **Yes** |

**Example:**

```bash
uip rpa uia object-repository get-screen --reference-id "screen-456"
```

#### Get Screen XAML

Gets the XAML representation of a screen from the Object Repository.

```bash
uip rpa uia object-repository get-screen-xaml [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `reference-id` | `--reference-id <reference-id>` | The reference ID of the screen entry. | **Yes** |

**Example:**

```bash
uip rpa uia object-repository get-screen-xaml --reference-id "screen-456"
```

#### Delete Screen

Deletes a screen entry from the Object Repository.

```bash
uip rpa uia object-repository delete-screen [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `reference-id` | `--reference-id <reference-id>` | The reference ID of the screen entry to delete. | **Yes** |

**Example:**

```bash
uip rpa uia object-repository delete-screen --reference-id "screen-456"
```

### Elements

#### Create Elements

Creates multiple element entries in the Object Repository from definition files in a single batch.

```bash
uip rpa uia object-repository create-elements [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `screen-reference-id` | `--screen-reference-id <screen-reference-id>` | The reference ID of the screen entry under which the elements should be created. | **Yes** |
| `definition-file-paths` | `--definition-file-paths <definition-file-paths>` | Comma-separated list of definition file paths (e.g., `path1.xaml,path2.xaml`). | **Yes** |

**Example:**

```bash
# Create multiple elements under a screen
uip rpa uia object-repository create-elements \
  --screen-reference-id "screen-456" \
  --definition-file-paths "path/to/username.xaml,path/to/password.xaml"
```

#### Get Elements

Gets elements from the Object Repository for a given screen.

```bash
uip rpa uia object-repository get-elements [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `screen-reference-id` | `--screen-reference-id <screen-reference-id>` | The reference ID of the screen entry to get elements from. | **Yes** |

**Example:**

```bash
uip rpa uia object-repository get-elements --screen-reference-id "screen-456"
```

#### Get Element

Gets an element from the Object Repository by reference ID.

```bash
uip rpa uia object-repository get-element [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `reference-id` | `--reference-id <reference-id>` | The reference ID of the element entry. | **Yes** |

**Example:**

```bash
uip rpa uia object-repository get-element --reference-id "elem-789"
```

#### Get Elements XAML

Gets the XAML representation of multiple elements from the Object Repository in a single batch.

```bash
uip rpa uia object-repository get-elements-xaml [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `reference-ids` | `--reference-ids <reference-ids>` | Comma-separated element reference IDs (e.g., `ref-123,ref-456`). Element names are read from the Object Repository. | **Yes** |

**Example:**

```bash
uip rpa uia object-repository get-elements-xaml --reference-ids "elem-789,elem-012"
```

#### Delete Element

Deletes an element entry from the Object Repository.

```bash
uip rpa uia object-repository delete-element [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `reference-id` | `--reference-id <reference-id>` | The reference ID of the element entry to delete. | **Yes** |

**Example:**

```bash
uip rpa uia object-repository delete-element --reference-id "elem-789"
```

### Linking

#### Link Elements

Links one or more Object Repository elements to activities in the XAML workflow.

```bash
uip rpa uia object-repository link-elements [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `elements` | `--elements <json>` | JSON array of link entries. Each entry has the shape `{"workflowFilePath":"Main.xaml","activityId":"ACT","referenceId":"REF","targetProperty":"Target"}`. `workflowFilePath` (relative to the project directory or absolute inside it), `activityId`, and `referenceId` are required. `targetProperty` is optional and defaults to `"Target"`; use dot-separated names for nested properties (e.g., `"SearchedElement.Target"`). | **Yes** |

**Example:**

```bash
# Link a single element (target property defaults to "Target")
uip rpa uia object-repository link-elements \
  --elements '[{"workflowFilePath":"Main.xaml","activityId":"act-123","referenceId":"elem-789"}]'

# Link multiple elements with per-activity target properties (and per-entry workflow paths)
uip rpa uia object-repository link-elements \
  --elements '[{"workflowFilePath":"Main.xaml","activityId":"act-123","referenceId":"elem-789","targetProperty":"Target"},{"workflowFilePath":"Main.xaml","activityId":"act-456","referenceId":"elem-012","targetProperty":"SearchedElement.Target"}]'
```

#### Link Screen

Links an Object Repository screen to an activity in the XAML workflow.

```bash
uip rpa uia object-repository link-screen [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `workflow-file-path` | `--workflow-file-path <workflow-file-path>` | The path to the .xaml file. Can be a relative path from the project directory or a full path inside the project directory. | **Yes** |
| `activity-id` | `--activity-id <activity-id>` | The activity reference ID available on the activity in XAML. | **Yes** |
| `reference-id` | `--reference-id <reference-id>` | The reference ID of the screen from Object Repository to link to the activity. | **Yes** |

**Example:**

```bash
uip rpa uia object-repository link-screen \
  --workflow-file-path "Main.xaml" \
  --activity-id "act-456" \
  --reference-id "screen-789"
```

### Definitions

#### Get Element Definition

Gets the definition of an element from the Object Repository.

```bash
uip rpa uia object-repository get-element-definition [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `folder-path` | `--folder-path <folder-path>` | The folder path for input/output data. | **Yes** |
| `definition-file-path` | `--definition-file-path <definition-file-path>` | Path to the target definition XAML file. | **Yes** |
| `reference-id` | `--reference-id <reference-id>` | The reference ID of the element entry. | **Yes** |

**Example:**

```bash
uip rpa uia object-repository get-element-definition \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/target.xaml" \
  --reference-id "elem-789"
```

#### Get Screen Definition

Gets the definition of a screen from the Object Repository.

```bash
uip rpa uia object-repository get-screen-definition [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `folder-path` | `--folder-path <folder-path>` | The folder path for input/output data. | **Yes** |
| `definition-file-path` | `--definition-file-path <definition-file-path>` | Path to the target definition JSON file. | **Yes** |
| `reference-id` | `--reference-id <reference-id>` | The reference ID of the screen entry. | **Yes** |

**Example:**

```bash
uip rpa uia object-repository get-screen-definition \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/target.xaml" \
  --reference-id "screen-456"
```

---

## Target Anchorable

Commands for target anchorable operations.

**Base command:** `uip rpa uia target-anchorable`

### Link

Links one or more target anchorable definition files to activities in the XAML workflow.

```bash
uip rpa uia target-anchorable link [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `targets` | `--targets <json>` | JSON array of link entries. Each entry has the shape `{"workflowFilePath":"Main.xaml","activityId":"ACT","definitionFilePath":"path/to/target.xaml","targetProperty":"Target"}`. `workflowFilePath` (relative to the project directory or absolute inside it), `activityId`, and `definitionFilePath` are required. `targetProperty` is optional; use dot-separated names for nested properties (e.g., `"SearchedElement.Target"`). | **Yes** |

**Example:**

```bash
# Link a single target anchorable
uip rpa uia target-anchorable link \
  --targets '[{"workflowFilePath":"Main.xaml","activityId":"act-123","definitionFilePath":"path/to/target.xaml"}]'

# Link multiple target anchorables with per-activity target properties (and per-entry workflow paths)
uip rpa uia target-anchorable link \
  --targets '[{"workflowFilePath":"Main.xaml","activityId":"act-123","definitionFilePath":"path/to/t1.xaml"},{"workflowFilePath":"Main.xaml","activityId":"act-456","definitionFilePath":"path/to/t2.xaml","targetProperty":"SearchedElement.Target"}]'
```

### Get Definition

Gets the definition of a target anchorable.

```bash
uip rpa uia target-anchorable get-definition [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `folder-path` | `--folder-path <folder-path>` | The folder path for input/output data. | **Yes** |
| `definition-file-path` | `--definition-file-path <definition-file-path>` | Path to the target definition JSON file. | **Yes** |
| `activity-id` | `--activity-id <activity-id>` | The activity reference ID available on the activity in XAML. | **Yes** |
| `workflow-file-path` | `--workflow-file-path <workflow-file-path>` | The path to the workflow file. Can be a relative path from the project directory or a full path inside the project directory. | **Yes** |
| `target-property` | `--target-property <target-property>` | The name of the property where the target will be attached on the activity. For nested properties, use path-like structures (e.g., `SearchedElement.Target`). Defaults to `Target`. | No |

**Example:**

```bash
uip rpa uia target-anchorable get-definition \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/target.xaml" \
  --activity-id "act-1" \
  --workflow-file-path "workflows/Main.xaml"

# With a custom target property
uip rpa uia target-anchorable get-definition \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/target.xaml" \
  --activity-id "act-1" \
  --workflow-file-path "workflows/Main.xaml" \
  --target-property "SearchedElement.Target"
```

### Update Definition

Updates a target anchorable definition file. Only provided options are updated; omitted options are left unchanged.

```bash
uip rpa uia target-anchorable update-definition [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `definition-file-path` | `--definition-file-path <definition-file-path>` | The path to the definition file to update. | **Yes** |
| `name` | `--name <name>` | The display name for the target anchorable. | No |
| `description` | `--description <description>` | An optional description for the entry. | No |
| `scope-selector` | `--scope-selector <scope-selector>` | The scope selector. | No |
| `full-selector` | `--full-selector <full-selector>` | The full selector for the element. | No |
| `semantic-selector` | `--semantic-selector <semantic-selector>` | The semantic selector for the element. | No |
| `activity-type` | `--activity-type <activity-type>` | The activity type. Available values: None, Click, TypeInto, GetText, Check, Hover, Highlight, SelectItem, GetAttribute, TakeScreenshot, DragAndDrop, KeyboardShortcut, MouseScroll, ExtractData, SetText, CheckState, FindElements, InjectJsScript, CheckElement. | No |

**Example:**

```bash
# Update the name and full selector
uip rpa uia target-anchorable update-definition \
  --definition-file-path "path/to/target.xaml" \
  --name "Login Button" \
  --full-selector "<wnd cls='LoginForm' /><ctrl name='Login' role='button' />"

# Update the scope selector and activity type
uip rpa uia target-anchorable update-definition \
  --definition-file-path "path/to/target.xaml" \
  --scope-selector "<wnd cls='LoginForm' />" \
  --activity-type "Click"
```

### Resolve Default

Resolve the default target anchorables from snapshot refs and write the target definition files.

```bash
uip rpa uia target-anchorable resolve-default [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `folder-path` | `--folder-path <folder-path>` | The folder path for input/output data. | **Yes** |
| `refs` | `--refs <json>` | JSON array of refs to resolve. Each entry has the shape `{"ref":"REF","definitionFilePath":"path/to/target.xaml","activityType":"Click"}`. `activityType` is optional and may differ per entry. Available activity types: None, Click, TypeInto, GetText, Check, Hover, Highlight, SelectItem, GetAttribute, TakeScreenshot, DragAndDrop, KeyboardShortcut, MouseScroll, ExtractData, SetText, CheckState, FindElements, InjectJsScript, CheckElement. | **Yes** |
| `window-definition-file-path` | `--window-definition-file-path <window-definition-file-path>` | Path to the window definition (XAML) used to seed the scope selector. | **Yes** |
| `source` | `--source <source>` | Which tree to use: `window` or `application`. | No |
| `from-snapshot` | `--from-snapshot` | Return the selector from the captured tree snapshot without probing the live element. | No |

**Example:**

```bash
# Resolve a single anchorable target
uip rpa uia target-anchorable resolve-default \
  --folder-path "path/to/data" \
  --refs '[{"ref":"e42","definitionFilePath":"path/to/target.xaml","activityType":"Click"}]' \
  --window-definition-file-path "path/to/window.xaml"

# Resolve multiple targets in one call (per-entry activity types)
uip rpa uia target-anchorable resolve-default \
  --folder-path "path/to/data" \
  --refs '[{"ref":"e28","definitionFilePath":"path1.xaml","activityType":"Click"},{"ref":"e30","definitionFilePath":"path2.xaml","activityType":"TypeInto"}]' \
  --window-definition-file-path "path/to/window.xaml"

# Resolve from snapshot
uip rpa uia target-anchorable resolve-default \
  --folder-path "path/to/data" \
  --refs '[{"ref":"e42","definitionFilePath":"path/to/target.xaml"}]' \
  --window-definition-file-path "path/to/window.xaml" \
  --from-snapshot
```

> **Tip:** Inline JSON with Windows `C:\…` paths fails because `\U` etc. are invalid JSON escapes. Use forward slashes (`C:/Users/...`) inside the JSON. The same applies to `--elements` / `--targets` on the link commands.

### Add Anchor

Resolve an anchor element from a snapshot ref and append it to the target anchorable definition. A target anchorable can hold up to four anchors (slots `0..3`); the new anchor is written to the next free slot and the command returns that slot index.

```bash
uip rpa uia target-anchorable add-anchor [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `folder-path` | `--folder-path <folder-path>` | The folder path for input/output data. | **Yes** |
| `ref` | `--ref <ref>` | The snapshot ref (e.g., `e28`) of the anchor element to add. | **Yes** |
| `definition-file-path` | `--definition-file-path <definition-file-path>` | The path to the target anchorable definition file to update. | **Yes** |
| `from-snapshot` | `--from-snapshot` | Return the selector from the captured tree snapshot without probing the live element. | No |
| `name` | `--name <name>` | The display name for the anchor entry. | No |
| `description` | `--description <description>` | An optional description for the entry. | No |

**Example:**

```bash
# Add an anchor resolved from the live element
uip rpa uia target-anchorable add-anchor \
  --folder-path "path/to/data" \
  --ref "e28" \
  --definition-file-path "path/to/target.xaml"

# Add a named anchor from the captured snapshot
uip rpa uia target-anchorable add-anchor \
  --folder-path "path/to/data" \
  --ref "e28" \
  --definition-file-path "path/to/target.xaml" \
  --from-snapshot \
  --name "TitleBar" \
  --description "The window title bar"
```

### Remove Anchor

Remove the anchor at the specified index from the target anchorable definition. Metadata slots shift down so they stay aligned with the remaining anchors.

```bash
uip rpa uia target-anchorable remove-anchor [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `definition-file-path` | `--definition-file-path <definition-file-path>` | The path to the target anchorable definition file to update. | **Yes** |
| `index` | `--index <index>` | The anchor slot index (`0..3`) within the target anchorable definition. | **Yes** |

**Example:**

```bash
# Remove the first anchor
uip rpa uia target-anchorable remove-anchor \
  --definition-file-path "path/to/target.xaml" \
  --index 0
```

### Update Anchor Definition

Update properties (name, description, full selector) of the anchor at the specified index on the target anchorable definition. Only provided options are updated; omitted options are left unchanged.

```bash
uip rpa uia target-anchorable update-anchor-definition [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `definition-file-path` | `--definition-file-path <definition-file-path>` | The path to the target anchorable definition file to update. | **Yes** |
| `index` | `--index <index>` | The anchor slot index (`0..3`) within the target anchorable definition. | **Yes** |
| `name` | `--name <name>` | The display name for the anchor entry. | No |
| `description` | `--description <description>` | An optional description for the entry. | No |
| `full-selector` | `--full-selector <full-selector>` | The full selector for the anchor element. | No |

**Example:**

```bash
# Rename an anchor and tweak its description
uip rpa uia target-anchorable update-anchor-definition \
  --definition-file-path "path/to/target.xaml" \
  --index 0 \
  --name "TitleBar" \
  --description "The window title bar"

# Replace the anchor's full selector
uip rpa uia target-anchorable update-anchor-definition \
  --definition-file-path "path/to/target.xaml" \
  --index 1 \
  --full-selector "<ctrl name='OK' role='button' />"
```

### Fuzzify

Fuzzifies the selector of one or more target anchorables. For each definition, the strict full selector is converted to a fuzzy selector: the fuzzy search step is enabled and the strict selector step is disabled. Each definition is processed independently, so a failure on one entry does not abort the others — the per-file outcome (`OK` or `ERROR`) is reported for every entry.

```bash
uip rpa uia target-anchorable fuzzify [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `definition-file-paths` | `--definition-file-paths <json>` | JSON array of target anchorable definition file paths to fuzzify (e.g., `["path/to/t1.xaml","path/to/t2.xaml"]`). | **Yes** |

A definition fails to fuzzify (reported as `ERROR`) when its fuzzy selector is already enabled, when no strict selector is enabled to convert, or when fuzzy selector generation fails.

**Example:**

```bash
# Fuzzify a single target anchorable
uip rpa uia target-anchorable fuzzify \
  --definition-file-paths '["path/to/target.xaml"]'

# Fuzzify several target anchorables in one call
uip rpa uia target-anchorable fuzzify \
  --definition-file-paths '["path/to/t1.xaml","path/to/t2.xaml"]'
```

### Fuzzify Anchor

Fuzzifies the selector of an anchor within a target anchorable definition file. Works like `fuzzify`, but targets the anchor at the given slot index rather than the target itself. Each entry is processed independently and reports its own `OK`/`ERROR` outcome.

```bash
uip rpa uia target-anchorable fuzzify-anchor [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `anchors` | `--anchors <json>` | JSON array of anchors to fuzzify. Each entry has the shape `{"definitionFilePath":"path/to/target.xaml","index":0}`. `definitionFilePath` is required and `index` (the anchor slot, `0..3`) must be non-negative. | **Yes** |

An anchor fails to fuzzify (reported as `ERROR`) when the index is out of range, when its fuzzy selector is already enabled, when no strict selector is enabled to convert, or when fuzzy selector generation fails.

**Example:**

```bash
# Fuzzify the first anchor of a target anchorable
uip rpa uia target-anchorable fuzzify-anchor \
  --anchors '[{"definitionFilePath":"path/to/target.xaml","index":0}]'

# Fuzzify anchors across multiple definitions in one call
uip rpa uia target-anchorable fuzzify-anchor \
  --anchors '[{"definitionFilePath":"path/to/t1.xaml","index":0},{"definitionFilePath":"path/to/t2.xaml","index":1}]'
```

---

## Target App

Commands for target application operations.

**Base command:** `uip rpa uia target-app`

### Link

Links a target application to an activity in the XAML workflow.

```bash
uip rpa uia target-app link [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `definition-file-path` | `--definition-file-path <definition-file-path>` | Path to the target definition XAML file. | **Yes** |
| `workflow-file-path` | `--workflow-file-path <workflow-file-path>` | The path to the .xaml file. Can be a relative path from the project directory or a full path inside the project directory. | **Yes** |
| `activity-id` | `--activity-id <activity-id>` | The activity reference ID available on the activity in XAML. | **Yes** |

**Example:**

```bash
uip rpa uia target-app link \
  --definition-file-path "path/to/app.xaml" \
  --workflow-file-path "Main.xaml" \
  --activity-id "act-123"
```

### Get Definition

Gets the definition of a target application.

```bash
uip rpa uia target-app get-definition [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `folder-path` | `--folder-path <folder-path>` | The folder path for input/output data. | **Yes** |
| `definition-file-path` | `--definition-file-path <definition-file-path>` | Path to the target definition XAML file. | **Yes** |
| `activity-id` | `--activity-id <activity-id>` | The activity reference ID available on the activity in XAML. | **Yes** |
| `workflow-file-path` | `--workflow-file-path <workflow-file-path>` | The path to the workflow file. Can be a relative path from the project directory or a full path inside the project directory. | **Yes** |

**Example:**

```bash
uip rpa uia target-app get-definition \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/target.xaml" \
  --activity-id "act-1" \
  --workflow-file-path "workflows/Main.xaml"
```

### Update Definition

Updates a target app definition file. Only provided options are updated; omitted options are left unchanged.

```bash
uip rpa uia target-app update-definition [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `definition-file-path` | `--definition-file-path <definition-file-path>` | The path to the definition file to update. | **Yes** |
| `name` | `--name <name>` | The display name for the target application. | No |
| `description` | `--description <description>` | An optional description for the entry. | No |
| `selector` | `--selector <selector>` | The selector. | No |

**Example:**

```bash
# Update the name and selector
uip rpa uia target-app update-definition \
  --definition-file-path "path/to/app.xaml" \
  --name "My Web App" \
  --selector "<wnd cls='MainWindow' />"
```

### Resolve Default

Resolve the target apps from snapshot refs and write the target definition files.

```bash
uip rpa uia target-app resolve-default [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `folder-path` | `--folder-path <folder-path>` | The folder path for input/output data. | **Yes** |
| `refs` | `--refs <json>` | JSON array of refs to resolve. Each entry has the shape `{"ref":"REF","definitionFilePath":"path/to/target.xaml"}`. | **Yes** |
| `name` | `--name <name>` | The display name for the target application. | **Yes** |
| `description` | `--description <description>` | An optional description for the entry. | No |
| `source` | `--source <source>` | Which tree to use: `window` or `application`. | No |
| `from-snapshot` | `--from-snapshot` | Return the selector from the captured tree snapshot without probing the live element. | No |

**Example:**

```bash
# Resolve a target app from a window ref
uip rpa uia target-app resolve-default \
  --folder-path "path/to/data" \
  --refs '[{"ref":"w1","definitionFilePath":"path/to/app.xaml"}]' \
  --name "My Web App"

# Resolve from snapshot with a description
uip rpa uia target-app resolve-default \
  --folder-path "path/to/data" \
  --refs '[{"ref":"w1","definitionFilePath":"path/to/app.xaml"}]' \
  --name "My Web App" \
  --description "The main web application" \
  --from-snapshot
```

---

## Snapshot

Snapshot commands for capturing and inspecting application DOM data.

**Base command:** `uip rpa uia snapshot`

**Refs and the snapshot lifecycle.** `uia interact` resolves refs against the most recent snapshot, no matter which command produced it. Each new snapshot replaces prior refs:

- **Top-level snapshots** reset **all** refs and mint fresh `wN`/`bN` — they carry no `eN` refs.
- **App-level snapshots** reset and re-mint the `eN` refs. `wN`/`bN` remain valid **as long as** the top-level snapshot is unchanged; if a new top-level snapshot is captured/loaded, `wN`/`bN` are reset too.
- An old ref is **not necessarily rejected as stale** — it might silently resolve to whatever element now carries that number in the current snapshot.

### Capture

Capture the DOM snapshot of a live application.

```bash
uip rpa uia snapshot capture [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `folder-path` | `--folder-path <folder-path>` | The folder path for input/output data. | **Yes** |
| `definition-file-path` | `--definition-file-path <definition-file-path>` | Path to the target definition JSON file. When omitted, a snapshot of the top-level windows is captured. | No |
| `framework` | `--framework <framework>` | The UI scanning framework to use for DOM snapshot creation. Available values: Default, UIA, AA, Java. | No |

**Example:**

```bash
# Capture a snapshot scoped to a target definition
uip rpa uia snapshot capture \
  --folder-path "path/to/output" \
  --definition-file-path "path/to/target.xaml"

# Capture a snapshot with the elements from the window's tree
uip rpa uia snapshot capture \
  --folder-path "path/to/output" \
  --definition-file-path "path/to/window.xaml"

# Capture a snapshot of the top-level windows (no target definition)
uip rpa uia snapshot capture \
  --folder-path "path/to/output"
```

### Inspect

Inspect top-level targets (windows and browser tabs), or drill into one to extract its element tree.

```bash
uip rpa uia snapshot inspect [wb-ref] [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `wb-ref` | `[wb-ref]` (positional) | Window or browser tab target reference (e.g. `w1` or `b3`). Omit to list all top-level targets. | No |
| `framework` | `--framework <Default\|AA\|UIA\|Java>` | UI scanning framework. `Default` is recommended for most apps, especially browsers. [default: `Default`] | No |
| `no-filter` | `--no-filter` | Disable all window filtering (show all top-level windows). | No |
| `visualize` | `--visualize` | Briefly highlight the target element during the operation (for visual confirmation). | No |

**Example:**

```bash
# List top-level targets (windows + browser tabs)
uip rpa uia snapshot inspect

# Drill into a window to get element refs
uip rpa uia snapshot inspect w1

# Drill into a browser tab with a specific framework
uip rpa uia snapshot inspect b3 --framework UIA

# Show all top-level windows without filtering
uip rpa uia snapshot inspect --no-filter
```

---

## Selector Intelligence

Selector Intelligence commands for instructions and validation.

**Base command:** `uip rpa uia selector-intelligence`


### Get Instructions

Get instructions based on the runtime data.

```bash
uip rpa uia selector-intelligence get-instructions [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `folder-path` | `--folder-path <folder-path>` | The folder path for input/output data. | **Yes** |
| `definition-file-path` | `--definition-file-path <definition-file-path>` | Path to the target definition JSON file. | **Yes** |
| `mode` | `--mode <mode>` | Instructions mode: `recover` or `improve`. | No |

**Example:**

```bash
# Get recovery instructions
uip rpa uia selector-intelligence get-instructions \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/target.xaml" \
  --mode "recover"

# Get improvement instructions
uip rpa uia selector-intelligence get-instructions \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/target.xaml" \
  --mode "improve"
```

### Validate

Performs validation of a selector based on runtime data.

```bash
uip rpa uia selector-intelligence validate [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `folder-path` | `--folder-path <folder-path>` | The folder path for input/output data. | **Yes** |
| `definition-file-path` | `--definition-file-path <definition-file-path>` | Path to the target definition JSON file. | **Yes** |
| `improve-selector-response-file-path` | `--improve-selector-response-file-path <improve-selector-response-file-path>` | Improve selector response file path. | No |
| `mode` | `--mode <mode>` | Validation mode: `recover` or `improve`. | No |

**Example:**

```bash
# Validate in recover mode
uip rpa uia selector-intelligence validate \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/target.xaml" \
  --mode "recover"

# Validate in improve mode with response file
uip rpa uia selector-intelligence validate \
  --folder-path "path/to/data" \
  --definition-file-path "path/to/target.xaml" \
  --mode "improve" \
  --improve-selector-response-file-path "path/to/response.xaml"
```

---

## Interact

Commands for driving live UI interactions against the current snapshot. Each verb operates on a reference (`wN`, `bN`, or `eN`) produced by `uip rpa uia snapshot inspect` or `uip rpa uia snapshot capture`.

**Base command:** `uip rpa uia interact`

> **Do NOT pass `--folder-path` to `uip rpa uia interact`.** It is not a valid argument for this command — `uia interact` operates against the latest snapshot.

Use these commands when the UIA snapshot is already loaded (for example, immediately after a `snapshot inspect` call or a `uia-configure-target` capture of the current screen), so the refs are still valid. Refs from before the latest snapshot are re-interpreted against it — they may resolve to a different element or fail; see [Snapshot](#snapshot). Every leaf and sub-verb accepts `--visualize` to briefly highlight the target for visual confirmation.

### Click

Click a UI element.

```bash
uip rpa uia interact click <e-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `e-ref` | `<e-ref>` (positional) | Element reference from the current snapshot (e.g. `e42`). | **Yes** |
| `button` | `--button <Left\|Right\|Middle>` | Mouse button. [default: `Left`] | No |
| `type` | `--type <Single\|Double\|Down\|Up>` | Click type. [default: `Single`] | No |
| `input-method` | `--input-method <HardwareEvents\|WindowMessages\|Simulate\|WebBrowserDebugger>` | Input method for interacting with the target. [default: `HardwareEvents`] | No |
| `modifiers` | `--modifiers <Alt\|Ctrl\|Shift\|Win>` | Key modifiers to hold while clicking. Combine values with commas (e.g. `"Ctrl,Shift"`). | No |
| `origin` | `--origin <Center\|TopLeft\|TopRight\|BottomLeft\|BottomRight>` | Reference point within the element for the click offset. [default: `Center`] | No |
| `offset-x` | `--offset-x <integer>` | Horizontal pixel offset from the chosen origin. [default: `0`] | No |
| `offset-y` | `--offset-y <integer>` | Vertical pixel offset from the chosen origin. [default: `0`] | No |
| `visualize` | `--visualize` | Briefly highlight the target during the operation. | No |

**Example:**

```bash
# Single left click
uip rpa uia interact click e42

# Right-click with modifier
uip rpa uia interact click e42 --button Right --modifiers "Shift,Ctrl"

# Double click with pixel offset
uip rpa uia interact click e42 --type Double --origin TopLeft --offset-x 5 --offset-y 10
```

### Type

Type text into an element.

```bash
uip rpa uia interact type <e-ref> <text> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `e-ref` | `<e-ref>` (positional) | Element reference (e.g. `e42`). | **Yes** |
| `text` | `<text>` (positional) | Text to type. Supports special-key encoding `[d(key)]`/`[u(key)]`/`[k(key)]`. | **Yes** |
| `input-method` | `--input-method <HardwareEvents\|WindowMessages\|Simulate\|WebBrowserDebugger>` | Input method. [default: `HardwareEvents`] | No |
| `click-before-mode` | `--click-before-mode <Single\|Double>` | Click the element before typing. | No |
| `clear-before-mode` | `--clear-before-mode <SingleLine\|MultiLine>` | Clear the element before typing. | No |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
# Basic typing
uip rpa uia interact type e42 "hello world"

# Clear a single-line field first, then type
uip rpa uia interact type e42 "new value" --clear-before-mode SingleLine
```

### Hover

Hover over a UI element.

```bash
uip rpa uia interact hover <e-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `e-ref` | `<e-ref>` (positional) | Element reference. | **Yes** |
| `input-method` | `--input-method <HardwareEvents\|WindowMessages\|Simulate\|WebBrowserDebugger>` | Input method. [default: `HardwareEvents`] | No |
| `origin` | `--origin <Center\|TopLeft\|TopRight\|BottomLeft\|BottomRight>` | Reference point within the element for the hover offset. [default: `Center`] | No |
| `offset-x` | `--offset-x <integer>` | Horizontal pixel offset from the chosen origin. [default: `0`] | No |
| `offset-y` | `--offset-y <integer>` | Vertical pixel offset from the chosen origin. [default: `0`] | No |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
# Hover the element center
uip rpa uia interact hover e42

# Hover with a pixel offset from the default origin (center)
uip rpa uia interact hover e42 --offset-x 10 --offset-y -5

# Hover anchored to a specific origin
uip rpa uia interact hover e42 --origin TopLeft --offset-x 5 --offset-y -10
```

### Select

Select an item from a dropdown.

```bash
uip rpa uia interact select <e-ref> <value> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `e-ref` | `<e-ref>` (positional) | Dropdown element reference. | **Yes** |
| `value` | `<value>` (positional) | Item to select. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact select e73 "Second"
```

### Wheel

Scroll a UI element with the mouse wheel.

```bash
uip rpa uia interact wheel <e-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `e-ref` | `<e-ref>` (positional) | Element reference to scroll. | **Yes** |
| `direction` | `--direction <Down\|Up\|Left\|Right>` | Wheel direction. [default: `Down`] | No |
| `units` | `--units <integer>` | Number of wheel scroll units. [default: `1`] | No |
| `input-method` | `--input-method <HardwareEvents\|WindowMessages\|Simulate\|WebBrowserDebugger>` | Input method. [default: `HardwareEvents`] | No |
| `modifiers` | `--modifiers <Alt\|Ctrl\|Shift\|Win>` | Key modifiers held while scrolling. Combine with commas. | No |
| `origin` | `--origin <Center\|TopLeft\|TopRight\|BottomLeft\|BottomRight>` | Reference point within the element for the scroll offset. [default: `Center`] | No |
| `offset-x` | `--offset-x <integer>` | Horizontal pixel offset from the chosen origin. [default: `0`] | No |
| `offset-y` | `--offset-y <integer>` | Vertical pixel offset from the chosen origin. [default: `0`] | No |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
# Scroll down 10 units
uip rpa uia interact wheel e5 --direction Down --units 10

# Scroll with a pixel offset from the default origin (center)
uip rpa uia interact wheel e5 --direction Down --units 10 --offset-x 50 --offset-y 0

# Scroll anchored to a specific origin
uip rpa uia interact wheel e5 --direction Down --origin TopLeft --offset-x 30 --offset-y 30
```

### Focus

Bring a target into view and focus it.

```bash
uip rpa uia interact focus <any-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `any-ref` | `<any-ref>` (positional) | Any target reference (`w1`, `b3`, or `e5`). | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact focus e5
```

### Screenshot

Take a screenshot of the entire screen or a specific target.

```bash
uip rpa uia interact screenshot [any-ref] [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `any-ref` | `[any-ref]` (positional) | Target reference to screenshot. Omit for the full desktop. | No |
| `full-page` | `--full-page` | Capture the entire scrollable area (browser top-level targets only). | No |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
# Full desktop screenshot
uip rpa uia interact screenshot

# Screenshot a specific window
uip rpa uia interact screenshot w1

# Full-page browser screenshot
uip rpa uia interact screenshot b2 --full-page
```

### Highlight

Draw a colored border around the target for a fixed duration.

```bash
uip rpa uia interact highlight <any-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `any-ref` | `<any-ref>` (positional) | Target reference to highlight. | **Yes** |
| `duration` | `--duration <integer>` | Duration in seconds. [default: `3`] | No |
| `color` | `--color <Red\|Green\|Blue\|Yellow>` | Border color. [default: `Red`] | No |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact highlight e5 --color Blue --duration 5
```

### Get

Read an attribute value from a target.

```bash
uip rpa uia interact get <any-ref> <attribute> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `any-ref` | `<any-ref>` (positional) | Target reference. | **Yes** |
| `attribute` | `<attribute>` (positional) | Attribute name to read. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact get e5 text
```

### Get All

Read all available attribute values from a target.

```bash
uip rpa uia interact get-all <any-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `any-ref` | `<any-ref>` (positional) | Target reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact get-all e5
```

### Extract Table

Extract structured table data from an element.

```bash
uip rpa uia interact extract-table <e-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `e-ref` | `<e-ref>` (positional) | Table-like element reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact extract-table e10
```

### Window Close

Close a window or browser tab.

```bash
uip rpa uia interact window close <wb-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `wb-ref` | `<wb-ref>` (positional) | Window or browser tab reference (e.g. `w1` or `b3`). | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact window close b3
```

### Window Foreground

Bring a window to the foreground.

```bash
uip rpa uia interact window foreground <wb-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `wb-ref` | `<wb-ref>` (positional) | Window or browser tab reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact window foreground w2
```

### Window Maximize

Maximize a window.

```bash
uip rpa uia interact window maximize <wb-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `wb-ref` | `<wb-ref>` (positional) | Window or browser tab reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact window maximize w1
```

### Window Minimize

Minimize a window.

```bash
uip rpa uia interact window minimize <wb-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `wb-ref` | `<wb-ref>` (positional) | Window or browser tab reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact window minimize w1
```

### Window Restore

Restore a window.

```bash
uip rpa uia interact window restore <wb-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `wb-ref` | `<wb-ref>` (positional) | Window or browser tab reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact window restore w1
```

### Window Hide

Hide a window.

```bash
uip rpa uia interact window hide <wb-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `wb-ref` | `<wb-ref>` (positional) | Window or browser tab reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact window hide w1
```

### Window Show

Show a window.

```bash
uip rpa uia interact window show <wb-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `wb-ref` | `<wb-ref>` (positional) | Window or browser tab reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact window show w1
```

### Browser Open

Launch a new browser and navigate to a URL.

```bash
uip rpa uia interact browser open [url] [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `url` | `[url]` (positional) | URL to navigate to after opening. Omit to open the browser's default page. | No |
| `browser` | `--browser <Chrome\|Edge\|Firefox>` | Browser to launch. | No |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
# Open the default browser
uip rpa uia interact browser open

# Open Edge and navigate to a URL
uip rpa uia interact browser open "https://example.com" --browser Edge
```

### Browser Navigate

Navigate a browser tab to a URL.

```bash
uip rpa uia interact browser navigate <b-ref> <url> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `b-ref` | `<b-ref>` (positional) | Browser tab reference (e.g. `b3`). | **Yes** |
| `url` | `<url>` (positional) | URL to navigate to. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact browser navigate b1 "https://example.com"
```

### Browser Eval

Execute JavaScript in a browser tab or on an element.

```bash
uip rpa uia interact browser eval <any-ref> <script> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `any-ref` | `<any-ref>` (positional) | Target reference (`b3` for a tab, `e5` for an element). | **Yes** |
| `script` | `<script>` (positional) | `() => { ... }` for b-refs or `(element) => { ... }` for e-refs. | **Yes** |
| `world` | `--world <Main\|Isolated>` | Execution world for the script. [default: `Main`] | No |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
# Tab-level eval
uip rpa uia interact browser eval b1 "() => document.title"

# Element-level eval
uip rpa uia interact browser eval e5 "(el) => el.textContent"

# Run in an isolated world
uip rpa uia interact browser eval b1 "() => document.title" --world Isolated
```

### Browser Tab New

Open a new tab in the given browser.

```bash
uip rpa uia interact browser tab-new <b-ref> [url] [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `b-ref` | `<b-ref>` (positional) | Browser tab reference (used to identify the browser). | **Yes** |
| `url` | `[url]` (positional) | URL for the new tab. | No |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact browser tab-new b1 "https://example.com"
```

### Browser Tab Close

Close a browser tab.

```bash
uip rpa uia interact browser tab-close <b-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `b-ref` | `<b-ref>` (positional) | Browser tab reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact browser tab-close b1
```

### Browser Tab Select

Switch to a browser tab.

```bash
uip rpa uia interact browser tab-select <b-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `b-ref` | `<b-ref>` (positional) | Browser tab reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact browser tab-select b2
```

### Browser Go Back

Navigate back in browser history.

```bash
uip rpa uia interact browser go-back <b-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `b-ref` | `<b-ref>` (positional) | Browser tab reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact browser go-back b1
```

### Browser Go Forward

Navigate forward in browser history.

```bash
uip rpa uia interact browser go-forward <b-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `b-ref` | `<b-ref>` (positional) | Browser tab reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact browser go-forward b1
```

### Browser Reload

Reload the current page.

```bash
uip rpa uia interact browser reload <b-ref> [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `b-ref` | `<b-ref>` (positional) | Browser tab reference. | **Yes** |
| `visualize` | `--visualize` | Briefly highlight the target. | No |

**Example:**

```bash
uip rpa uia interact browser reload b1
```

---

## Indicate

Commands for manually indicating an application screen or element via a user click. Used when elements appear only after user interaction (e.g., a compose form that opens after clicking a button), so `uia-configure-target`'s automated capture cannot see them. Both commands require the user to physically click on the target.

> **CRITICAL:** User-driven and screen-blocking. Confirm readiness with the user before invoking; do not wrap in short timeouts (use minutes, not seconds, if any).

> **Note:** These are top-level `uip rpa` commands — they live OUTSIDE the `uip rpa uia` subtree. They are documented here because the Object Repository indication-fallback workflow uses them.

**Base commands:** `uip rpa indicate-application`, `uip rpa indicate-element`

Each command returns `{ "Data": { "reference": "..." } }` — use that reference ID for Object Repository lookups and target attachment. After indication, Studio regenerates Object Repository files; for coded workflows, re-read `ObjectRepository.cs` to get descriptor paths.

**Workflow:** indicate the screen first, then indicate elements within it using `--parent-id` from the screen's `Data.reference`.

### Indicate Application

Creates a screen entry in the Object Repository from a user click on the target window.

```bash
uip rpa indicate-application [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `name` | `--name <name>` | Screen name (e.g., `"LoginScreen"`). | No (recommended) |
| `parent-id` | `--parent-id <parent-id>` | AppVersion reference ID. Prefer over `--parent-name`. | No |
| `parent-name` | `--parent-name <parent-name>` | AppVersion name. Unreliable if names are non-unique. | No |
| `activity-class-name` | `--activity-class-name <class>` | Activity class (e.g., `"UiPath.UIAutomationNext.UI.App"`). | No |
| `description` | `--description <description>` | Description for the screen. | No |

When no App exists in `.objects/`, omit `--parent-id` and `--parent-name` — the command creates App + AppVersion automatically. When adding to an existing App, provide `--parent-id` with the **AppVersion** reference.

**Example:**

```bash
uip rpa indicate-application \
  --name "LoginScreen" \
  --description "Main login screen" \
  --project-dir "<PROJECT_DIR>" \
  --output json
```

**Troubleshooting:**

| Error | Cause | Recovery |
|-------|-------|----------|
| `"No application version found matching parentId=..."` | AppVersion reference is stale or App was never created. | Re-read `.objects/` metadata for a fresh reference. If no App exists, call `indicate-application` without `--parent-id` — it creates the App automatically. |
| `.objects/` has subdirectories but no `.metadata` files | Corrupted/incomplete App from a failed creation. | Clear orphan directories and run `indicate-application` without `--parent-id`. |

### Indicate Element

Creates an element entry under an existing screen in the Object Repository, from a user click on the target element.

```bash
uip rpa indicate-element [options]
```

| Option | Flags | Description | Required |
|--------|-------|-------------|----------|
| `name` | `--name <name>` | Element name (e.g., `"UsernameField"`). | **Yes** |
| `parent-id` | `--parent-id <parent-id>` | Screen reference ID (from `indicate-application` result or OR). | One of `--parent-id` / `--parent-name` |
| `parent-name` | `--parent-name <parent-name>` | Screen name. Alternative to `--parent-id`. | One of `--parent-id` / `--parent-name` |
| `activity-class-name` | `--activity-class-name <class>` | Interaction type: `TypeInto`, `Click`, `GetText`, etc. | **Yes** |
| `description` | `--description <description>` | Description for the element. | No |

**Example:**

```bash
uip rpa indicate-element \
  --name "UsernameField" \
  --activity-class-name "TypeInto" \
  --parent-id "<screen-reference>" \
  --project-dir "<PROJECT_DIR>" \
  --output json
```
