---
name: uia-interact
description: "Inspect and interact with live desktop/browser apps -- click buttons, type text, read values, take screenshots, inspect UI state, verify behavior, fill forms, navigate menus, and extract table data from running applications"
allowed-tools: Bash(uip:*), Bash, Read
---

# UI Interaction via "uip rpa uia"

## CLI

```bash
CLI="uip rpa uia"
```

If `$PROJECT_DIR` is set, append it: `CLI="uip rpa uia --project-dir \"$PROJECT_DIR\""`. All subsequent `"$CLI" ...` commands will automatically include it.

**Commands that produce output print the output-file path to stdout — read the printed path.**

**If a prior step already captured a tree (e.g., `uia-configure-target`), grep it for the ref and call `uia interact` directly -- do NOT re-run `snapshot inspect`.** Only re-snapshot when the UI has actually changed since capture.

## Quick Start

```bash
# If a prior step already captured a tree, reuse those refs:
grep -n "Submit" tree.md                 # Find [ref=eN] for the target
"$CLI" interact click e42                # Use it directly

# Otherwise, start from a fresh inspection:
"$CLI" snapshot inspect                  # List top-level windows and browser tabs
"$CLI" snapshot inspect w1               # Inspect a window to get its UI tree
"$CLI" interact click e5                 # Interact using element refs from the snapshot
"$CLI" interact type e3 "hello"
"$CLI" interact select e8 "Option B"
"$CLI" snapshot inspect w1               # Re-inspect after UI changes -- refs may be stale
"$CLI" interact screenshot w1            # Take a screenshot
```

Example stdout from a command that produces a file:

```
### Top-level targets (11 windows, 2 browser tabs)
- [Targets](.local/.uia/.interact/output/snapshot-2026-04-24T14-32-15-001.yml)
```

Read the linked file to see the full results.

## Commands

All commands accept `--help`. `--visualize` is available on every verb — use it for visual confirmation; omitted from examples below for brevity.

### Discover

```bash
"$CLI" snapshot inspect                     # List windows (w-refs) and browser tabs (b-refs)
"$CLI" snapshot inspect --no-filter         # Show ALL top-level targets (disables automatic filtering)
"$CLI" snapshot inspect w1                  # Capture UI tree with element refs (e-refs)
"$CLI" snapshot inspect b1                  # Inspect a browser tab
"$CLI" snapshot inspect w1 --framework UIA  # Use specific UI framework
```

### Core

```bash
"$CLI" interact click e5                                         # Click (left, single)
"$CLI" interact click e5 --button Right                          # Right-click
"$CLI" interact click e5 --type Double                           # Double-click
"$CLI" interact click e5 --input-method Simulate                 # Click via element API (background windows)
"$CLI" interact click e5 --offset-x 10 --offset-y -5             # Click with pixel offset from center (default origin)
"$CLI" interact click e5 --origin TopLeft --offset-x 5 --offset-y -10 # Origin: Center, TopLeft, TopRight, BottomLeft, BottomRight
"$CLI" interact click e5 --modifiers Ctrl                        # Ctrl+click (e.g. multi-select in lists)
"$CLI" interact click e5 --modifiers "Ctrl,Shift"                # Ctrl+Shift+click
"$CLI" interact hover e4                                         # Hover over element (center)
"$CLI" interact hover e4 --offset-x 10 --offset-y -5             # Hover with pixel offset from center (default origin)
"$CLI" interact hover e4 --origin TopLeft --offset-x 5 --offset-y -10 # Origin: Center, TopLeft, TopRight, BottomLeft, BottomRight
"$CLI" interact type e3 "some text"                              # Type into element
"$CLI" interact type e3 "text" --clear-before-mode SingleLine    # Clear field, then type (implies click-before)
"$CLI" interact type e3 "text" --click-before-mode Single        # Click field before typing (auto-enabled for HardwareEvents)
"$CLI" interact type e3 "text" --input-method Simulate           # Use Simulate (may auto-clear)
"$CLI" interact type e3 "text" --input-method WebBrowserDebugger # Use browser debugger protocol (recommended for Chrome/Edge)
"$CLI" interact type e3 "[d(ctrl)]a[u(ctrl)]"                    # Select all (Ctrl+A)
"$CLI" interact select e8 "Option B"                             # Select "Option B" from dropdown/list
"$CLI" interact wheel e5 --direction Down --units 10             # Scroll down 10 clicks
"$CLI" interact wheel e5 --direction Down --units 10 --offset-x 50 --offset-y 0 # Scroll with offset from center
"$CLI" interact wheel e5 --direction Down --origin TopLeft --offset-x 30 --offset-y 30 # Scroll anchored to origin
"$CLI" interact focus e5                                         # Bring element into view and focus
```

### Window

```bash
"$CLI" interact window foreground w2    # Bring window to front
"$CLI" interact window close b1         # Close browser tab
"$CLI" interact window maximize w1      # Actions: close, foreground, maximize, minimize, restore, hide, show
```

### Browser

```bash
"$CLI" interact browser open                                    # Open default browser
"$CLI" interact browser open "https://example.com" --browser Edge # Open in specific browser (Chrome, Edge, Firefox)
"$CLI" interact browser navigate b1 "https://example.com"       # Navigate tab to URL
"$CLI" interact browser eval b1 "() => document.title"          # Execute JavaScript in tab
"$CLI" interact browser eval e5 "(el) => el.textContent"        # Execute JavaScript on element
"$CLI" interact browser eval b1 "() => document.title" --world Isolated # Run in isolated execution world
"$CLI" interact browser tab-new b1 "https://example.com"        # Open new tab with URL
"$CLI" interact browser tab-close b1                            # Close tab
"$CLI" interact browser tab-select b2                           # Switch to tab
"$CLI" interact browser go-back b1                              # Navigate back
"$CLI" interact browser go-forward b1                           # Navigate forward
"$CLI" interact browser reload b1                               # Reload page
```

### Inspect

```bash
"$CLI" interact get e5 text                # Read a single attribute
"$CLI" interact get-all e5                 # Read all attributes
"$CLI" interact screenshot                 # Full desktop screenshot
"$CLI" interact screenshot w1              # Window screenshot
"$CLI" interact screenshot b2 --full-page  # Full browser tab screenshot
"$CLI" interact screenshot e5              # Element screenshot
"$CLI" interact extract-table e5           # Extract table data as markdown
"$CLI" interact highlight e5               # Draw red border
"$CLI" interact highlight e5 --color Blue --duration 5
```

## Node Format

`"$CLI" snapshot inspect` outputs the same node format whether you pass a top-level ref, a window ref, or a browser tab ref:

- **Role** -- Node type: `Window`, `BrowserTab`, `Button`, `InputBox`, `CheckBox`, `DropDown`, `List`, `TreeItem`, `TabPage`, `MenuItem`, etc.
- **"Name"** -- Accessible label in quotes (e.g., `Button "OK"`)
- **[ref=...]** -- Reference for interaction: `w1` (window), `b1` (browser tab), `e5` (element)
- **[state]** -- State markers: `[selected]`, `[focused]`, `[disabled]`, `[read only]`, `[minimized]`, `[items deferred]`
- **: text** -- Inline value (e.g., `InputBox [ref=e3]: pre-filled`)
- **/attr** -- Attributes as child lines (e.g., `/url: https://...`, `/placeholder: Type here`)
- **Children** -- Nested with indentation

Top-level example (`snapshot inspect` with no ref):

```
- Window "Hello World" [minimized] [ref=w1]:
  - /process: notepad.exe
  - /class: Notepad
- Window "Google Chrome" [ref=w2]:
  - /process: chrome.exe
  - BrowserTab "Example" [ref=b1] [selected]:
    - /url: https://example.com/
  - BrowserTab "HTML5 Test Page" [ref=b2] [file URL]:
    - /url: file:///C:/Pages/index.html
```

Per-window example (`snapshot inspect w1`):

```
- DropDown [ref=e73]: Second
  - ListItem "-- Choose --" [ref=e405]
  - ListItem "First" [ref=e406]
  - ListItem "Second" [ref=e407] [selected]
  - ListItem "Third (disabled)" [ref=e408]
- DropDown "Region" [ref=e74] [items deferred]: East
- InputBox "Username" [ref=e5]: john_doe
  - /placeholder: Enter username
- CheckBox "Remember me" [ref=e6] [selected]
- Button "Submit" [ref=e7]
- Button "Cancel" [ref=e8] [disabled]
```

**Key rules:**

- Only nodes with `[ref=...]` are interactable. Nodes without refs are context only.
- `[disabled]` elements cannot be interacted with -- skip them.
- `[selected]` on CheckBox/RadioButton means checked; on TabPage/ListItem means active.
- `[items deferred]` on a DropDown/List means the items aren't in the snapshot — they may still be retrievable via `"$CLI" interact get-all <ref>`. Relevant attributes: `items`, `selectedItem`, `selecteditems`.
- Output may not show all values. Use `"$CLI" interact get <ref> text` or `"$CLI" interact get-all <ref>` to read values, or `"$CLI" interact extract-table <ref>` for table data.

## Ref Lifecycle

Any w/b/e-ref from the most recently loaded snapshot is valid for `uia interact`, no matter which `uia snapshot` subcommand loaded it. Each new snapshot replaces all prior refs.

**Window refs (w1, w2) and browser refs (b1, b2)** are assigned by top-level `snapshot inspect`. They reset on each top-level `snapshot inspect` call.

**Element refs (e1, e2, e3...)** are assigned by `snapshot inspect <wb-ref>`. They reset on each per-window `snapshot inspect` call.

b-refs target browser tabs; w-refs target windows. See Application Guides > Browsers for details.

**Always re-snapshot after actions that change UI state.** Clicking a button, selecting a tab, or typing may alter the UI tree, making previous e-refs invalid.

```bash
"$CLI" snapshot inspect w1        # Get refs
"$CLI" interact click e7          # Perform action
"$CLI" snapshot inspect w1        # Get fresh refs -- old ones are stale
"$CLI" interact type e5 "hello"   # Use new refs
```

## Frameworks

Use `--framework` with `snapshot inspect` to control how the UI tree is scanned. Set: `Default`, `UIA`, `AA`, `Java`.

Default framework works for most apps. Exceptions — use `--framework UIA` for:
- WinUI3 apps — modern Windows apps like Windows Terminal, and the redesigned Notepad, Paint, Calculator, Media Player
- WPF apps — .NET desktop apps with rich UI like Visual Studio, Blend, or any app built with XAML
- SAP Logon (only the connection picker)

If a snapshot looks empty or incomplete, try a different framework.

## Input Methods

Use `--input-method` with `click`, `type`, and `hover`:

- **HardwareEvents** (default) -- Simulates real mouse/keyboard. Auto-activates the window (foreground required). Typing appends to existing text, use `--clear-before-mode` to clear first.
- **Simulate** -- Uses the element's native API directly. Works on background windows. Usually auto-clears the field before typing, so do not use `--clear-before-mode` by default. Verify the result with `interact get`, `interact get-all`, or re-inspect -- only retry with `--clear-before-mode` if the field contains unexpected text. Recommended for Firefox (only in b-ref mode), Java Swing/AWT apps, and SAP WinGUI session windows.
- **WebBrowserDebugger** -- Dispatches via Chromium Debugger. Recommended for Chrome/Edge. Does not require foreground.

Switch input methods when the default has no visible effect on the target element.

Special keys in `interact type` and `--modifiers` in `interact click` are fully supported with HardwareEvents and WebBrowserDebugger. Simulate may support special keys for some applications (e.g.: Browsers (b-refs) and SAP session windows), but this is not guaranteed -- other input methods may silently ignore them.

## Special Keys

`"$CLI" interact type` supports special key syntax: `[k(key)]` to press and release, `[d(key)]` to hold down, `[u(key)]` to release.

See [references/special-keys.md](references/special-keys.md) for the full key list and examples.

## Common Patterns

### Select from dropdown/list

DropDown and List elements might show options as children. Use the **parent's ref** and the option's text:

```bash
"$CLI" interact select e73 "First"    # Select "First" using the DropDown's ref
```

The current selection is shown as inline text after `:` or as a child marked `[selected]`. Re-inspect to confirm.

If the parent element is tagged `[items deferred]`, the items aren't in the snapshot but may still be retrievable:

```bash
"$CLI" interact get-all e74           # `items`, `selectedItem`, `selecteditems`
"$CLI" interact select e74 "North"    # assuming `items` includes "North"
```

If items are missing, click the element to expand it and retry -- some controls load children only when opened.

### Toggle a checkbox

```bash
"$CLI" interact click e6                # Click to toggle; re-inspect to verify
"$CLI" snapshot inspect w1              # [selected] = checked, no [selected] = unchecked
```

### Navigate a menu

```bash
"$CLI" interact click e13               # Click "File" menu item
"$CLI" snapshot inspect w1              # Inspect to see submenu items
"$CLI" interact click e42               # Click the desired submenu item
```

### Expand a tree node

```bash
"$CLI" interact click e108 --type Double   # Double-click to expand tree item
"$CLI" snapshot inspect w1                 # Inspect to see children
```

### Switch non-browser tabs

```bash
"$CLI" interact click e115              # Click the tab you want
"$CLI" snapshot inspect w1              # Verify tab is now [selected] and content changed
```

### Fill a form

```bash
"$CLI" snapshot inspect                                       # Find the window
"$CLI" snapshot inspect w1
"$CLI" interact type e5 "John Doe" --clear-before-mode SingleLine
"$CLI" interact type e6 "john@example.com" --clear-before-mode SingleLine
"$CLI" interact select e8 "USA"
"$CLI" interact click e10               # Submit button
"$CLI" snapshot inspect w1              # Verify result
```

### Extract table data

```bash
"$CLI" interact extract-table e5    # Recognizes tables, data grids, and other tabular structures
                                    # Prefer over other scraping methods; fall back only if it fails
```

## Error Recovery

**Possible misconfiguration:** If you suspect an app is not properly configured for automation (e.g., missing extension, scripting disabled), tell the user and point them to the relevant Application Guide.

**Empty/partial snapshot** -- Wrong framework or window not ready:

```bash
"$CLI" interact window foreground w1         # Ensure window is visible
"$CLI" interact window maximize w1           # Maximize to see all elements
"$CLI" snapshot inspect w1 --framework UIA   # Try different framework
```

**Dropdown/list options not visible**

If the control is tagged `[items deferred]`, try reading the items directly:

```bash
"$CLI" interact get-all e10                  # see `items`, `selectedItem`, `selecteditems`
```

Fallback -- click to expand, then retry:

```bash
"$CLI" interact click e10                    # Click the DropDown to expand it
"$CLI" interact get-all e10                  # Retry reading the `items`

# Or, if still no items, try to snapshot again after the click:
"$CLI" snapshot inspect w1                   # Re-inspect to see the options
```

**Interaction has no visible effect:**

```bash
"$CLI" interact get-all e5                        # Check element attributes for clues
"$CLI" interact click e5 --input-method Simulate  # Try a different input method
```

**Click lands on wrong spot** -- The click feedback shows screen coordinates. Take a screenshot and check whether those coordinates are visually inside the intended element. If not:

```bash
"$CLI" interact highlight e5                                     # Highlight the element to see its bounds
"$CLI" interact screenshot e5                                    # Screenshot the element
"$CLI" interact get e5 position                                  # Get the reported position of the element

# Try
"$CLI" interact click e5 --origin TopLeft --offset-x 5 --offset-y 5 # Use a different origin point instead of center
"$CLI" interact click e5 --input-method Simulate                 # Might ignore the coordinates
"$CLI" snapshot inspect w1 --framework UIA                       # Re-inspect with different framework (bounds may differ)
"$CLI" interact click e10                                        # Click a child element that may be more reliably located
```

## Application Guides

### Browsers

**Prerequisites:** UiPath browser extensions must be installed for b-refs to appear in `snapshot inspect`. Install: https://docs.uipath.com/studio/standalone/latest/user-guide/about-extensions

**Targeting:** Use b-refs (not w-refs) for web content -- b-refs provide the DOM tree. Browser tabs appear nested under their parent window in `snapshot inspect` (see Node Format above).

```bash
"$CLI" snapshot inspect b1   # Preferred: inspect the browser tab
```

The active tab is marked `[selected]`. Some tabs have states that signal access limitations:

- `[discarded]` -- Suspended by the browser to save memory. The CLI attempts to restore it automatically on first interaction.
- `[internal page]` -- Browser internal pages (new tab, settings, etc.). Cannot inspect or interact with page elements. Browser tab commands (navigate, reload, close, etc.) may still work. Use the parent w-ref for element-level interaction as a desktop application.
- `[extension store]` -- Web store pages. Same limitations as internal pages.
- `[file URL]` -- Local file URLs. Same limitations unless "Allow access to file URLs" is enabled for the UiPath extension.

**Browser windows without tabs:** When a browser window has no BrowserTab children, `snapshot inspect` adds a state to the parent window to explain why:

- `[extension missing]` -- The UiPath browser extension is not available. It may not be installed or may be disabled.
- `[incognito]` -- The extension is installed but cannot access this window (likely incognito/private). Allow the extension to run in incognito mode in the browser's extension settings.

In both cases, you can still use the w-ref to control the browser as a desktop application.

After page navigation, re-inspect to get fresh refs.

**Input methods:**
- Chromium (Chrome, Edge): Use `--input-method WebBrowserDebugger`. Fallback: Simulate, then HardwareEvents.
- Firefox: Use `--input-method Simulate` -- WebBrowserDebugger is not supported. Fallback: HardwareEvents.

### SAP WinGUI

**Prerequisites:** SAP GUI Scripting should be enabled (server and client). Setup guide: https://docs.uipath.com/activities/other/latest/ui-automation/sap-wingui-configuration-steps

**Framework selection:**
- SAP Logon (connection picker) -> `--framework UIA`
- All other SAP GUI windows (after connecting) -> `--framework Default` (or omit)

```bash
"$CLI" snapshot inspect w1 --framework UIA    # SAP Logon window
"$CLI" snapshot inspect w1                    # SAP session window (Default)
```

**Transaction code navigation:**

```bash
"$CLI" snapshot inspect w1                                           # Get ref for the command field
"$CLI" interact type e1 --input-method Simulate "/nVA01[k(enter)]"   # Navigate to transaction VA01
"$CLI" snapshot inspect w1                                           # New transaction screen loaded with new refs
```

**Reading table data:** Snapshots only show rows currently in view. Maximize first for more rows in snapshot:

```bash
"$CLI" interact extract-table e15    # Extracts entire table, not just visible rows
```

To interact with specific rows not in view, scroll and re-inspect:

```bash
"$CLI" interact wheel e15 --direction Down --units 5    # Scroll down
"$CLI" snapshot inspect w1                              # Fresh refs for newly visible rows
```

**Status bar:** SAP confirms operations via the status bar. After an action:

```bash
"$CLI" interact get e99 text        # Read status bar
```

**Tips:**
- Use longer timeouts for SAP operations
- Check status bar messages to confirm operations
- SAP tables only expose visible rows in snapshots -- use `extract-table` for full data
