# Kid AI Tutorial — Design Spec
**Date:** 2026-04-03
**Goal:** Interactive web tutorial for a 13-year-old on Ubuntu Linux to learn terminal, Python, and AI — ending with a real contribution to a parent's AI project.

---

## Overview

A single `index.html` file opened in Firefox on Ubuntu. No server, no install, no accounts. Progress is saved in `localStorage`. The kid completes 6 modules in order, each unlocking the next after a hands-on terminal challenge.

---

## Architecture

```
kid-ai-tutorial/
  index.html        ← entire app lives here
  README.md         ← "open index.html in Firefox"
```

- Pure HTML + CSS + JavaScript
- No external dependencies or CDN (works offline)
- `localStorage` key `tutorialProgress` stores which modules are completed

---

## Modules

| # | Title | Concepts | Challenge |
|---|-------|----------|-----------|
| 1 | The Terminal | `ls`, `pwd`, `cd`, `mkdir`, `python3 -c` | Create folder `ai-lab`, list its contents |
| 2 | Python in 30 min | variables, functions, loops, `input()` | Script that asks for name and says hello |
| 3 | What is AI? | How LLMs work (plain analogy), what a prompt is | Ask Claude one question, paste the answer |
| 4 | Your first API call | Install `anthropic` lib, 10-line script calling Claude | Call Claude with a custom prompt you write |
| 5 | Build something real | Combine skills into a small tool for dad's project | Show dad it works |
| 6 | Save your work with Git | `git init`, `git add`, `git commit`, `git log` | Commit their finished project |

Module 5 content is intentionally left open — the parent fills in the actual tool based on their current AI project.

---

## UI Design

- **Theme:** Dark terminal aesthetic — near-black background, green (#00ff88) monospace text, subtle scanline feel
- **Progress bar:** Top of page, shows "Module X of 6 complete"
- **Per module:**
  - Short explanation (plain language, no jargon)
  - Code blocks with one-click copy button
  - Hands-on challenge description
  - "Hint" toggle — hidden by default, reveals step-by-step help
  - "Mark as done ✓" button — saves progress, unlocks next module
- **Module lock:** Locked modules show a padlock icon and are greyed out until unlocked

---

## Data Flow

1. Page loads → reads `localStorage` → unlocks completed modules
2. Kid clicks "Mark as done" → writes to `localStorage` → next module unlocks with animation
3. Page refresh → state restored from `localStorage`
4. No data leaves the browser

---

## Error Handling

- If `localStorage` is unavailable (private mode), show a notice: "Progress won't be saved in private mode"
- Code copy button falls back to selecting text if clipboard API is unavailable

---

## Testing

Manual checklist:
- [ ] All 6 modules render correctly in Firefox on Ubuntu
- [ ] Completing module N unlocks module N+1
- [ ] Progress survives page refresh
- [ ] Copy buttons work
- [ ] Hints toggle correctly
- [ ] Works with no internet connection
