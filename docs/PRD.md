# prompt.kit product requirements

Build: v0.10.1. Updated: 18 November 2026.

The current store package is an unpublished draft. The existing published, unlisted version is 0.8.12. This document describes the current V10.1 source, not the older V8 toolbar manager. It is not a claim that every browser, editor, or live GitHub scenario has passed acceptance.

## Purpose

Save reusable instructions, organise them into categories, and insert them into supported website text fields. The destination website runs any AI instructions. prompt.kit does not run models or submit the user's message.

Product copy and the manifest use the exact name `prompt.kit`, without a version suffix. The short description is:

> Stop digging through old chats for that one prompt. Save it. Type a shortcut. Use it again.

## Product structure

| View | Responsibility |
| --- | --- |
| Search | Find saved prompts by query or shortcut and insert the selected prompt |
| Library | Create, edit, copy, delete, categorise, and order prompts |
| Sync | Local JSON backups, import and recovery, optional GitHub connection and sync |

The current build opens an extension iframe over the website. It does not provide the V8 toolbar popup, a separate manager window, or a first-use tour. Do not direct users to those older controls.

## First installation

A new installation starts with an empty prompt library and no GitHub connection. Search explains how to add a first prompt. Opening and closing the interface must not populate an empty library. Existing saved prompts must remain intact when the extension reloads or updates.

No example instructions ship as library content. Browser regression fixtures belong in the test harness, not in the packaged runtime. Store screenshots must show the empty-library state rather than imply that prompts are included.

## Retrieval and insertion

Typing `/q` in a supported text field opens Search. Saved shortcuts also retrieve matching prompts. Search supports keyboard navigation, Enter to insert, and Escape to dismiss. Pointer selection inserts the chosen prompt.

Supported paths include text inputs, textareas, and eligible contenteditable fields. Password and protected fields are excluded. Embedded-frame and editor compatibility must be tested explicitly.

Search must take focus. Its keystrokes must not reach the website's composer or submit its form. Insertion must preserve surrounding text, replace the intended command or selection, and leave sending to the user. Failure must not report success or silently destroy the user's text.

## Library

Prompts have a shortcut, description, instruction body, category, and ordering and usage metadata. The shortcut identifies the prompt. Search and Library use the same saved library.

Library supports creation, editing, copying, deletion, category management, and manual ordering. Preserve unsaved edits with the existing navigation and discard controls. Copying does not save an editor draft. Destructive actions require confirmation. Local changes must not claim to have changed GitHub.

Maintain visible selection and keyboard focus separately. Preserve the two-column Library layout where space permits. At narrow widths or increased zoom, keep the active editor visible and make returning to the list explicit.

## Files and recovery

**Sync → This device** contains **Add from file** and **Download backup**. These work without GitHub sign-in.

Export downloads a JSON copy of the saved local library, not authentication tokens. Import validates the file before changing the library. **Add prompts** keeps existing prompts and handles shortcut conflicts. **Replace prompts…** requires confirmation and explains the local replacement. Invalid input or cancellation must preserve the current library.

**Restore an earlier version** and **Undo last sync** are local recovery actions when backups exist. Show the saved date and prompt count when available. Explain that recovery replaces local prompts and categories and loses later local edits. Offer a download of the current library before restoring. Local recovery does not change GitHub.

## GitHub sync

GitHub is optional. Sign-in uses the GitHub App device flow. Ask for GitHub host access when needed. Never request a user-supplied private key, client secret, or pasted access token.

The user chooses a writable repository authorised for `promptkit-app`. The library path remains `prompt-kit/library.json`. A public repository exposes its prompts and accessible Git history. Private repositories use GitHub's access controls, not end-to-end encryption. Repository contents permission applies to the authorised repository, not just this one file.

V10.1 sync runs when the user starts it. Do not describe the V8 automatic-sync alarms as current behavior. Preview changes before writing. Empty repositories are supported after confirmation. Preserve a local recovery copy before replacing local data during sync. Report uncertain uploads and pending local application truthfully.

Deleting a prompt locally does not automatically delete the other copy. Disconnect clears this device's connection state but keeps the local library and remote history. Revoking GitHub App access is a separate action on GitHub.

## Privacy and implementation boundaries

The worker handles privileged storage and GitHub operations. Website-created embeds must not gain those privileges. The genuine overlay uses a worker-verified connection. Local library writes reject stale snapshots rather than overwrite newer state.

Prompts and recovery data use local extension storage. V10.1 GitHub credentials and connection state use session storage. Authentication data must not enter exported or synced library JSON. Local usage counts and last-used times are excluded from GitHub library payloads.

Do not upload surrounding website text or browsing history. Do not add advertising or third-party analytics. [Privacy policy](privacy.md) records user-facing data handling.

## Source ownership

Local source is `3Builds/promptkit-v0.10.1/`.

| File | Responsibility |
| --- | --- |
| `manifest.json` | Name, short description, version, and extension access |
| `chatgpt.js` | Host-field detection, overlay lifecycle, and insertion |
| `bridge.js` and `session.js` | Verified overlay connection and local storage operations |
| `ui.js` | Search, Library, and shared interaction state |
| `github-connection.js` | GitHub sign-in and session credentials |
| `github-storage-preview.js` and `github-merge-model.js` | Repository access, sync preview, reconciliation, and recovery |
| `sync-local-files-ui.js` and `sync-preview-files.js` | Local import, export, and restore |

`0Materials/store/package-release.py` packages this build locally. Runtime code, assets, and bundled vendor files are included. The build README and test fixtures are excluded. Store title and summary come from the packaged manifest; the long description is a separate dashboard field.

## Verification and remaining acceptance

The empty-library browser regression checks fresh installation, reopening, and preservation of an existing user's prompts. The Picker focus regression checks filtering and insertion without host-page keystroke leakage or form submission. These use disposable Chromium profiles, not the user's live library.

Retain separate acceptance for signed-in target editors, supported browser versions, independent GitHub accounts and devices, access revocation, interrupted uploads, offline changes, accessibility, increased zoom, and reduced motion. Mocked or isolated checks do not establish live multi-device acceptance.

Do not submit or publish a store release without explicit user approval.
