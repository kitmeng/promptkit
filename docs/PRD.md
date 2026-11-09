# Prompt.Kit — Product Requirements

**Build:** v0.8.0 · current local development build  
**Updated:** 9 November 2026  
**Release status:** Chrome Web Store publication paused  
**Product promise:** Your best prompts, always on hand.

## 1. Purpose and scope

Prompt.Kit is a personal library of reusable instructions, delivered as a Chromium browser extension. Save a useful way of working, then retrieve and insert it in the text field where you need it. The destination website or AI service runs the instructions; Prompt.Kit does not execute models.

The primary user repeatedly uses instructions for writing, visual work, research, strategy or thinking. Success means finding the right instruction quickly, inserting it predictably, and retaining control of the library and its storage.

This document describes current V8 behavior and its product contracts. It replaces the separate next-iteration plan and the mixed-version PRD. It is not a claim that every contract has passed live acceptance. Outstanding checks are in section 12. Historical documents are archived locally outside the active specification.

[README](../readme.md) introduces the product and installation workflow. [Design context](design.md) defines presentation rules without repeating feature requirements.

## 2. Product structure

| Area | User task | Capabilities |
|---|---|---|
| Retrieve | Reuse a saved instruction | `/q` picker, shortcut filtering, search, keyboard navigation and insertion |
| Manage | Maintain prompts | Create, View, Edit, Save, Cancel, Copy, Delete and Undo |
| Organize | Keep the library understandable | Category accordions, custom categories, ordering and moving prompts |
| Protect and transfer | Retain or move a library | JSON export/import, optional GitHub sync and recovery copies |
| Learn and configure | Understand the tool and its storage | First-use tour, Settings, connection and repository controls |

## 3. Surfaces and responsibilities

| Surface | Responsibility |
|---|---|
| Browser toolbar menu | Explain `/q` and saved shortcuts; open the manager |
| In-page picker | Find and insert prompts quickly; provide a Manage prompts action |
| Manager header | Brand and New prompt on the left; opened-prompt breadcrumb, sync status/action and Settings on the right |
| Manager library | Search, category accordions, prompt navigation, drag organization and Add category |
| Manager content | View or edit one prompt; show description, instructions and contextual actions |
| Settings | Explain browser/GitHub storage and sync state; contain sync controls, library files and tutorial replay |
| Quick tour | Teach the core workflow without creating content or connecting an account |

The manager can open over the current website or as an extension page. Sync settings remain inside the manager. Only external authorization, repository approval and GitHub links need to leave it. The overlay Close control stays separate from the manager header.

At narrow widths, Library and Editor become separate panes with explicit navigation. Feedback and confirmations must remain visible in the active pane.

## 4. First installation and onboarding

A fresh V8 installation starts with zero prompts, six category definitions (Visual, Writing, Thinking, Research, Strategy and Utility), and no GitHub sign-in or selected repository. A returning user retains created/imported prompts and existing connection state; reloading must not reset them.

The first-use tour covers creation, retrieval and keyboard use, categories, JSON files, optional GitHub sync, and website insertion. It supports Back, Next, Skip, Escape, persisted progress and replay from Settings. Starting real work pauses the tour. Tutorial actions never create sample prompts, authorize GitHub or reset user content.

An empty manager explains that no prompt is selected and offers New prompt. The sidebar retains the category structure and Add category without an additional global “No prompts yet” message. Empty-category and no-search-result feedback remain contextual.

## 5. Retrieval and insertion

### Picker

- Typing `/q` in a supported editable field opens the searchable library. Typing a saved shortcut filters matching prompts.
- Support eligible text inputs, textareas and contenteditable fields. Avoid password and other ineligible fields. Site-specific editor compatibility must be tested rather than assumed.
- Search matches shortcut, description and category. Ordered browsing and usage metadata assist retrieval.
- ↑/↓ changes the highlighted result; Enter inserts; Escape dismisses. Pointer selection inserts the chosen prompt. Visible keyboard hints explain these actions.
- Search, empty results and placement respond to the current field and viewport. The picker must follow an expanding field and remain usable near viewport edges.
- Manage prompts opens the manager, focusing its search and preventing subsequent typing from reaching the host page.

### Insertion contract

Insert at the intended trigger/selection without submitting the host form. Preserve unrelated text and support another `/q` after insertion when the trigger is separated appropriately.

Snapshot the insertion target before asynchronous loading. Accept one insertion operation at a time. Closing/reopening the picker or changing the host field cancels stale work. A load failure retains the user's text and allows retry. Repeated Enter events from the activation key must not spill into the host editor after insertion. Insertion never waits for animation.

Template-variable parsing/resolution remains in the underlying runtime for compatible saved prompts. The simplified manager does not expose a variable-authoring panel; completion behavior is an outstanding compatibility checkpoint, not a new advertised editor feature.

## 6. Prompt management

A prompt consists of a shortcut, optional short description and instruction body. The shortcut is its title, displayed with `/`; there is no separate saved display name or “Edit /name” heading.

| State | Behavior and actions |
|---|---|
| No selection | Empty content state and New prompt action |
| New | Editable draft; Create prompt persists a valid prompt; Cancel exits without creating a prompt |
| View | Selectable read-only instructions; Edit prompt, Copy and Delete available |
| Edit, unchanged | Existing fields editable; Save disabled until a real valid change |
| Edit, changed | Save changes and Cancel available; navigation/closing uses discard protection |
| Saving | Commit the captured draft; do not lose newer typing or allow overlapping saves |
| Save succeeded | Return to View if the current draft matches the committed snapshot; retain newer unsaved edits otherwise |
| Save failed | Keep edits and provide actionable feedback; do not claim success |

Cancel returns to View and discards the draft. Escape or changing context with dirty text uses the appropriate discard confirmation. Reverting text to its saved values clears the dirty state. Metadata autosaves do not implicitly save the instruction draft.

Shortcut validation removes leading slashes, rejects reserved `/q`, and allows letters, digits, `_`, `-`, `[`, `]` and `.` up to 80 characters. Shortcuts must be unique case-insensitively. Description is limited to 500 characters. Instructions are required and limited to 250,000 characters.

The instruction surface contains its own Prompt label, character count and borderless copy icon. Copy writes the displayed instructions, including unsaved edits, without saving. Confirm clipboard success only after the write succeeds; denied access shows manual-copy recovery. Empty instructions disable copying. Successful copying briefly changes the icon to a tick and announces feedback.

Delete requires confirmation. A short Undo opportunity restores the recently deleted prompt; it is not a recycle bin or multi-level history. Deletions participate in sync when connected.

## 7. Library navigation and categories

Search sits above the category accordions. All prompts is a system view; Uncategorized is the fallback for unassigned prompts. Category tags appear in All prompts/flat search, avoiding repetition inside a named category.

↑/↓ highlights prompts; Enter opens one. Highlighting alone does not change the opened content or breadcrumb. Keyboard navigation continues into the next nonempty category instead of trapping the user at a category boundary. The breadcrumb reflects opened content and subsequent category/name changes. Selecting its category returns to that category.

Add category follows the final accordion as a dashed row. Its inline form has a name field and confirm/cancel actions. Names must be unique case-insensitively, nonempty, at most 40 characters, and cannot use reserved All or Uncategorized names.

Each editable category has an options action beside its title, revealed on hover or keyboard focus and persistently available on touch. Rename edits inline. Delete requires confirmation and moves prompts to Uncategorized rather than deleting them. All prompts and Uncategorized cannot be renamed or deleted.

Drag grips reorder prompts or move them between categories. The destination category receives clear feedback and can expand during dragging. A completed move updates the category label and persists category/order automatically, preserving unsaved editor text. Keyboard browsing and prompt selection remain independent of drag operations.

## 8. JSON files and recovery

Settings exposes Import JSON and Export JSON independently of GitHub sign-in.

Export downloads saved prompts and categories as a portable library snapshot. It does not implicitly save unsaved editor text or export authentication credentials and browser state.

Import validates structure, IDs, shortcuts, categories and instruction sizes before asking to replace the library. Current limits include a 5,000,000-byte input-file cap, 2,000 prompts and 100 categories. Invalid/cancelled imports must preserve the existing library. The compact confirmation anchors to the visible Settings control. Users with unsaved editor text receive appropriate protection.

Import is a library replacement workflow, not an advertised additive merge. When sync is connected, saved additions and removals may propagate through the sync policy. Explain consequential replacement before confirmation. Where available, sync recovery copies provide an additional saved-library recovery path; they are distinct from editor Undo.

## 9. GitHub sync

### Connection and destination

Sync is optional and uses GitHub App device sign-in plus installation access to selected repositories. Request GitHub host access when needed. Never require a user to paste a client secret, private key or access token into the product.

Choose a dedicated repository. Private storage is recommended. The current V8 source accepts writable public repositories too, explicitly warns that anyone can read their prompts/history, and shows repository visibility. Unknown visibility is shown as unverified rather than assumed private.

The synced file is `prompt-kit/library.json` on the repository's resolved default branch. Sign-in alone does not restore prompts: choose the same library and confirm a merge. Changing destination requires a new preview and does not erase the previous repository.

### First sync and subsequent operations

First sync shows a preview of the merged library and requires Merge and sync. The preview expires after five minutes and is tied to the local revision, connection and remote version. Recheck these before writing; reject stale previews and conflicting remote writes.

After connection, saved local changes schedule a background check after roughly 30 seconds; a periodic alarm checks roughly every five minutes while the browser runs. Browser suspension, offline conditions and GitHub limits can delay this. Unsaved editor changes are not synced. Manual Sync now must report actual progress and result.

Use a merge baseline, deletion records and conflict copies to reconcile changes. Incoming destructive changes or conflicts may pause for review. Preserve recovery data before applying changes. Verify an uploaded payload before acknowledging success; interrupted or uncertain writes must remain recoverable and must not display false success. Disconnect cancels obsolete work and preserves local prompts and remote history.

### Status and Settings

| State | Meaning |
|---|---|
| Sync off | Prompts stored in this browser; no selected connection |
| Choose location | Signed in, destination not yet selected |
| Not synced yet | Connected but no acknowledged initial sync |
| Waiting to sync | Local saved changes awaiting synchronization |
| Syncing… | An operation is unresolved; repeated activation is disabled |
| Synced | This browser's saved library matched GitHub at the last successful sync |
| Sync paused / Sign in again | An error or authorization problem needs action |
| Upload unverified | Remote acceptance is uncertain; verification/recovery is required |

“Synced” does not mean every other device has already updated. Keep the destination visible with status. Settings leads with a browser ↔ GitHub storage explanation, last successful sync, visibility and optional instructions for another computer. Manage sync/technical details hold repository changes, connection management, file location, errors and recovery controls. Settings also retains separate Library files and Getting started groups.

### Privacy and security boundaries

GitHub Contents permission covers all files in each selected repository, not only the library file. Private repositories are not end-to-end encrypted. Git history can retain prior or deleted prompts. Public repositories expose library content. Credentials are local privileged state and must not be included in synced/exported library content. Browser-profile compromise can expose locally stored content and credentials. Users should keep passwords and secrets out of prompts.

Privileged operations belong in the extension worker. The genuine in-page manager receives a temporary worker-verified session; arbitrary website-created embeds must not gain privileged sync access. Cross-window messaging checks sender/origin and targets the extension origin explicitly. Reload/close/reconnect must invalidate stale sessions and callbacks. Revoking sign-in and uninstalling repository access are separate GitHub actions.

## 10. Presentation and interaction requirements

Preserve the established neutral surfaces, rounded controls, yellow selection and shared typography. New prompt actions match across header and empty state. The header remains distinct from the browsing/content areas. Settings has normal buttons and a visible cursor.

Motion should explain arrival, selection, drag destination, completion and progress without delaying actions. Keep keyboard highlight immediate; do not stagger results on each search keystroke. Magnetic action surfaces preserve stationary labels and click targets. Manager Settings contents, Delete prompt, prompt rows, accordion headings and navigation tabs are excluded from magnetic cursor hiding.

Respect Reduced Motion and coarse pointers. Maintain visible keyboard focus, named icon controls, announced operation feedback, readable truncation and viewport fitting. Do not infer accessibility certification or a complete dark theme from the current visual system. Detailed tokens and motion rules live in [design context](design.md).

## 11. Implementation and scope boundaries

The current runtime is in `3Builds/promptkit-v0.8.0/`: the manifest and worker define extension access/lifecycle; content scripts own detection/picker/insertion; manager modules own editing, storage access, sync UI, onboarding and motion; worker-side sync/model modules own remote reconciliation. Tests are in that build's `tests/` directory.

V7 remains the preserved rollback baseline. Older builds and local documentation archives are historical references, not competing active specifications.

Not included: built-in AI generation/chat, prompt testing or improvement agents, team roles/shared workspaces, marketplace, billing, a version-history browser, automatic conversation capture, persisted unsaved editor drafts, or a redesigned theme system. Legacy metadata such as activation and variables can remain in storage without requiring visible controls in the simplified manager.

## 12. Outstanding work and acceptance

This is a source-aligned specification of the current build, not a declaration that development is final. The consolidation inspected V8 source and existing test inventory; it did not rerun runtime tests or access a live library. Earlier recorded single-profile GitHub checks and isolated/mock suites are useful evidence, not proof of complete multi-device or security acceptance.

| Checkpoint | Acceptance required |
|---|---|
| Current-build regression in Dia | Reload V8 and refresh host pages; verify first-install/returning state, picker insertion, expanded fields, keyboard navigation, editing/cancel and category moves |
| Animation acceptance | Verify open/close, hover, drag/drop/reorder, copy/save/delete feedback, Settings/tour and async sync states together; test Reduced Motion and coarse-pointer fallbacks. Current motion hooks do not establish a completed end-to-end pass |
| Two-device convergence | Two independent profiles/computers using dummy libraries and a temporary repo; verify additions, edits, category changes, deletion/Undo, concurrent edits and repeat sync without duplicate growth or rollback |
| Interruption and recovery | Terminate/restart at remote-write/local-apply boundaries; test lost responses, offline retries and recovery copies; preserve newer local work |
| Credential and access lifecycle | Live expiry/refresh, server-side revocation, repository visibility/access changes, disconnect/reconnect during work and stale-session rejection |
| Import failure/concurrency | Malformed/oversized input, cancelled replacement, persistence failure and another open manager; confirm no silent content loss and truthful replacement feedback |
| Template/site compatibility | Verify compatible imported variable prompts and real supported editors, including input/textarea/contenteditable, before promoting unsupported behavior |
| Accessibility and responsive review | Full keyboard paths, focus return, feedback in narrow panes, contrast, screen-reader naming and overflow checks |
| Store release | Resume only when requested; recheck packaging, privacy disclosures, permissions, licensing notices, listing assets and release evidence against the actual build |

Resolve these within this PRD when scope changes. Do not create a parallel next-iteration document containing another product specification.
