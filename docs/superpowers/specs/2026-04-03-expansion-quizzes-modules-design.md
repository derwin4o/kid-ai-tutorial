# Tutorial Expansion — Quizzes, Ask-Me-Anything, New Modules
**Date:** 2026-04-03
**Goal:** Add quiz gates, a kid's idea box, a global ask-me-anything bar, and 4 new modules to the existing 6-module tutorial.

---

## Overview

Three new interactive features added to `index.html`:
1. **Quiz gate** — replaces "Mark as done" button on every module. Kid must answer all questions correctly to unlock the next module.
2. **Kid's idea box** — appears after passing the quiz. Kid writes what they'd add; Claude responds with encouragement.
3. **Ask-me-anything bar** — sticky bottom bar, unlocks after module 4. Kid types any question; Claude answers inline.

Plus 4 new modules (7-10) following the existing learning arc.

---

## Architecture

Same single-file `index.html`. No new files. Changes:
- `MODULES` array: add `quiz` field to all 10 modules
- `renderModules()`: render quiz section instead of mark-done button
- New JS functions: `submitQuizAnswer()`, `submitIdea()`, `askClaude()`
- New HTML: sticky ask-me-anything bar at bottom of `<body>`
- New CSS: quiz buttons, response panel, idea box, sticky bar

---

## Feature 1: Quiz System

### Data shape
Each module gains a `quiz` field:
```js
quiz: [
  {
    q: "What command shows your current folder?",
    options: ["ls", "pwd", "cd", "mkdir"],
    answer: 1   // index into options[]
  }
]
```
2-3 questions per module.

### Render behavior
- Quiz renders after the challenge section, replacing the "Mark as done" button
- Questions shown one at a time
- Options rendered as buttons
- **Wrong answer:** button flashes red (`background: var(--warn)`), stays on same question, no penalty
- **Correct answer:** button flashes green, advances to next question
- **All correct:** shows animated "✓ Mission complete! Next mission unlocked." then triggers unlock
- Already-completed modules: quiz section replaced with "✓ Passed" badge (not re-takeable)

### State
Progress key `tutorialProgress` already in localStorage — no change needed. Quiz pass is recorded as module completion (same as before).

---

## Feature 2: Kid's Idea Box

### Behavior
- Appears below the quiz section after the quiz is passed
- Prompt text: *"What would YOU add to this mission? Tell Claude your idea."*
- Single `<textarea>` + "Send idea →" button
- Requires API key (same as ask-me-anything bar, stored under `anthropicKey` in localStorage).
  - If module 4 not yet complete: shows "Complete Mission 4 first to unlock Claude chat."
  - If module 4 complete but no key entered yet: shows "Enter your API key in the bar at the bottom of the page."
  - If key present: idea box is active.
- Claude responds with encouragement + 1-2 sentences expanding on the idea
- Response appears below the textarea, replaces previous response if re-submitted
- Uses `claude-haiku-4-5-20251001`, max 128 tokens
- System prompt: `"You are encouraging a 13-year-old learning to code. They just finished a programming lesson and shared an idea. Respond warmly in 2 sentences max."`

---

## Feature 3: Ask-Me-Anything Bar

### Behavior
- Fixed bar at bottom of viewport, `z-index: 100`
- Hidden until module 4 (`id: 'm4'`) is marked complete
- First use: shows API key input field. On submit, key saved to `localStorage` key `anthropicKey`.
- After key saved: shows question input + "Ask →" button
- Response panel slides up above the bar, shows newest response on top
- Max 3 responses shown (oldest removed when 4th arrives)
- Each question is independent (no conversation history)
- Uses `claude-haiku-4-5-20251001`, max 256 tokens
- System prompt: `"You are helping a 13-year-old learn programming and AI. Answer in plain language, max 3 sentences."`
- Loading state: button shows "thinking..." while waiting
- Error state: shows "Something went wrong — check your API key" in red

### API call (vanilla JS fetch)
```js
fetch('https://api.anthropic.com/v1/messages', {
  method: 'POST',
  headers: {
    'x-api-key': apiKey,
    'anthropic-version': '2023-06-01',
    'content-type': 'application/json',
    'anthropic-dangerous-direct-browser-access': 'true'
  },
  body: JSON.stringify({
    model: 'claude-haiku-4-5-20251001',
    max_tokens: 256,
    system: '...',
    messages: [{ role: 'user', content: question }]
  })
})
```

---

## New Modules

| # | id | Title | Key concepts | Challenge |
|---|----|-------|-------------|-----------|
| 7 | m7 | GitHub | `git push`, GitHub account, remote repos, `git pull` | Push ai-lab to a new GitHub repo, share the URL |
| 8 | m8 | Prompt Engineering | System prompts, role prompting, few-shot examples, why wording matters | Rewrite a vague prompt into a precise one; test both with Claude |
| 9 | m9 | Python + Files | `open()`, read/write `.txt`, iterate over lines | Read a text file and send each line to Claude for a one-line summary |
| 10 | m10 | Multi-turn Chat | Conversation history list, `messages` array, stateful chatbot | Build a chatbot loop that remembers the last 3 exchanges |

Each new module includes:
- Explanation + code examples
- Challenge
- 2-3 quiz questions
- Kid's idea box (after passing quiz)

---

## CSS Additions

- `.quiz-section` — container for quiz
- `.quiz-question` — question text
- `.quiz-option` — answer button (hover: green border)
- `.quiz-option.correct` — green flash animation
- `.quiz-option.wrong` — red flash animation
- `.quiz-passed` — "✓ Passed" badge for completed modules
- `.idea-box` — textarea + send button container
- `.idea-response` — Claude's response text
- `.ask-bar` — fixed bottom bar
- `.ask-bar-input` — question input
- `.ask-bar-btn` — submit button
- `.ask-responses` — response panel above bar
- `.ask-response-item` — individual response card

---

## Testing Checklist

- [ ] Quiz shows one question at a time
- [ ] Wrong answer flashes red, stays on question
- [ ] Correct answer advances to next question
- [ ] Passing all questions unlocks next module
- [ ] Already-passed modules show "✓ Passed" badge, not re-takeable
- [ ] Idea box hidden until quiz passed
- [ ] Idea box shows "Complete Mission 4 first" if no API key
- [ ] Idea box sends to Claude, response appears
- [ ] Ask bar hidden until module 4 complete
- [ ] API key input appears on first use, saved to localStorage
- [ ] Ask bar sends question to Claude, response appears above bar
- [ ] Max 3 responses shown, oldest removed on 4th
- [ ] Error state shown if API key wrong
- [ ] All 10 modules render correctly
- [ ] Progress survives page refresh
- [ ] Works offline (no external resources loaded)
