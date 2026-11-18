# prompt.kit design context

Build: v0.10.1. Updated: 18 November 2026.

This document follows the current Search, Library, and Sync interface. It replaces the V8 toolbar-manager guidance. [Product requirements](PRD.md) owns behavior and acceptance. This file owns presentation.

## Identity and content

Use `prompt.kit` in public product copy. Keep repository names, GitHub App identifiers, library paths, and historical version identifiers unchanged.

Start with an empty library. Search and Library should explain the next action without filling the interface with invented prompts. Store images show the empty state. User-authored instructions are content, not decoration, and must never be copied from a live library into promotional assets.

## Typography

Current CSS defines six shared sizes. Values below are before the overlay's 80% scale.

| Token | Value | Purpose |
| --- | --- | --- |
| `--pk-type-meta` | `.75rem` | Metadata and compact hints |
| `--pk-type-body` | `.8125rem` | Controls and supporting copy |
| `--pk-type-reading` | `.9375rem` | Reading text |
| `--pk-type-section` | `1rem` | Section labels |
| `--pk-type-prompt` | `1.125rem` | Prompt titles |
| `--pk-type-heading` | `1.3125rem` | Headings |

Geist is the main UI family. Geist Mono supplies shortcut and technical text. Inter is also bundled and declared in the stylesheet. Keep bundled font and vendor license files with the runtime. Instrument Sans and Season are not the current V10.1 type system.

## Surfaces and colour

Primary, secondary, and tertiary neutral text are `#171717`, `#525252`, and `#626262`. The standalone canvas is `#ececec`; the main panel is `#f5f5f5`. Rounded neutral surfaces and restrained reactive rims establish hierarchy.

Routine primary actions use black. Secondary controls remain neutral. GitHub actions and the healthy Sync dot use selective blue, with `--pk-sync-blue` set to `#2563eb`. Keep category colours and semantic feedback separate from general navigation. Destructive final actions retain a clear danger treatment.

## Composition

Search, Library, and Sync share navigation and a Close control. The overlay is displayed at 80% scale. Keep hit areas and text readable after scaling rather than judging only raw CSS sizes.

Search has one query field, a result list, contextual empty-state guidance, and keyboard hints. An empty library offers a first-prompt action. Search results contain only the user's saved instructions.

Library keeps browsing and editing visible together where space permits. At narrow widths, preserve an active editor and provide explicit navigation back to prompts. Keep category controls within category context. Dropdowns fit their labels within viewport bounds, reveal the selected item on opening, and use the shared type, scrollbar, focus, and cursor styles.

Sync separates the local-device library from optional GitHub storage. Keep connection setup, preview, replacement, recovery, and disconnect flows concise. Dates and counts identify backups. Replacement warnings must state what will be lost and whether GitHub changes.

## Interaction and motion

Keep selected state, pointer hover, and keyboard focus distinct. Magnetic movement must preserve stable labels and hit targets. Reactive rims and rainbow feedback must not obscure text or become the only signal of state.

Use motion to explain a transition, not to delay insertion or acknowledge an operation that has not completed. Repeated search input should not replay entry animation. Reduced Motion and coarse-pointer behavior retain usable static controls and visible feedback.

Search must retain focus without passing typing to the host page. Closing should return the user to their writing context. Match Close contrast to its surrounding page and keep it visible on both light and dark backgrounds.

## Accessibility and verification

Use named controls, visible focus, readable contrast, keyboard operation, and clear error text. Status cannot depend only on colour or animation. Preserve content and controls under increased zoom, long labels, and narrow viewports.

Verify the actual extension, not just a static mockup. Empty-library store screenshots must be captured with an isolated profile and no GitHub credentials or user library. Do not infer accessibility certification or universal editor compatibility from screenshots.

## Local owning files

`3Builds/promptkit-v0.10.1/ui.css`, `sync-approved-ui.css`, and `preview-palette.css` define the current styling. `index.html`, `ui.js`, and the Sync UI modules define the current components. Older V8 design files and screenshots are historical references, not instructions to restore the old interface.
