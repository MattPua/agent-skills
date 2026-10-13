---
name: unslop
description: Edit prose to remove AI writing patterns and restore human voice.
disable-model-invocation: true
metadata:
  author: mattpua
  version: "1.0.0"
  credit: Adapted from poteto/noodle unslop (https://github.com/poteto/noodle)
---

# Unslop

Edit text to remove AI patterns and add human voice. For docs, READMEs, blog posts, and other non-code prose.

## Process

1. Scan for the patterns in `references/patterns.md`.
2. Rewrite — preserve meaning, match intended tone.
3. Add soul (below).
4. Self-audit: “What makes this obviously AI generated?” Fix remaining tells.
5. Done when a cold read no longer screams template.

## Adding soul

Removing patterns is half the job. Sterile, voiceless writing is just as obvious.

- **Have opinions.** React to facts instead of neutrally listing pros and cons.
- **Vary rhythm.** Short sentences. Then longer ones that take their time.
- **Acknowledge complexity.** “Impressive but also kind of unsettling” beats “impressive.”
- **Use “I” when it fits.** First person isn’t unprofessional.
- **Let some mess in.** Perfect structure feels algorithmic.
- **Be specific.** Not “this is concerning” but a concrete image or detail.

## Scope

- In scope: prose the user pointed at (or the file they named).
- Out of scope: rewriting code, inventing new product claims, expanding scope into a full content strategy.
