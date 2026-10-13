# Search notes

Search lets a user find notes by title or body, open a result, and distinguish no matches from unavailable search.

## Sub-features

- `search-open` opens search from each supported entry point.
- `search-match` returns title and body matches without changing data.
- `search-open-result` opens a result in the editor.
- `search-empty` shows empty state for no matches.
- `search-clear` clears the query and restores recent notes.
- `search-cli` returns the same matches from the terminal.

## How to get to it (user POV)

- Choose the `Search` button in the toolbar.
- Press `/` while focus is outside an editable field.
- Run `notes search <query>` in a terminal.

## Driving it with control-notes

Preconditions:

- App healthy; seeded notes include a known title/body pair.
- Doctor passes.

- **Toolbar / keyboard entry.** Open search; assert dialog + focus in searchbox.
- **Title / body match.** Type queries; assert expected hits and misses.
- **Open result.** Choose a hit; assert editor heading.
- **Empty / clear.** No-match status; clear restores recent list.
- **CLI.** JSON search hit and miss with exit 0.
- **Proof.** Artifacts show app identity, query, and a matching title.

## Gotchas

- `/` in an editable field inserts text.
- Wait for results or empty status (debounce), not a fixed sleep.
- Opening a result changes state — reopen search before the next query.
