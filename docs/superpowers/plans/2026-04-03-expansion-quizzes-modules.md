# Tutorial Expansion — Quizzes, Ask-Me-Anything, Modules 7-10

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Add quiz gates (replacing mark-done buttons), a kid's idea box powered by Claude, a global ask-me-anything bar, and 4 new modules (GitHub, Prompt Engineering, Python Files, Multi-turn Chat) to the existing 6-module tutorial.

**Architecture:** All changes are in a single file `index.html`. The quiz system replaces the mark-done button in `renderModules()`. The idea box renders after the quiz when a module is complete. The ask-me-anything bar is a fixed HTML element at the bottom of the page. Both Claude features use a shared `callClaude()` utility and the API key stored in `localStorage` key `anthropicKey`.

**Tech Stack:** Vanilla HTML/CSS/JS, Anthropic Messages API (fetch from browser), localStorage

---

## File Map

| File | Changes |
|------|---------|
| `kid-ai-tutorial/index.html` | All changes — CSS additions, JS additions, modified renderModules, quiz data on m1-m6, 4 new module objects |

---

### Task 1: CSS — quiz, idea box, ask bar

**Files:**
- Modify: `kid-ai-tutorial/index.html` — add CSS inside `<style>` block, before the closing `</style>` tag (currently around line 196)

- [ ] **Step 1: Add CSS for quiz system, idea box, and ask bar**

Find the `</style>` tag and insert this CSS immediately before it:

```css
    /* ── Quiz ─────────────────────────────────────────────────────── */
    .quiz-section { margin-top: 1.2rem; }
    .quiz-question { color: var(--text); font-size: 0.9rem; margin-bottom: 0.8rem; line-height: 1.6; }
    .quiz-options { display: flex; flex-direction: column; gap: 0.5rem; }
    .quiz-option {
      background: none;
      border: 1px solid var(--grey);
      color: var(--text);
      border-radius: 4px;
      padding: 0.5rem 0.8rem;
      font-size: 0.85rem;
      cursor: pointer;
      font-family: var(--font);
      text-align: left;
      transition: border-color 0.15s, background 0.15s;
    }
    .quiz-option:hover { border-color: var(--green-dim); color: var(--green-dim); }
    .quiz-option.correct {
      border-color: var(--green);
      background: rgba(0,255,136,0.1);
      color: var(--green);
      animation: flashGreen 0.4s ease;
    }
    .quiz-option.wrong {
      border-color: var(--warn);
      background: rgba(255,136,68,0.1);
      color: var(--warn);
      animation: flashRed 0.4s ease;
    }
    @keyframes flashGreen {
      0% { background: rgba(0,255,136,0.4); }
      100% { background: rgba(0,255,136,0.1); }
    }
    @keyframes flashRed {
      0% { background: rgba(255,136,68,0.4); }
      100% { background: rgba(255,136,68,0.1); }
    }
    .quiz-passed {
      color: var(--green);
      font-size: 0.85rem;
      padding: 0.4rem 0;
    }
    .quiz-progress { color: var(--grey); font-size: 0.75rem; margin-bottom: 0.6rem; }
    /* ── Idea box ──────────────────────────────────────────────────── */
    .idea-box {
      margin-top: 1.2rem;
      border-top: 1px solid var(--grey);
      padding-top: 1rem;
    }
    .idea-prompt { color: var(--green-dim); font-size: 0.85rem; margin-bottom: 0.6rem; }
    .idea-textarea {
      width: 100%;
      background: #111;
      border: 1px solid var(--grey);
      border-radius: 4px;
      color: var(--text);
      font-family: var(--font);
      font-size: 0.85rem;
      padding: 0.6rem 0.8rem;
      resize: vertical;
      min-height: 60px;
    }
    .idea-textarea:focus { outline: none; border-color: var(--green-dim); }
    .idea-send-btn {
      margin-top: 0.5rem;
      background: none;
      border: 1px solid var(--green-dim);
      color: var(--green-dim);
      border-radius: 4px;
      padding: 0.3rem 0.9rem;
      font-size: 0.8rem;
      cursor: pointer;
      font-family: var(--font);
    }
    .idea-send-btn:hover { background: var(--green-dim); color: #000; }
    .idea-response {
      margin-top: 0.6rem;
      font-size: 0.85rem;
      color: #aaa;
      line-height: 1.6;
      font-style: italic;
      display: none;
    }
    .idea-locked { color: var(--grey); font-size: 0.8rem; margin-top: 0.5rem; }
    /* ── Ask bar ───────────────────────────────────────────────────── */
    .ask-bar {
      position: fixed;
      bottom: 0;
      left: 0;
      right: 0;
      background: #111;
      border-top: 1px solid var(--green-dim);
      padding: 0.6rem 1rem;
      z-index: 100;
    }
    .ask-bar-inner { max-width: 800px; margin: 0 auto; }
    .ask-bar-label { color: var(--green-dim); font-size: 0.7rem; margin-bottom: 0.4rem; }
    .ask-bar-row { display: flex; gap: 0.5rem; }
    .ask-bar-input {
      flex: 1;
      background: var(--bg);
      border: 1px solid var(--grey);
      border-radius: 4px;
      color: var(--text);
      font-family: var(--font);
      font-size: 0.85rem;
      padding: 0.4rem 0.7rem;
    }
    .ask-bar-input:focus { outline: none; border-color: var(--green-dim); }
    .ask-bar-btn {
      background: none;
      border: 1px solid var(--green);
      color: var(--green);
      border-radius: 4px;
      padding: 0.4rem 0.9rem;
      font-size: 0.85rem;
      cursor: pointer;
      font-family: var(--font);
      white-space: nowrap;
    }
    .ask-bar-btn:hover { background: var(--green); color: #000; }
    .ask-bar-btn:disabled { border-color: var(--grey); color: var(--grey); cursor: default; }
    .ask-responses {
      max-width: 800px;
      margin: 0 auto 0.5rem;
    }
    .ask-response-item {
      background: var(--bg2);
      border: 1px solid var(--grey);
      border-radius: 4px;
      padding: 0.6rem 0.8rem;
      margin-bottom: 0.4rem;
      font-size: 0.85rem;
      color: var(--text);
      line-height: 1.6;
    }
    .ask-response-q { color: var(--green-dim); font-size: 0.75rem; margin-bottom: 0.3rem; }
    .ask-error { color: var(--warn); font-size: 0.8rem; margin-top: 0.3rem; }
    /* padding so content doesn't hide behind fixed bar */
    body.ask-bar-visible { padding-bottom: 120px; }
```

- [ ] **Step 2: Verify no CSS syntax errors**

Open `index.html` in Firefox. Expected: page renders normally, no visual breakage. No console errors.

- [ ] **Step 3: Commit**

```bash
cd /Users/derwin4o/kid-ai-tutorial
git add index.html
git commit -m "feat: add CSS for quiz, idea box, ask bar"
```

---

### Task 2: callClaude utility + quizState

**Files:**
- Modify: `kid-ai-tutorial/index.html` — add JS after `saveProgress` function (around line 234)

- [ ] **Step 1: Add quizState variable and callClaude utility after the `saveProgress` function**

Find:
```js
    // ── Module definitions (content added in Tasks 3-7) ──────────────
```

Insert immediately before that comment:

```js
    // ── Quiz state (in-memory, resets on page refresh) ────────────────
    const quizState = {}; // { moduleId: currentQuestionIndex }

    // ── Claude API utility ────────────────────────────────────────────
    async function callClaude(userMessage, systemPrompt, maxTokens) {
      const apiKey = localStorage.getItem('anthropicKey');
      if (!apiKey) throw new Error('no-key');
      const response = await fetch('https://api.anthropic.com/v1/messages', {
        method: 'POST',
        headers: {
          'x-api-key': apiKey,
          'anthropic-version': '2023-06-01',
          'content-type': 'application/json',
          'anthropic-dangerous-direct-browser-access': 'true'
        },
        body: JSON.stringify({
          model: 'claude-haiku-4-5-20251001',
          max_tokens: maxTokens,
          system: systemPrompt,
          messages: [{ role: 'user', content: userMessage }]
        })
      });
      if (!response.ok) throw new Error('api-error');
      const data = await response.json();
      return data.content[0].text;
    }

```

- [ ] **Step 2: Verify in browser console**

Open DevTools. Run: `typeof callClaude`
Expected: `"function"`

Run: `typeof quizState`
Expected: `"object"`

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add quizState and callClaude utility"
```

---

### Task 3: Quiz render engine — renderQuizQuestion + submitQuizAnswer + modify renderModules

**Files:**
- Modify: `kid-ai-tutorial/index.html`

- [ ] **Step 1: Add renderQuizQuestion and submitQuizAnswer functions**

Find:
```js
    function renderModules() {
```

Insert immediately before it:

```js
    // ── Quiz engine ───────────────────────────────────────────────────
    function renderQuizQuestion(moduleId, quiz) {
      const currentQ = quizState[moduleId] || 0;
      if (!quiz || quiz.length === 0) return '<div class="quiz-passed">✓ Complete</div>';
      const q = quiz[currentQ];
      return `
        <div class="quiz-progress">Question ${currentQ + 1} of ${quiz.length}</div>
        <div class="quiz-question">${q.q}</div>
        <div class="quiz-options">
          ${q.options.map((opt, i) =>
            `<button class="quiz-option" id="qopt-${moduleId}-${i}"
              onclick="submitQuizAnswer('${moduleId}', ${currentQ}, ${i})">${opt}</button>`
          ).join('')}
        </div>
      `;
    }

    function submitQuizAnswer(moduleId, questionIndex, selectedIndex) {
      const mod = MODULES.find(m => m.id === moduleId);
      const correct = mod.quiz[questionIndex].answer;
      const btn = document.getElementById(`qopt-${moduleId}-${selectedIndex}`);

      if (selectedIndex !== correct) {
        // Wrong — flash red, reset after animation
        btn.classList.add('wrong');
        // Disable all options briefly to prevent rapid clicking
        const allBtns = btn.closest('.quiz-options').querySelectorAll('.quiz-option');
        allBtns.forEach(b => b.disabled = true);
        setTimeout(() => {
          btn.classList.remove('wrong');
          allBtns.forEach(b => b.disabled = false);
        }, 600);
        return;
      }

      // Correct — flash green, then advance
      btn.classList.add('correct');
      const allBtns = btn.closest('.quiz-options').querySelectorAll('.quiz-option');
      allBtns.forEach(b => b.disabled = true);

      setTimeout(() => {
        const nextQ = questionIndex + 1;
        if (nextQ < mod.quiz.length) {
          // More questions — update quiz section in place
          quizState[moduleId] = nextQ;
          document.getElementById(`quiz-${moduleId}`).innerHTML = renderQuizQuestion(moduleId, mod.quiz);
        } else {
          // All questions correct — complete the module
          saveProgress(moduleId);
          renderModules();
          // Scroll to next module
          const idx = MODULES.findIndex(m => m.id === moduleId);
          if (idx < MODULES.length - 1) {
            setTimeout(() => {
              const next = document.querySelectorAll('.module')[idx + 1];
              if (next) next.scrollIntoView({ behavior: 'smooth', block: 'start' });
            }, 300);
          }
        }
      }, 500);
    }

```

- [ ] **Step 2: Replace the mark-done button in renderModules with quiz section**

Find in `renderModules`:
```js
            <button class="mark-done-btn" id="btn-${mod.id}"
              onclick="markDone('${mod.id}')"
              ${isDone ? 'disabled' : ''}>
              ${isDone ? '✓ Already completed' : '✓ Mark as done — unlock next mission'}
            </button>
```

Replace with:
```js
            <div class="quiz-section" id="quiz-${mod.id}">
              ${isDone
                ? '<div class="quiz-passed">✓ Quiz passed</div>'
                : renderQuizQuestion(mod.id, mod.quiz || [])}
            </div>
```

- [ ] **Step 3: Verify in browser**

Open `index.html`. Module 1 should show "Question 1 of 0" (no quiz data yet — added in Task 4).
Actually: with `mod.quiz || []` and empty array, `renderQuizQuestion` returns `'<div class="quiz-passed">✓ Complete</div>'`. That's fine as a temporary state.

Expected: Module 1 shows "✓ Complete" at the bottom where the mark-done button was. No JS errors in console.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add quiz engine - renderQuizQuestion + submitQuizAnswer"
```

---

### Task 4: Add quiz data to modules 1-6

**Files:**
- Modify: `kid-ai-tutorial/index.html` — add `quiz` field to each of the 6 existing module objects

- [ ] **Step 1: Add quiz to module m1 (The Terminal)**

Find the m1 object opening:
```js
      {
        id: 'm1',
        title: 'The Terminal',
        content: `
```

Add a `quiz` field after the closing backtick of `content`. Find:
```js
        `
      },
      {
        id: 'm2',
```

Replace with:
```js
        `,
        quiz: [
          { q: 'What command shows your current folder?', options: ['ls', 'pwd', 'cd', 'mkdir'], answer: 1 },
          { q: 'What command creates a new folder called ai-lab?', options: ['pwd ai-lab', 'cd ai-lab', 'mkdir ai-lab', 'touch ai-lab'], answer: 2 },
          { q: 'How do you run a Python script called hello.py?', options: ['run hello.py', 'python3 hello.py', 'start hello.py', 'execute hello.py'], answer: 1 }
        ]
      },
      {
        id: 'm2',
```

- [ ] **Step 2: Add quiz to module m2 (Python in 30 min)**

Find:
```js
        `
      },
      {
        id: 'm3',
```

Replace with:
```js
        `,
        quiz: [
          { q: 'Which keyword defines a function in Python?', options: ['func', 'define', 'def', 'function'], answer: 2 },
          { q: 'What does input() do?', options: ['Prints text to the screen', 'Asks the user to type something', 'Creates a variable', 'Loops forever'], answer: 1 },
          { q: 'What is the output of: print(2 + 3)?', options: ['23', '2+3', '5', 'Error'], answer: 2 }
        ]
      },
      {
        id: 'm3',
```

- [ ] **Step 3: Add quiz to module m3 (What is AI?)**

Find:
```js
        `
      },
      {
        id: 'm4',
```

Replace with:
```js
        `,
        quiz: [
          { q: 'What is a prompt?', options: ['A type of Python error', 'The message you send to an AI', 'A computer program', 'A database query'], answer: 1 },
          { q: 'What does API stand for?', options: ['Artificial Programming Intelligence', 'Application Programming Interface', 'Automated Process Integration', 'Advanced Python Interface'], answer: 1 },
          { q: 'What do LLMs predict?', options: ['The weather', 'Stock prices', 'The most helpful next word or sentence', 'Your password'], answer: 2 }
        ]
      },
      {
        id: 'm4',
```

- [ ] **Step 4: Add quiz to module m4 (Your first API call)**

Find:
```js
        `
      },
      {
        id: 'm5',
```

Replace with:
```js
        `,
        quiz: [
          { q: 'What Python library do you install to use Claude?', options: ['openai', 'anthropic', 'claude', 'requests'], answer: 1 },
          { q: 'What does max_tokens control?', options: ['The speed of the response', 'How long the response can be', 'The cost of the API', 'The model version'], answer: 1 },
          { q: 'Where should you NEVER put your real API key?', options: ['In a script you commit to Git', 'In a .env file', 'In your memory', 'In a config file you keep private'], answer: 0 }
        ]
      },
      {
        id: 'm5',
```

- [ ] **Step 5: Add quiz to module m5 (Build something real)**

Find:
```js
        `
      },
      {
        id: 'm6',
```

Replace with:
```js
        `,
        quiz: [
          { q: 'In the template, what function wraps the Claude API call?', options: ['call()', 'ask()', 'claude()', 'query()'], answer: 1 },
          { q: 'What Python function gets input from the user?', options: ['read()', 'scan()', 'input()', 'get()'], answer: 2 },
          { q: 'What should you do if your script gives an error?', options: ['Delete it and start over', 'Paste the error into Claude and ask what it means', 'Give up', 'Ignore it'], answer: 1 }
        ]
      },
      {
        id: 'm6',
```

- [ ] **Step 6: Add quiz to module m6 (Save your work with Git)**

Find:
```js
        `
      }
    ];
```

Replace with:
```js
        `,
        quiz: [
          { q: 'What command creates a new git repository?', options: ['git start', 'git create', 'git init', 'git new'], answer: 2 },
          { q: 'What command stages all files for a commit?', options: ['git save .', 'git add .', 'git stage .', 'git push .'], answer: 1 },
          { q: 'What does git log show?', options: ['Your current files', 'The history of your commits', 'The git settings', 'Remote repositories'], answer: 1 }
        ]
      }
    ];
```

- [ ] **Step 7: Verify quiz works end-to-end**

Open `index.html`. Module 1 should now show "Question 1 of 3" with 4 answer buttons.
- Click a wrong answer → button flashes red, resets
- Click the correct answer (pwd) → green flash, advances to question 2
- Answer all 3 correctly → "✓ Quiz passed" appears on module 1, module 2 unlocks

- [ ] **Step 8: Commit**

```bash
git add index.html
git commit -m "feat: add quiz data to modules 1-6"
```

---

### Task 5: Idea box — renderIdeaBox + submitIdea

**Files:**
- Modify: `kid-ai-tutorial/index.html`

- [ ] **Step 1: Add renderIdeaBox and submitIdea functions**

Find:
```js
    // ── Quiz engine ───────────────────────────────────────────────────
```

Insert immediately before it:

```js
    // ── Idea box ──────────────────────────────────────────────────────
    function hasApiKey() {
      return !!localStorage.getItem('anthropicKey');
    }

    function m4Complete() {
      return !!getProgress()['m4'];
    }

    function renderIdeaBox(moduleId) {
      if (!m4Complete()) {
        return `<div class="idea-box"><p class="idea-locked">💡 Complete Mission 4 to unlock Claude chat and share your ideas.</p></div>`;
      }
      if (!hasApiKey()) {
        return `<div class="idea-box"><p class="idea-locked">💡 Enter your API key in the bar at the bottom of the page to share your ideas with Claude.</p></div>`;
      }
      return `
        <div class="idea-box">
          <p class="idea-prompt">💡 What would YOU add to this mission? Tell Claude your idea.</p>
          <textarea class="idea-textarea" id="idea-input-${moduleId}"
            placeholder="e.g. I'd add a module about making a website with AI..."></textarea>
          <button class="idea-send-btn" onclick="submitIdea('${moduleId}')">Send idea →</button>
          <div class="idea-response" id="idea-response-${moduleId}"></div>
        </div>
      `;
    }

    async function submitIdea(moduleId) {
      const textarea = document.getElementById(`idea-input-${moduleId}`);
      const responseEl = document.getElementById(`idea-response-${moduleId}`);
      const btn = textarea.nextElementSibling;
      const text = textarea.value.trim();
      if (!text) return;

      btn.textContent = 'Sending...';
      btn.disabled = true;
      responseEl.style.display = 'none';

      try {
        const reply = await callClaude(
          text,
          'You are encouraging a 13-year-old who is learning to code. They just finished a programming lesson and shared an idea for extending it. Respond warmly and specifically in 2 sentences max.',
          128
        );
        responseEl.textContent = reply;
        responseEl.style.display = 'block';
      } catch(e) {
        responseEl.textContent = e.message === 'no-key'
          ? 'Enter your API key in the bar below first.'
          : 'Something went wrong — check your API key.';
        responseEl.style.color = 'var(--warn)';
        responseEl.style.display = 'block';
      } finally {
        btn.textContent = 'Send idea →';
        btn.disabled = false;
      }
    }

```

- [ ] **Step 2: Add idea box rendering to renderModules after the quiz section**

Find in `renderModules` the template literal, specifically:
```js
            <div class="quiz-section" id="quiz-${mod.id}">
              ${isDone
                ? '<div class="quiz-passed">✓ Quiz passed</div>'
                : renderQuizQuestion(mod.id, mod.quiz || [])}
            </div>
```

Replace with:
```js
            <div class="quiz-section" id="quiz-${mod.id}">
              ${isDone
                ? '<div class="quiz-passed">✓ Quiz passed</div>'
                : renderQuizQuestion(mod.id, mod.quiz || [])}
            </div>
            ${isDone ? renderIdeaBox(mod.id) : ''}
```

- [ ] **Step 3: Verify idea box**

Complete module 1 by answering all 3 quiz questions correctly.
Expected: Idea box appears below the quiz section.

If module 4 is not complete: shows "Complete Mission 4 to unlock Claude chat."
After completing module 4 and entering an API key in the bar: idea box becomes active.

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add idea box with Claude response"
```

---

### Task 6: Ask-me-anything bar

**Files:**
- Modify: `kid-ai-tutorial/index.html`

- [ ] **Step 1: Add the ask bar HTML before the `<script>` tag**

The ask bar HTML must be placed BEFORE the `<script>` tag so it exists in the DOM when the script runs. Find the line `  <script>` (the main script block near the bottom of `<body>`) and insert immediately before it:

```html
  <div class="ask-bar" id="ask-bar" style="display:none" aria-label="Ask Claude anything">
    <div class="ask-responses" id="ask-responses"></div>
    <div class="ask-bar-inner">
      <div class="ask-bar-label" id="ask-bar-label">Ask Claude anything about what you learned →</div>
      <div class="ask-bar-row">
        <input class="ask-bar-input" id="ask-bar-input" type="text"
          placeholder="Type a question..." autocomplete="off" />
        <button class="ask-bar-btn" id="ask-bar-btn" onclick="handleAskBar()">Ask →</button>
      </div>
      <div class="ask-error" id="ask-error" style="display:none"></div>
    </div>
  </div>
```

- [ ] **Step 2: Add ask bar JS functions**

Find:
```js
    // Initial render
    renderModules();
```

Insert immediately before it:

```js
    // ── Ask-me-anything bar ────────────────────────────────────────────
    let askBarMode = 'question'; // 'key' | 'question'

    function updateAskBarVisibility() {
      const bar = document.getElementById('ask-bar');
      if (!m4Complete()) { bar.style.display = 'none'; document.body.classList.remove('ask-bar-visible'); return; }
      bar.style.display = 'block';
      document.body.classList.add('ask-bar-visible');
      if (!hasApiKey()) {
        askBarMode = 'key';
        document.getElementById('ask-bar-label').textContent = 'Enter your Anthropic API key to unlock Claude chat:';
        document.getElementById('ask-bar-input').placeholder = 'sk-ant-...';
        document.getElementById('ask-bar-input').type = 'password';
        document.getElementById('ask-bar-btn').textContent = 'Save →';
      } else {
        askBarMode = 'question';
        document.getElementById('ask-bar-label').textContent = 'Ask Claude anything about what you learned →';
        document.getElementById('ask-bar-input').placeholder = 'Type a question...';
        document.getElementById('ask-bar-input').type = 'text';
        document.getElementById('ask-bar-btn').textContent = 'Ask →';
      }
    }

    async function handleAskBar() {
      const input = document.getElementById('ask-bar-input');
      const btn = document.getElementById('ask-bar-btn');
      const errorEl = document.getElementById('ask-error');
      const val = input.value.trim();
      if (!val) return;
      errorEl.style.display = 'none';

      if (askBarMode === 'key') {
        localStorage.setItem('anthropicKey', val);
        input.value = '';
        updateAskBarVisibility();
        renderModules(); // re-render to update idea boxes
        return;
      }

      // Ask mode
      btn.textContent = 'thinking...';
      btn.disabled = true;
      input.disabled = true;

      try {
        const answer = await callClaude(
          val,
          'You are helping a 13-year-old learn programming and AI. Answer clearly in plain language, maximum 3 sentences.',
          256
        );
        addAskResponse(val, answer);
        input.value = '';
      } catch(e) {
        errorEl.textContent = 'Something went wrong — check your API key.';
        errorEl.style.display = 'block';
      } finally {
        btn.textContent = 'Ask →';
        btn.disabled = false;
        input.disabled = false;
        input.focus();
      }
    }

    function addAskResponse(question, answer) {
      const container = document.getElementById('ask-responses');
      const item = document.createElement('div');
      item.className = 'ask-response-item';
      item.innerHTML = `<div class="ask-response-q">You asked: ${question.replace(/</g,'&lt;')}</div>${answer.replace(/</g,'&lt;')}`;
      container.insertBefore(item, container.firstChild);
      // Keep max 3 responses
      while (container.children.length > 3) container.removeChild(container.lastChild);
    }

    // Allow Enter key in ask bar (ask-bar-input is already in DOM since HTML is before this script)
    document.getElementById('ask-bar-input').addEventListener('keydown', e => {
      if (e.key === 'Enter') handleAskBar();
    });

```

- [ ] **Step 3: Call updateAskBarVisibility from renderModules**

Find at the end of `renderModules`:
```js
      document.getElementById('progress-label').textContent =
        completedCount + ' of ' + MODULES.length + ' modules complete';
    }
```

Replace with:
```js
      document.getElementById('progress-label').textContent =
        completedCount + ' of ' + MODULES.length + ' modules complete';
      updateAskBarVisibility();
    }
```

- [ ] **Step 4: Verify ask bar behavior**

Open `index.html`. Expected: ask bar NOT visible (module 4 not complete).

Complete modules 1-4 via quiz. Expected: ask bar appears at bottom of page with API key input.

Enter API key. Expected: bar switches to question mode.

Type a question and press Enter or click Ask. Expected: response appears above bar after a moment. Button shows "thinking..." while waiting.

Ask 4 questions. Expected: only 3 responses shown (oldest removed).

- [ ] **Step 5: Commit**

```bash
git add index.html
git commit -m "feat: add ask-me-anything bar with API key entry and Claude responses"
```

---

### Task 7: Module 7 — GitHub

**Files:**
- Modify: `kid-ai-tutorial/index.html` — add module m7 to MODULES array

- [ ] **Step 1: Add module m7 after the closing `};` of m6**

Find:
```js
      }
    ];
```

Replace with:
```js
      },
      {
        id: 'm7',
        title: 'GitHub — Put Your Code Online',
        content: `
          <p>Git saves your code locally. GitHub puts it on the internet so it's backed up,
          shareable, and safe. Every real project lives on GitHub.</p>

          <h3>What is GitHub?</h3>
          <p>GitHub is a website that stores Git repositories. Think of it as Google Drive
          for code — but smarter, because it knows about commits and history.</p>

          <h3>Step 1: Create an account</h3>
          <p>Go to <strong>github.com</strong> and create a free account.
          Ask dad to help if needed.</p>

          <h3>Step 2: Create a new repository</h3>
          <p>Click the <strong>+</strong> button → "New repository". Name it <code style="color:var(--green)">ai-lab</code>.
          Leave it public. Do NOT add a README (you already have your files). Click "Create repository".</p>

          <h3>Step 3: Connect and push</h3>
          <p>GitHub will show you commands. Copy the ones that start with <code style="color:var(--green)">git remote add</code>.
          They look like this:</p>
          <div class="code-block">
            <pre>git remote add origin https://github.com/YOUR_USERNAME/ai-lab.git
git branch -M main
git push -u origin main</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>Step 4: Pull — get the latest changes</h3>
          <p>If someone else changes your code (or you edit on GitHub directly), use:</p>
          <div class="code-block">
            <pre>git pull</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Mission 7 Challenge</div>
            <p>1. Create a GitHub account (or use dad's).<br>
            2. Create a new repo called <strong>ai-lab</strong>.<br>
            3. Push your local ai-lab folder to it.<br>
            4. Open the GitHub page and see your code online.</p>
            <button class="hint-toggle" onclick="toggleHint(this)">show hint</button>
            <div class="hint-body">
              Make sure you're inside the ai-lab folder first:<br>
              <code style="color:var(--green)">cd ~/ai-lab</code><br><br>
              Then run the three commands GitHub shows you after creating the repo.
              The first one is <code style="color:var(--green)">git remote add origin ...</code>
            </div>
          </div>
        `,
        quiz: [
          { q: 'What command connects your local repo to GitHub?', options: ['git connect', 'git remote add origin', 'git link', 'git attach'], answer: 1 },
          { q: 'What command uploads your commits to GitHub?', options: ['git upload', 'git send', 'git push', 'git sync'], answer: 2 },
          { q: 'What command downloads the latest changes from GitHub?', options: ['git download', 'git fetch', 'git pull', 'git get'], answer: 2 }
        ]
      }
    ];
```

- [ ] **Step 2: Verify module 7 renders**

Open `index.html`. Complete modules 1-6 using the quiz. Module 7 should unlock.
Expected: "Mission 7: GitHub — Put Your Code Online" appears, locked initially, unlocks after m6 quiz passed.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Module 7 - GitHub"
```

---

### Task 8: Module 8 — Prompt Engineering

**Files:**
- Modify: `kid-ai-tutorial/index.html` — add module m8

- [ ] **Step 1: Add m8 after m7**

Find the closing `];` after m7 and replace with:

```js
      },
      {
        id: 'm8',
        title: 'Prompt Engineering',
        content: `
          <p>You can get dramatically better answers from Claude just by changing how you ask.
          This skill — writing good prompts — is called <strong style="color:var(--green)">prompt engineering</strong>.</p>

          <h3>Be specific</h3>
          <p>Bad: <em>"Tell me about dogs."</em><br>
          Good: <em>"In 3 bullet points, explain what makes dogs good pets for families with young children."</em></p>

          <h3>Give Claude a role (system prompt)</h3>
          <p>You can set a <strong style="color:var(--green)">system prompt</strong> to tell Claude who it should be:</p>
          <div class="code-block">
            <pre>import anthropic

client = anthropic.Anthropic(api_key="YOUR_API_KEY_HERE")

message = client.messages.create(
    model="claude-haiku-4-5-20251001",
    max_tokens=256,
    system="You are a friendly Python teacher for beginners.",
    messages=[
        {"role": "user", "content": "What is a for loop?"}
    ]
)
print(message.content[0].text)</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>Few-shot examples</h3>
          <p>Show Claude what you want by giving examples before your question:</p>
          <div class="code-block">
            <pre>prompt = """
Translate these sentences to pirate speak:
Hello → Ahoy!
How are you? → How be ye?
Now translate this: What time is it?
"""</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Mission 8 Challenge</div>
            <p>Write two versions of a prompt asking Claude to explain what a variable is:<br>
            1. A vague version (short, no context)<br>
            2. A specific version (role + audience + format)<br>
            Run both with your script and compare the answers.</p>
            <button class="hint-toggle" onclick="toggleHint(this)">show hint</button>
            <div class="hint-body">
              Vague: <code style="color:var(--green)">"What is a variable?"</code><br><br>
              Specific: <code style="color:var(--green)">"You are a teacher for 13-year-olds.
              Explain what a variable is in Python using one real-world analogy. Keep it under 3 sentences."</code>
            </div>
          </div>
        `,
        quiz: [
          { q: 'What is a system prompt?', options: ['An error message from the terminal', 'Instructions that tell the AI how to behave before the conversation starts', 'A type of Python function', 'The user\'s first message'], answer: 1 },
          { q: 'What is few-shot prompting?', options: ['Using a small AI model', 'Giving the AI examples of what you want before your question', 'A fast way to run code', 'Sending short messages'], answer: 1 },
          { q: 'Which prompt will get a better answer?', options: ['Tell me about loops', 'In 2 sentences for a beginner, explain what a Python for loop does', 'loops?', 'loop info'], answer: 1 }
        ]
      }
    ];
```

- [ ] **Step 2: Commit**

```bash
git add index.html
git commit -m "feat: add Module 8 - Prompt Engineering"
```

---

### Task 9: Module 9 — Python + Files

**Files:**
- Modify: `kid-ai-tutorial/index.html` — add module m9

- [ ] **Step 1: Add m9 after m8**

Find the closing `];` after m8 and replace with:

```js
      },
      {
        id: 'm9',
        title: 'Python + Files',
        content: `
          <p>Real programs read and write files — logs, data, results. Let's learn how
          Python handles files, then feed file content to Claude.</p>

          <h3>Writing a file</h3>
          <div class="code-block">
            <pre>with open("notes.txt", "w") as f:
    f.write("Line one\\n")
    f.write("Line two\\n")</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>Reading a file</h3>
          <div class="code-block">
            <pre>with open("notes.txt", "r") as f:
    content = f.read()
    print(content)</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>Reading line by line</h3>
          <div class="code-block">
            <pre>with open("notes.txt", "r") as f:
    for line in f.readlines():
        print(line.strip())</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>Send each line to Claude</h3>
          <div class="code-block">
            <pre>import anthropic

client = anthropic.Anthropic(api_key="YOUR_API_KEY_HERE")

with open("notes.txt", "r") as f:
    for line in f.readlines():
        line = line.strip()
        if not line:
            continue
        message = client.messages.create(
            model="claude-haiku-4-5-20251001",
            max_tokens=64,
            messages=[{"role": "user", "content": f"Summarize in 5 words: {line}"}]
        )
        print(f"Original: {line}")
        print(f"Summary:  {message.content[0].text}")
        print()</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Mission 9 Challenge</div>
            <p>1. Create a file called <strong>facts.txt</strong> with 3 interesting facts (one per line).<br>
            2. Write a Python script that reads each line and asks Claude to give it a fun emoji.<br>
            3. Print the original fact + the emoji Claude chose.</p>
            <button class="hint-toggle" onclick="toggleHint(this)">show hint</button>
            <div class="hint-body">
              Create facts.txt with nano:<br>
              <code style="color:var(--green)">nano facts.txt</code><br><br>
              Your prompt to Claude could be:<br>
              <code style="color:var(--green)">f"Give exactly one emoji that matches this fact: {line}"</code>
            </div>
          </div>
        `,
        quiz: [
          { q: 'Which Python function opens a file?', options: ['read()', 'file()', 'open()', 'load()'], answer: 2 },
          { q: 'What does "with open(filename) as f:" do?', options: ['Creates a new file', 'Opens a file and automatically closes it when done', 'Deletes a file', 'Prints a file'], answer: 1 },
          { q: 'How do you read all lines of a file into a list?', options: ['f.read()', 'f.lines()', 'f.readlines()', 'f.all()'], answer: 2 }
        ]
      }
    ];
```

- [ ] **Step 2: Commit**

```bash
git add index.html
git commit -m "feat: add Module 9 - Python + Files"
```

---

### Task 10: Module 10 — Multi-turn Chat

**Files:**
- Modify: `kid-ai-tutorial/index.html` — add module m10

- [ ] **Step 1: Add m10 after m9**

Find the closing `];` after m9 and replace with:

```js
      },
      {
        id: 'm10',
        title: 'Multi-turn Chat',
        content: `
          <p>So far every script sends one message and gets one answer. Real chatbots
          remember what you said before. Let's build one.</p>

          <h3>How conversation history works</h3>
          <p>The Anthropic API doesn't automatically remember previous messages.
          You keep a list called <code style="color:var(--green)">messages</code> and
          add to it every turn:</p>
          <div class="code-block">
            <pre>messages = []

# Turn 1
messages.append({"role": "user", "content": "My name is Ada."})
# ... get response, add it too ...
messages.append({"role": "assistant", "content": "Nice to meet you, Ada!"})

# Turn 2 — Claude now knows your name
messages.append({"role": "user", "content": "What's my name?"})</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>A full chatbot loop</h3>
          <div class="code-block">
            <pre>import anthropic

client = anthropic.Anthropic(api_key="YOUR_API_KEY_HERE")
messages = []
MAX_HISTORY = 6  # keep last 3 exchanges (6 messages)

print("Chat with Claude. Type 'quit' to exit.")

while True:
    user_input = input("You: ")
    if user_input.lower() == "quit":
        break

    messages.append({"role": "user", "content": user_input})

    # Keep history to last MAX_HISTORY messages
    if len(messages) > MAX_HISTORY:
        messages = messages[-MAX_HISTORY:]

    response = client.messages.create(
        model="claude-haiku-4-5-20251001",
        max_tokens=256,
        messages=messages
    )
    reply = response.content[0].text
    messages.append({"role": "assistant", "content": reply})
    print(f"Claude: {reply}\\n")</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Mission 10 Challenge</div>
            <p>1. Copy the chatbot script above into a file called <strong>chatbot.py</strong>.<br>
            2. Run it and have a 5-message conversation with Claude.<br>
            3. In message 5, ask Claude something that requires it to remember message 1.<br>
            4. Does it remember?</p>
            <button class="hint-toggle" onclick="toggleHint(this)">show hint</button>
            <div class="hint-body">
              Try telling Claude your name in the first message, then asking
              "What's my name?" later. If the history is working, it will know.<br><br>
              If you get a token limit error, reduce MAX_HISTORY to 4.
            </div>
          </div>
        `,
        quiz: [
          { q: 'In the Anthropic API, what list stores the conversation history?', options: ['history', 'chat', 'messages', 'conversation'], answer: 2 },
          { q: 'What role does the AI\'s response get in the messages list?', options: ['ai', 'bot', 'assistant', 'claude'], answer: 2 },
          { q: 'Why limit conversation history length?', options: ['It\'s required by the API', 'To save API tokens and reduce cost', 'Because Claude forgets anyway', 'To make responses faster'], answer: 1 }
        ]
      }
    ];
```

- [ ] **Step 2: Commit**

```bash
git add index.html
git commit -m "feat: add Module 10 - Multi-turn Chat"
```

---

### Task 11: Update subtitle + push to GitHub Pages

**Files:**
- Modify: `kid-ai-tutorial/index.html` — update subtitle count

- [ ] **Step 1: Update subtitle from "6 missions" to "10 missions"**

Find:
```html
  <p class="subtitle">6 missions. Complete them in order. No skipping.</p>
```

Replace with:
```html
  <p class="subtitle">10 missions. Complete them in order. No skipping.</p>
```

- [ ] **Step 2: Full manual test checklist**

Open `index.html` in Firefox:
- [ ] 10 modules visible, modules 2-10 locked
- [ ] Module 1 shows quiz (Question 1 of 3)
- [ ] Wrong answer flashes red, correct advances
- [ ] All 3 correct → module 2 unlocks, scrolls into view
- [ ] Ask bar NOT visible (m4 not done)
- [ ] Complete modules 1-4 → ask bar appears with API key input
- [ ] Enter API key → bar switches to question mode
- [ ] Ask a question → response appears, button shows "thinking..."
- [ ] Ask 4 questions → only 3 responses shown
- [ ] Idea box shows after passing each module
- [ ] Idea box on m1-m3: shows "Complete Mission 4 first"
- [ ] Idea box on m4+: shows textarea, response appears
- [ ] Progress bar shows "4 of 10 modules complete"
- [ ] Page refresh → progress preserved

- [ ] **Step 3: Commit and push**

```bash
git add index.html
git commit -m "feat: update subtitle to 10 missions"
git push
```

Expected: GitHub Pages republishes automatically. Live at `https://derwin4o.github.io/kid-ai-tutorial/` within ~1 minute.

---

## Spec Coverage Checklist

| Requirement | Task |
|---|---|
| CSS for quiz, idea box, ask bar | Task 1 |
| callClaude utility | Task 2 |
| quizState variable | Task 2 |
| renderQuizQuestion + submitQuizAnswer | Task 3 |
| Mark-done replaced by quiz in renderModules | Task 3 |
| Quiz data for m1-m6 | Task 4 |
| renderIdeaBox + submitIdea | Task 5 |
| Idea box locked until m4 complete | Task 5 |
| Idea box shows "enter key" message when no key | Task 5 |
| Ask bar HTML | Task 6 |
| API key entry flow | Task 6 |
| Ask bar hidden until m4 complete | Task 6 |
| Max 3 responses in ask bar | Task 6 |
| Error state for bad API key | Task 6 |
| Module 7: GitHub | Task 7 |
| Module 8: Prompt Engineering | Task 8 |
| Module 9: Python + Files | Task 9 |
| Module 10: Multi-turn Chat | Task 10 |
| Subtitle updated to 10 missions | Task 11 |
| Pushed to GitHub Pages | Task 11 |
