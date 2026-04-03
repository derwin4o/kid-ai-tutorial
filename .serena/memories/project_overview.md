# Kid AI Tutorial - Project Overview

## Purpose
An interactive, self-paced AI engineer training tutorial delivered as a single HTML file. Users progress through 5 missions sequentially, with progress saved to localStorage. Built with HTML/CSS/JS.

## Tech Stack
- Pure HTML5, vanilla CSS3, and vanilla JavaScript (no build tools or external dependencies)
- localStorage for persistence
- Single file application: index.html

## Key Features
- Dark terminal-themed UI (color scheme with CSS variables)
- Progress tracking (completed/unlocked/locked module states)
- Sequential unlock system (must complete module N to unlock module N+1)
- Accessibility: keyboard navigation, aria labels, tabindex management
- Code copy buttons and hint toggles within each module
- Module data defined in JS MODULES array (populated in Tasks 3-7)
- Rendering engine handles empty MODULES array gracefully

## Codebase Structure
- `/Users/derwin4o/kid-ai-tutorial/index.html` - Single file containing all HTML, CSS, and JS
- `/README.md` - Basic project description
- `/docs/` - Task plans and specs from superpowers

## Code Style & Conventions
- HTML5 semantic structure
- CSS variables for theming (--bg, --green, --text, etc.)
- camelCase for JS functions and variables
- Inline comments with ── headers for section organization
- Comments mark intended Task boundaries (e.g., "// placeholder — logic added in Task 2")

## Module Structure (MODULES array element)
Each module object has:
- `id` - string identifier, used in localStorage keys
- `title` - displayed as "Mission N: [title]"
- `content` - HTML string for module body (inserted via innerHTML)

Module UI state:
- locked: opacity 0.45, pointer-events none, tabindex -1 on children
- unlocked: border-color green-dim
- open: module-body display block
- done: green status text, disabled mark-done button

## Task Completion Checklist
When a task is complete:
1. Replace/add code as specified
2. Verify with browser console if applicable
3. Run: `git add [files]`
4. Run: `git commit -m "feat: [description]"` (follow conventional commits)
5. Self-review for code quality
6. Report status and commit hash
