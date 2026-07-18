## Webctrl:

WebCtrl tags are used to automate UI elements in browser. Their attributes are generated dynamically based on attributes on HTML tags, so you have to be careful which add value and which are unreliable.

This is just a subset of predefined attributes that are known to be more or less reliable.

a) very reliable: tag, aria-label, aria-labeledby, aria-role, shadowhostid, aria-describedby
b) alright: id, parentid, visibleinnertext, innertext, name, parentname, isleaf, type, aaname
c) specific for tables: tableCol, tableRow, colName, rowName
d) last resort, only use if needed to make the selector unique: action, alt, href, src, tabindex, placeholder, uipath-html-title, css-selector, class, parentclass, aria-pressed, data-cy
e) last resort — framework-generated, sensitive to framework updates, only use if nothing from a-d uniquely identifies the target: ng-reflect-*, ng-version, _ngcontent-*, _nghost-*, data-reactid, __reactProps$*, v-*, data-v-*

## CRITICAL - IMMEDIATE DISQUALIFICATION FOR WEBCTRL:

### shadowhostid rule:
Any tag whose matched element lives inside a shadow DOM **must** include the `shadowhostid` attribute, even if its value is empty string.
The engine searches the entire HTML DOM including all shadow roots, so **intermediate ancestor tags are free to add or remove** — you do not need to keep every tag in the shadow DOM chain.
The only requirement is: if you keep or add a tag, and its node has `shadowhostid` in the attribute map, you must include `shadowhostid` on that tag with its exact value.

### frame/iframe rule:
Do not remove tags with tag 'frame' or 'iframe' since they are critical for identifying elements inside frames. They should keep pointing to the same ancestor nodes after your modifications.

## ADVICE FOR WEBCTRL SELECTORS:

### Dynamic Content Handling
Web attributes like `aaname`, `visibleinnertext`, `innertext` often contain dynamic data (dates, numbers, user names, amounts) that change between contexts. When you see these patterns, use wildcards to preserve the stable parts:

**Example of unreliable selector:**
`<webctrl tag='LABEL' aaname='$72.41 (1.12%)' id='201__25061_totalRaise_field' visibleinnertext='$72.41 (1.12%)' parentid='201__field_editor_layout' />`

Why this breaks: The dollar amount and percentage will change, making exact matches fail.

**Improved with wildcards:**
`<webctrl tag='LABEL' aria-labelledby='*totalRaise*' id='*totalRaise*' aaname='$*%*' />`

Why this works: Keeps semantic ""totalRaise"" pattern, uses wildcards for dynamic values, removes redundant duplicate text attributes.

### Text-Aggregating Attributes as Reliable Pillars

Unlike the previous section, which wildcards the target's *own* volatile text, this section is about using a *container's* aggregated text — which is often stable and uniquely identifies the row/card you want.

On `webctrl` tags, `visibleinnertext` and `innertext` contain the concatenated text of the element AND all its descendants. On container elements (rows, cards, list items, sections), this makes them reliable pillars for the selector: a single wildcarded value can pin the whole subtree to a known row/card identity.

**When to use:**
- Target sits inside a repeating structure (table row, card grid, list item) where the structural attributes alone don't differentiate it from siblings.
- A nearby text (row label, card title, ticker symbol) uniquely identifies the correct instance.
- Row/column index is fragile (sorting, pagination, virtualization).

**How to apply:**
- Put the text constraint on the CONTAINER (TR, DIV card, LI), not the target.
- Always wildcard: `visibleinnertext='*ETH*'` — never paste the full row text.
- Combine with a stable structural attribute on the same tag when possible (`parentid`, `data-testid`, `aria-role`) so the selector still works if the text changes.
- On tables, pair a row pinned by `visibleinnertext='*<rowKey>*'` with a cell identified by `colName='<column>'` — this is more robust than row index.

**Example — stock table, reading the ETH price cell:**
```
<html app='msedge.exe' title='Cryptocurrency Prices, Live Charts, Market Cap, News - Crypto.com EEA' />
<webctrl tag='TABLE' />
<webctrl parentid='cdc-market-body' tag='TR' visibleinnertext='*ETH*' />
<webctrl tag='TD' colName='Price' />
<webctrl tag='P' />
```
The target `P` has no stable identifier of its own; the ancestor `TR` with `visibleinnertext='*ETH*'` plus the `colName='Price'` cell make the selector unambiguous without filtering the target by its own dynamic price text.

### Framework Directives Are Last Resort

Attributes injected by UI frameworks should only be used as a last resort, even when their value looks semantic (e.g., `ng-reflect-name='labelEmail'`, `ng-reflect-dictionary-value='First Name'`).

Framework-generated attributes:
- Angular:  `ng-reflect-*`, `ng-version`, `_ngcontent-*`, `_nghost-*`, `ng-*`
- React:    `data-reactid`, `__reactProps$*`, `__reactFiber$*`
- Vue:      `v-*`, `data-v-*`

**Why last resort:** these attributes reflect internal framework state and @Input() property names, which are sensitive to every framework update — the app can look identical to the user while every one of these attributes has shifted.

**Preferred alternatives, in priority order:**
1. Ancestor/container text attribute: `visibleinnertext`, `innertext`, `aaname` on a wrapper (DIV, LABEL, custom component tag) — the visible label text survives framework upgrades.
2. ARIA attributes on or near the target: `aria-label`, `aria-labelledby`, `aria-describedby`.
3. Author-controlled `data-*` identifiers (e.g., `data-testid='first-name-input'`). `data-reactid` / `data-v-*` do NOT count — they're framework-generated.

Only fall back to framework directives when none of the above uniquely identifies the target, and never rank them above a semantic text alternative in the scoring.

### Auto-Generated Attribute Values
IDs and classes are often generated dynamically. Distinguish between those with semantic value vs purely random:

**Unreliable (fully auto-generated, no semantic meaning):**
- `id='89763184740'` → Avoid completely
- `id='container-45f6g7h8'` → Random suffix, unreliable
- `class='css-1wq41pf'` → CSS-in-JS generated class (Emotion/styled-components)
- `class='__className_6efda9'` → Next.js build hash class

**Partially reliable (contains semantic parts):**
- `id='129__25061_startDate_field'` → Use `id='*startDate*'` to extract semantic part
- `parentid='201__field_editor_layout'` → Use `parentid='*editor*'` or avoid

**Reliable (semantic, human-readable):**
- `id='search-form'` → Keep as-is
- `parentid='main-content'` → Keep as-is

### Hash-Based Tag Names
Some frameworks (e.g., ServiceNow) generate custom element tag names that embed a hex identifier, such as `MACROPONENT-F51912F4C700201072B211D4D8C26010` or `SCREEN-ACTION-TRANSFORMER-77B1DA1E6F22111089060168E25B36FD`. These are deployment-specific and may change across environments.
- **Prefer skipping these tags entirely** if a stable descendant with a semantic tag name (e.g., `SN-PAR-SCHEDULED-EXPORT`, `NOW-BUTTON`, `FILE-TO-EXPORT`) exists further down the chain.
- If you must keep one (e.g., it's the only shadow DOM entry point), wildcard the hash: `tag='MACROPONENT-*'`.

### Component Path IDs (id, data-testid, parentid)

`id`, `data-testid`, and `parentid` values with 3+ dot-separated segments often encode the component tree hierarchy rather than being stable identifiers. These break when any intermediate component is renamed — same fragility as deep `css-selector` paths, just hidden in one attribute. If a value has zero semantic content (purely numeric like `id='89763184740'`), avoid the attribute entirely.

**Detect:** Split on `.` or major boundaries. If most segments are structural (`ui`, `view`, `content`, `layout`, `components`, `container`, `wrapper`, `cell`, `body`, `header`, `lib`) rather than semantic (`auth`, `cart`, `submit-btn`, `search-field`), it's a component path.

**Fix:** Wildcard to the last 1-2 semantic segments:
- `data-testid='polaris-ideas.ui.view-content.idea-list.header.cell.trigger'` → `data-testid='*cell.trigger'`
- `id='polaris.ideas.ui.view-controls.filter-button'` → `id='*filter-button'`
- `parentid='app-layout-sidebar-navigation-menu-container'` → `parentid='*navigation-menu*'` or prefer `aria-role`
- `id='mat-form-field-0-input-container'` → `id='*form-field*input*'`

**Keep as-is:** Short semantic IDs (`data-testid='filter-button'`, `id='search-form'`) and intentional hierarchical IDs where most segments are semantic (`data-testid='auth.login-form.submit-btn'`).

**Hash suffixes:** `data-component-selector='header-button-eE39'` → `data-component-selector='header-button*'`

### CSS Classes — Semantic Core Extraction

Modern web frameworks generate class names that mix semantic developer-authored names with volatile build hashes, prefixes, and utility tokens. Instead of trying to detect which classes are hashes, extract the semantic core and wildcard the rest.

**Algorithm:**
1. Split the class value on `__` and `_` separators into segments
2. Score each segment: longer segments with hyphens (kebab-case like `search-input`) or recognizable multi-word patterns (camelCase like `cartTotal`) score high. Short mixed-case alphanumeric strings (3-8 chars like `kR4mZ`, `TnB8j`, `vYe3p`) are likely build hashes and score low
3. Select the highest-scoring segment as the semantic core
4. Emit `class='*semantic-core*'` with wildcards absorbing the volatile prefix/suffix/hash

**Examples:**
- `class='sc__components_search-input__kR4mZ'` → segments: [`sc`, `components`, `search-input`, `kR4mZ`] → best: `search-input` (kebab-case, 12 chars) → `class='*search-input*'`
- `class='ui__layout_sidebar-nav-item__xPq2W'` → best: `sidebar-nav-item` → `class='*sidebar-nav-item*'`
- `class='mod__CartSummary_cartTotal__TnB8j'` → best: `cartTotal` (camelCase) → `class='*cartTotal*'`
- `class='page__DataGrid_columnHeader__vYe3p'` → best: `columnHeader` → `class='*columnHeader*'`

**Reject entirely (no semantic content):**
- Emotion/styled-components: `css-[alphanumeric]` (e.g., `css-1wq41pf`)
- Next.js build hashes: `__[word]_[hex]` (e.g., `__className_6efda9`)
- Utility-only classes: layout tokens like `m-2`, `flex`, `w-100`, `hidden`

**Multiple classes in one attribute:** Keep only the semantic part from the best class:
- `class='k-dialog-wrapper dialog-xl dialog-voucher'` → `class='*dialog-voucher*'`
- `parentclass='btn btn-primary btn-lg'` → `parentclass='*btn-primary*'`

Apply the same extraction to `parentclass`.

### What to Avoid Completely
- Utility/layout-only CSS classes: `m-2`, `flex`, `w-100`
- DIV tags without semantic attributes(aria-*, role, meaningful id/class)

### What to Prefer
- Aria attributes are highly reliable: `aria-label`, `aria-labelledby`, `aria-role`, `aria-describedby`
- Semantic HTML tags: prefer specific tags(BUTTON, INPUT) over generic(DIV, SPAN)

### SAP Web Frameworks (Fiori, Web GUI, Ariba)

When you recognize SAP framework-specific attributes on `webctrl` tags, prefer elements that carry those attributes — they are more reliable than generic HTML attributes on that page.

#### SAP Fiori (UI5)
- **Very reliable:** `ui5-label`, `ui5-tooltip`, `ui5-role`, `ui5-type`, `ui5-view-local-id`
- **Alright:** `ui5-class`
- **For tables:** `ui5-tableCol`, `ui5-tableRow`, `ui5-isEmpty`, `ui5-colLabel`
- **Inside trees:** `ui5-path`

Some UI5 control types expose additional reliable attributes based on their `ui5-class` value. When you see one of these controls, prefer their control-specific attributes:
- Breadcrumbs: `currentLocationText`, `separatorStyle`
- DeltaMicroChart: `title1`, `title2`
- CustomListItem: `highlightText`
- FeedContent / NewsContent: `contentText`
- FeedInput: `buttonTooltip`
- FeedListItem: `sender`
- GenericTile: `header`, `imageDescription`
- GroupHeaderListItem: `count`
- Image: `alt`
- ListBase / Tree: `headerText`, `footerText`, `mode`
- NotificationListBase: `authorName`, `authorPicture`
- ObjectHeader: `intro`, `number`, `numberUnit`
- ObjectNumber: `number`, `numberUnit`, `unit`
- Panel: `headerText`, `accessibleRole`
- QuickViewGroup: `heading`
- RadioButton: `groupName`
- UploadCollectionItem: `documentId`, `url`

#### SAP Web GUI
- **Very reliable:** `sapweb-id` (GUI scripting ID), `sapweb-type` (GUI scripting type) — strongly prefer `sapweb-id` for most elements
- **Avoid:** `sapweb-lsid` — LightSpeed control ID, unreliable
- **Alright:** `sapweb-lsclass` (LightSpeed control class), `sapweb-text` (text from controls like TextView), `sapweb-itemid` (item ID inside trees)
- **Session context:** `sapweb-ses-screen`, `sapweb-ses-transaction`, `sapweb-ses-client`, `sapweb-ses-user`, `sapweb-ses-program`
- **For tables:** `sapweb-tablerow`, `sapweb-tablecol`, `sapweb-coltooltip`, `sapweb-colname` — for table cells, prefer these over `sapweb-id`
- **Inside trees:** `sapweb-path`

#### SAP Ariba
- **Very reliable:** `aw-name`, `aw-label`, `aw-type`
- **Avoid:** `id` — in Ariba pages the `id` attribute is an unreliable hash that changes across sessions
- **For tables:** `aw-tablerow`, `aw-tabledetailrow`, `aw-tablerowtype`, `aw-tablecol`, `aw-collabel`