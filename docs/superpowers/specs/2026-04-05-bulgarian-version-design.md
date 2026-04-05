# Bulgarian Version Design — `index-bg.html`

**Date:** 2026-04-05
**Author:** Claude Code (brainstorming session)

---

## Overview

Create a Bulgarian translation of the existing `index.html` tutorial, saved as a separate file `index-bg.html`. The target audience is a 13-year-old beginner with no coding experience.

## Goals

- Make all explanatory text accessible in Bulgarian
- Keep technical terms in English (they are universal in programming)
- Simplify tone for a young beginner — friendly, encouraging, short sentences
- Zero risk to the existing English version

## Non-Goals

- No language toggle / switcher
- No changes to CSS, JS engine, or localStorage logic
- No changes to code examples (code stays in English)

---

## Architecture

**Approach:** Copy `index.html` to `index-bg.html`, then translate all user-facing text in place.

**No structural changes** — the JS rendering engine, CSS theme, progress tracking, and localStorage logic are identical to the English version. Progress is tracked independently (different file = different localStorage keys are not shared, which is fine).

---

## What Gets Translated

### Page chrome
| English | Bulgarian |
|---|---|
| "AI Engineer Training" (title) | "Обучение за AI инженер" |
| Subtitle text | Translated |
| "X of Y modules complete" | "X от Y мисии завършени" |
| "Mark Done" button | "Отбележи като готово" |
| "Copy" button | "Копирай" |
| "Show Hint" / "Hide Hint" | "Покажи подсказка" / "Скрий подсказка" |
| "Complete Mission N first..." (locked message) | Translated |
| localStorage warning banner | Translated |

### Mission titles (kept in English with Bulgarian subtitle)
All 10 mission titles stay in English (Terminal, Python, Git, etc.) but each mission opens with a one-line Bulgarian description of what it is and why it matters.

### Mission content
Full translation of:
- Concept explanations
- Step-by-step instructions
- Code block captions and labels
- Hint text
- Challenge prompts and success criteria

### Technical term introductions
On first use of a technical term, add a short Bulgarian gloss in parentheses. Examples:
- *Terminal (терминал) — черният прозорец, с който говориш с компютъра*
- *API — начин да говориш с друга програма в интернет*
- *Git — програма, която помни всяка промяна в кода ти*

### Inline bilingual dictionary
Throughout the Bulgarian text, key words appear with their English equivalent in square brackets immediately after, acting as a mini-dictionary for the reader. This applies to:
- Technical terms: *"командата [command] се изпълнява веднага"*
- Action words: *"запази [save] файла"*, *"стартирай [run] програмата"*
- Concept words: *"грешка [error]"*, *"променлива [variable]"*, *"цикъл [loop]"*

The goal is that the son naturally learns the English programming vocabulary while reading in Bulgarian. Apply this consistently throughout all 10 missions.

This applies broadly — not just to technical terms, but also to common action words and everyday words used in instructions:
- Action verbs: *"Отвори [open]"*, *"Запази [save]"*, *"Натисни [press]"*, *"Стартирай [run]"*, *"Копирай [copy]"*, *"Виж [see]"*, *"Излез [exit]"*, *"Замени [replace]"*, *"Провери [check]"*
- Nouns: *"прозорец [window]"*, *"бутон [button]"*, *"акаунт [account]"*, *"страница [page]"*, *"отговор [answer]"*, *"грешка [error]"*
- The more brackets, the better — treat every Bulgarian sentence as a learning opportunity

---

## Tone Guidelines

- **Audience:** 13-year-old, zero coding experience
- **Style:** Friendly older sibling / cool mentor — not formal, not childish
- **Sentences:** Short and direct. One idea per sentence.
- **Encouragement:** Challenge prompts should feel exciting, not intimidating
- **Avoid:** Jargon without explanation, long paragraphs, passive voice

---

## Deliverable

- **File:** `/Users/derwin4o/kid-ai-tutorial/index-bg.html`
- **Commit:** `feat: add Bulgarian translation (index-bg.html)`

---

## Out of Scope

- Hosting or deployment
- Language detection / auto-redirect
- Further simplification of code examples
- New missions or content changes
