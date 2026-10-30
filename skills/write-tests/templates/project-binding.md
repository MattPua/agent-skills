# write-tests — project binding

Fill this in once per repo. The generic procedure lives in the installed `write-tests` skill.

## Layers

| Layer | Runner / file pattern | Use for | Helpers |
| --- | --- | --- | --- |
| Unit | `{{unit-runner}}` · `{{unit-pattern}}` | `{{unit-use}}` | `{{unit-helpers}}` |
| Component | `{{component-runner}}` · `{{component-pattern}}` | `{{component-use}}` | `{{component-helpers}}` |
| Integration | `{{integration-runner}}` · `{{integration-pattern}}` | `{{integration-use}}` | `{{integration-helpers}}` |
| End-to-end | `{{e2e-runner}}` · `{{e2e-pattern}}` | `{{e2e-use}}` | `{{e2e-helpers}}` |

## Budgets

- End-to-end: `{{e2e-budget}}` (e.g. critical hot paths and important mutations only)
- `{{other-budget}}`

## Mocking and isolation

- `{{mock-boundary}}` (e.g. mock the DB module at the package boundary)
- `{{isolation-rule}}`

## Naming

- `{{naming-rule}}` (e.g. integration test names state the business outcome only)

## Commands

| Need | Command |
| --- | --- |
| One file | `{{run-one}}` |
| Suite | `{{run-suite}}` |
