# Create a note

Create note lets a user save a titled note from the browser or CLI, cancel an unfinished draft, and confirm the saved note from a second user-facing view.

## Sub-features

- `create-open` opens a blank editor from each browser entry point.
- `create-save` persists a title and body.
- `create-cancel` discards an unfinished browser draft.
- `create-cli` creates the same note shape from the terminal.

## How to get to it (user POV)

- Choose the `New note` button in the browser toolbar.
- Press `n` in the browser while focus is outside an editable field.
- Run `notes create --title <title> --body <body>` in a terminal.

## Driving it with control-notes

Preconditions:

- App is healthy at the verify URL.
- No note is titled `Release checklist`.
- Doctor reports expected URL and disposable data directory.

- **Open editor.** Choose `New note`. Assert a form named `Note editor` with focus in `Title`.
- **Enter content.** Fill title and body. Assert `Save note` enabled.
- **Save note.** Choose `Save note`. Assert status `Note saved` and heading `Release checklist`.
- **Confirm persistence.** Reopen from the list. Assert both saved values.
- **Cancel draft.** Open new note, enter `Discard me`, choose `Cancel`. Assert list has no `Discard me`.
- **CLI entry.** `notes create --title "CLI note" --body "Created from terminal" --format json` → exit 0, id + title in stdout.
- **Proof.** Snapshot/screenshot list showing both titles under the named artifacts path.

## Gotchas

- `n` while a textbox is focused types the character.
- Assert rendered title after trim, not draft input.
- Save status alone is insufficient — reopen from the list.
- Remove fixture notes in cleanup; keep proof artifacts.
