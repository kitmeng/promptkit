# Prompt.Kit — Design implementation context

## Register

product

## Purpose and sources

Design guidance for the current v0.8.0 build, updated 9 November 2026. [PRD](PRD.md) owns features, behavioral contracts and outstanding acceptance. This file owns appearance and interaction styling, not a second requirements backlog.

Local design source: `1Design/design.pen`. Local references: `1Design/references/forpeople-style-guide-ref.md` and `1Design/atomic-design-workflow.md`. Design assets and these reference files are not published in the documentation repository. Preserve the approved base design; refine consistency and feedback rather than introducing a new aesthetic.

## Foundations

| Token or rule | Current direction |
|---|---|
| UI and display type | Locally bundled Instrument Sans, weights 400–700 |
| Shortcuts/technical text | Geist Mono |
| Canvas | `#F6F6F6` |
| Module surface | `#EEEEEE` |
| Ink | `#211A16` |
| Muted text | `#6F6862` |
| Lines | `#DDD6CF` |
| Yellow accent/selection | `#FFC400`; retain full color rather than lightening the panel fill |
| Error | `#C94B40` with explanatory text |
| Primary/secondary controls | 40px and 32px standard action heights; fields and category rows have their own geometry |

Instrument Sans supersedes earlier Season typography. Both runtime font families retain their bundled license notices. Reuse current runtime colors for GitHub's blue action/healthy-state treatment; do not introduce unrelated new colors.

## Composition

Manager: two-pane shell with a shared light-grey header, at least 72px tall on desktop. Left header contains brand and New prompt; right header contains breadcrumb, sync status/action and Settings. Keep the external Close control clear of header actions. Wrap at narrow widths without overlap.

Library: a sticky 50px search field close to, but distinct from, the accordions. Keep category titles/counts/arrows aligned. Expanded headers are white; selected prompt rows are yellow. Category tags belong in All prompts/flat search. Add category follows the final accordion as a dashed unfilled row. The category options icon sits beside its title and appears on hover/focus, remaining discoverable on touch.

Content: shortcut title, optional short description, then the instructions surface. Its internal header groups Prompt, character count and a borderless copy icon at top right. Keep metadata inside the surface without crowding the text. Empty/new/editing states use the same layout vocabulary.

Picker: brand at top-left above search, then the result list and a compact keyboard-guidance footer with Manage prompts. Use a 24px mark/16px wordmark; manager branding remains larger (32px mark/22px wordmark on desktop). Match row typography, category badge proportions, warm surfaces and description spacing across both surfaces.

Settings: a compact gear-anchored popover with a pointer aligned to the gear, a 20px inset and a subtle storage overview with 14px inner padding. Keep browser/GitHub endpoints unboxed within that overview. Use a blue outline and label for the quiet Sync now action. Header and diagram share a labelled health symbol; color supplements its shape and text. Lead with the storage/sync explanation; put less frequent sync controls inside progressive disclosure. Library files and Getting started remain separate groups. Keep the pointer aligned with the gear, dismiss accessibly and fit/scroll within the viewport.

## Components and states

Both New prompt actions use the same black pill, white text, plus icon, 40px height and 14px text. Keep Edit/Create/Save actions within the existing primary-action system. Delete retains the normal danger treatment. GitHub actions retain their blue recognition and clear text states.

Use the same typography before/after accordion expansion. Input focus outlines follow the field's rounded geometry and must not be clipped. Category-entry labels, fields and confirm/cancel icons share consistent insets and vertical alignment.

Distinguish selected prompt, pointer hover and keyboard focus. Do not make category selection yellow when nested prompt selection is also yellow. Use named icon actions, visible focus and concise contextual feedback. Status cannot depend solely on color or motion.

## Motion and pointer behavior

Picker entry is restrained (140ms), dismissal quicker (100ms). Placement updates do not replay entry. Pointer-color feedback can transition briefly; keyboard highlight stays immediate. Search does not animate every result per keystroke, and insertion never waits for motion.

Manager motion uses short fades/translations and position reconciliation to explain reveals and list movement. Dragging should expose the destination, settling into the new position; acknowledgments follow actual copy/save/sync success. Sync motion runs while work is unresolved, then communicates success, review or error truthfully. Do not animate idle status as a new successful operation.

The PHI Lab-inspired magnetic surface uses a small approach zone and bounded surface movement while labels and hit targets stay fixed. Filled actions retain their fill; quiet actions form a neutral surface. Hide the native cursor only over enabled magnetic controls on a fine pointer, restoring it on exit.

Exceptions: Settings contents and Delete prompt retain normal static buttons and visible native cursors. Prompt rows, accordion headings and navigation tabs are outside magnetic behavior. The header gear, copy icon and appropriate action buttons retain the effect.

Reduced Motion removes spatial movement and unnecessary animation while preserving state feedback and functionality. Coarse pointers retain usable static targets. All motion remains interruptible and must not steal focus, replay indefinitely or cause layout jumps. The PRD tracks incomplete motion acceptance; these rules do not claim every transition is implemented or verified.

## Responsive and accessibility review

Preserve readable text, long-shortcut truncation, consistent alignment and viewport fitting. Library/Editor pane changes retain navigation context. Errors, confirmations and action feedback must appear in the active pane. Keep focus visible and return it to the appropriate trigger after popover dismissal.

Use existing components/tokens before adding variants. Do not claim a complete dark theme, native Apple rendering or accessibility certification from a design reference. Validate the actual browser output at desktop and narrow widths.
