# Task 2: Module Data + Rendering Engine

## Status
IN_PROGRESS

## What to implement
Replace the `<script>` placeholder in index.html with a complete JS rendering engine that:

1. **Storage (localStorage)**
   - Check if localStorage is available
   - `getProgress()` - retrieves {moduleId: true} map from localStorage
   - `saveProgress(moduleId)` - saves completed module

2. **MODULES array**
   - Initialized as empty `[]` (content added in Tasks 3-7)
   - Each element: {id: string, title: string, content: string}

3. **Rendering engine**
   - `renderModules()` - main render function
   - Handles locked/unlocked/completed states
   - Calculates if previous module is done to unlock current
   - Disables tabindex for locked modules' interactive children
   - Updates progress bar with aria-valuenow
   - Auto-opens first unlocked incomplete module

4. **User interactions**
   - `toggleModule(bodyId)` - open/close module, blocked for locked
   - `markDone(moduleId)` - save progress, re-render, scroll to next
   - `copyCode(btn)` - copy button interaction
   - `toggleHint(btn)` - show/hide hints

## Edge cases handled
- localStorage disabled (incognito): warning shown, functions gracefully return
- Empty MODULES array: renders "0 of 0 modules complete"
- Keyboard accessibility: locked modules can't be focused
- Smooth scroll to next module when one is marked done
