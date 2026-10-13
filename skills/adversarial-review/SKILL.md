---
name: adversarial-review
description: Opposite-model adversarial review — challenge a diff via a different model CLI; verdict only, no edits.
disable-model-invocation: true
metadata:
  author: mattpua
  version: "1.0.0"
  credit: Adapted from poteto/noodle adversarial-review (https://www.skills.sh/poteto/noodle/adversarial-review)
---

# adversarial-review

Challenge work with reviewers on a **different model** than the one running this skill. Deliverable is a synthesized verdict. Do not edit the codebase.

Opposite of **critique-me** (same-model review + auto-fix). Use this when you want independent challenge; use critique-me when you want fixes applied.

**Hard constraint:** Reviewers run only via an **opposite-model CLI** (e.g. Claude → `codex exec`, Codex → `claude -p`). Do not use same-harness subagents / Agent tool / internal delegation as reviewers — those share your model and defeat the point. If the opposite CLI is missing or fails, say so in the verdict; do not silently fall back to same-model review.

## Process

1. **Load frame** — Read project principles the user/repo treat as law (`AGENTS.md`, architecture notes, standing rules). If a principles index exists, follow its links. No project frame → use the lens questions alone and note that.
2. **Scope + intent** — Diff, files, or PR from context. State **intent** explicitly (what the author is trying to achieve). Reviewers judge whether the work achieves that intent well — not whether the intent is correct.
3. **Size → reviewers** — See table below. Lens text: `references/reviewer-lenses.md`.
4. **Spawn opposite-model reviewers** — Temp dir for outputs; one file per lens (`skeptic.md`, …). Build prompts from `references/reviewer-prompt.md`. Run reviewers in parallel. Default read-only; allow write/test profile only if a reviewer must run tests.
5. **Verify outputs** — Log which CLI was used; confirm each expected file exists and is non-empty. Missing reviewer → note failure in the verdict.
6. **Synthesize** — Deduplicate; format per `references/verdict-format.md`.
7. **Lead judgment** — Accept or reject each finding with a one-line why. Call out false positives and style-as-substance.

**Done when:** intent stated, expected reviewers attempted, verdict + lead judgment delivered, no code edits from this skill.

## Reviewer count

| Size | Threshold | Reviewers |
| --- | --- | --- |
| Small | < 50 lines, 1–2 files | Skeptic |
| Medium | 50–200 lines, 3–5 files | Skeptic + Architect |
| Large | 200+ lines or 5+ files | Skeptic + Architect + Minimalist |

## Opposite CLI

| You are | Spawn reviewers with |
| --- | --- |
| Claude (or Claude-family harness) | `codex exec` (read-only by default) |
| Codex / GPT-family CLI agent | `claude -p` |

Other pairings are fine if the reviewer model is **actually different**. Same weights behind a different wrapper do not count.

Example shapes (adapt flags to current CLI docs):

```sh
REVIEW_DIR=$(mktemp -d /tmp/adversarial-review.XXXXXX)
# Claude lead → Codex reviewer
codex exec --skip-git-repo-check -o "$REVIEW_DIR/skeptic.md" "<prompt>"
# Codex lead → Claude reviewer
claude -p "<prompt>" > "$REVIEW_DIR/skeptic.md"
```

## After the verdict

- Applying accepted fixes → user may invoke **critique-me** (or implement manually).
- Recurring class of miss → Call the Skill tool with `"living-docs"` only if the user asked you to capture lessons; this skill itself stays review-only on the product diff.
