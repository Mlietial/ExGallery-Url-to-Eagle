## English Documentation

### 1. Overview

`ExGallery Url to Eagle` is a userscript that extracts tags from ExHentai / E-Hentai gallery pages and syncs the current gallery to Eagle as a bookmark item.

The panel title shown in the UI is `Eagle Tags Picker`.

It supports:
- extracting tags from the page
- selecting which tags to sync
- editing tag values manually
- custom suffix rules
- selecting an Eagle folder
- syncing different namespaces into different Eagle Tag Groups
- dragging, minimizing, edge hiding, and collapsing the top controls

If an Eagle item with the same page URL already exists, the script updates that item instead of creating a duplicate.

### 2. Requirements

You need:
- a userscript manager such as Tampermonkey or Violentmonkey
- a local Eagle installation running
- Eagle local API available at `http://localhost:41595` by default

Permissions and connections used by the script:
- `GM_xmlhttpRequest`
- `GM_getValue`
- `GM_setValue`
- `localhost`
- `127.0.0.1`

### 3. Main Features

#### 3.1 Tag extraction and manual editing

The script reads tags from the gallery page and shows them in the panel list.

You can:
- select or unselect tags individually
- use `Select All` and `Clear All`
- edit each tag value directly in the list

Manually edited values are persisted and will be reused later for the same tag.

#### 3.2 Custom suffix

The panel includes a `Suffix` input field.

Purpose:
- append a custom suffix to namespaces other than `artist`, `character`, `parody`, and `group`

Example behavior:
- `male: glasses` may become `glasses-C`
- if the suffix is empty, no suffix is appended

Notes:
- the default suffix is empty
- the user is expected to set it manually
- changing the suffix refreshes current default tag values immediately
- manually overridden tag values are not overwritten

#### 3.3 Eagle folder selection

You can choose the target folder from the `Folder` dropdown and refresh the Eagle folder list with the `Folders` button.

During sync:
- if a folder is selected, the bookmark item is synced into that folder
- if no folder is selected, the item is synced into the Eagle root library

#### 3.4 Eagle Tag Group sync

The script can sync different tag categories into different Eagle Tag Groups.

Current group selectors:
- `Suffix`
- `Parody`
- `Character`
- `Group`
- `Artist`

Each selector can:
- choose an existing Eagle Tag Group
- refresh the Eagle group list via the `Groups` button

Sync rules:

1. `Suffix`
   - syncs tags ending with the current custom suffix
   - skipped when the suffix is empty

2. `Parody`
   - syncs tags from the `parody` namespace

3. `Character`
   - syncs tags from the `character` namespace
   - attempts to resolve character tags using the selected `parody` tags
   - prefers matching existing related character tags already present in Eagle

4. `Group`
   - syncs tags from the `group` namespace

5. `Artist`
   - syncs tags from the `artist` namespace

Backward compatibility:
- if an older version saved a `Creator` group selection, the current version migrates that value into both `Group` and `Artist`

### 4. Panel Behavior

The title bar buttons are used for:
- collapsing or expanding the top controls
- minimizing or restoring the panel
- enabling or disabling edge hide
The top controls section includes:
- Folder
- Suffix
- all Tag Group selectors
- Refresh / Select All / Clear All
When collapsed:
- the overall panel height stays the same
- the tag list expands to fill the released space
When minimized, the panel becomes a compact title strip.
The panel can hide against the left or right edge:
- by clicking the title bar edge-hide button
- or by dragging the panel to a screen edge

When edge-hidden:
- the panel slides into the side
- hovering the side handle reveals it
- moving away hides it again

On the right side, the script tries to avoid overlapping the browser scrollbar area.

The panel supports dragging and persists:
- panel position
- minimized state
- docked state
- top-controls collapsed state

### 5. Sync Logic

When you click `Sync Selected to Eagle`, the script:

1. collects the currently selected tags
2. builds the current page title and URL
3. checks whether an Eagle item with the same URL already exists
4. updates the item if it exists
5. creates a bookmark item if it does not exist
6. adds tags into configured Eagle Tag Groups

Notes:
- for a newly created bookmark, the current behavior may ask you to click `Sync` one more time to finalize the title
- for an existing item, tags and title are updated directly

### 6. Recommended Workflow

1. Open an ExHentai / E-Hentai gallery page
2. Wait for the panel to appear
3. Click `Refresh` to reload page tags
4. Select the target folder
5. Fill in `Suffix` if needed
6. Choose Eagle Tag Groups for `Suffix / Parody / Character / Group / Artist`
7. Select or edit the tags you want to sync
8. Click `Sync Selected to Eagle`

### 7. Troubleshooting

Check that:
- Eagle is running
- the local API is available
- the default port has not changed
