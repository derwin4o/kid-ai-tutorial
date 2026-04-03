# Kid AI Tutorial Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Build a single `index.html` interactive tutorial that takes a 13-year-old from zero to making a real Claude API call across 5 locked modules.

**Architecture:** Pure HTML/CSS/JS single-file app. No dependencies, no server. Each module unlocks sequentially after the kid clicks "Mark as done". Progress persists in `localStorage`. Dark terminal theme throughout.

**Tech Stack:** HTML5, CSS3 (custom properties), vanilla JavaScript (ES6+), localStorage API, Clipboard API

---

## File Map

| File | Responsibility |
|------|---------------|
| `index.html` | Entire app — structure, styles, content, logic |
| `README.md` | One-line instructions |

---

### Task 1: Scaffold HTML + dark theme CSS

**Files:**
- Create: `kid-ai-tutorial/index.html`
- Create: `kid-ai-tutorial/README.md`

- [ ] **Step 1: Create README.md**

```markdown
# Kid AI Tutorial
Open `index.html` in Firefox to start.
```

- [ ] **Step 2: Create index.html with base structure and CSS**

```html
<!DOCTYPE html>
<html lang="en">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>AI Engineer Training</title>
  <style>
    :root {
      --bg: #0d0d0d;
      --bg2: #1a1a1a;
      --green: #00ff88;
      --green-dim: #00cc66;
      --grey: #444;
      --text: #cccccc;
      --locked: #2a2a2a;
      --font: 'Courier New', Courier, monospace;
    }
    * { box-sizing: border-box; margin: 0; padding: 0; }
    body {
      background: var(--bg);
      color: var(--text);
      font-family: var(--font);
      max-width: 800px;
      margin: 0 auto;
      padding: 2rem 1rem;
    }
    h1 {
      color: var(--green);
      font-size: 1.6rem;
      margin-bottom: 0.5rem;
    }
    .subtitle {
      color: var(--grey);
      font-size: 0.85rem;
      margin-bottom: 2rem;
    }
    /* Progress bar */
    .progress-wrap {
      margin-bottom: 2.5rem;
    }
    .progress-label {
      color: var(--green-dim);
      font-size: 0.8rem;
      margin-bottom: 0.4rem;
    }
    .progress-bar-bg {
      background: var(--bg2);
      border: 1px solid var(--grey);
      height: 12px;
      border-radius: 6px;
      overflow: hidden;
    }
    .progress-bar-fill {
      background: var(--green);
      height: 100%;
      border-radius: 6px;
      transition: width 0.4s ease;
      width: 0%;
    }
    /* Module cards */
    .module {
      background: var(--bg2);
      border: 1px solid var(--grey);
      border-radius: 8px;
      margin-bottom: 1.5rem;
      overflow: hidden;
      transition: border-color 0.3s;
    }
    .module.unlocked {
      border-color: var(--green-dim);
    }
    .module.locked {
      opacity: 0.45;
      pointer-events: none;
    }
    .module-header {
      display: flex;
      align-items: center;
      justify-content: space-between;
      padding: 1rem 1.2rem;
      cursor: pointer;
      user-select: none;
    }
    .module-title {
      color: var(--green);
      font-size: 1rem;
    }
    .module-status {
      font-size: 0.8rem;
      color: var(--grey);
    }
    .module.done .module-status { color: var(--green); }
    .module-body {
      padding: 0 1.2rem 1.2rem;
      display: none;
    }
    .module.open .module-body { display: block; }
    p { margin: 0.8rem 0; line-height: 1.6; font-size: 0.9rem; }
    h3 { color: var(--green-dim); margin: 1.2rem 0 0.5rem; font-size: 0.95rem; }
    /* Code blocks */
    .code-block {
      position: relative;
      background: #111;
      border: 1px solid #333;
      border-radius: 4px;
      margin: 0.8rem 0;
    }
    .code-block pre {
      padding: 0.8rem 1rem;
      overflow-x: auto;
      font-size: 0.85rem;
      color: var(--green);
      line-height: 1.5;
    }
    .copy-btn {
      position: absolute;
      top: 6px;
      right: 6px;
      background: var(--grey);
      color: #fff;
      border: none;
      border-radius: 3px;
      padding: 2px 8px;
      font-size: 0.7rem;
      cursor: pointer;
      font-family: var(--font);
    }
    .copy-btn:hover { background: var(--green-dim); color: #000; }
    /* Challenge section */
    .challenge {
      background: #111a11;
      border-left: 3px solid var(--green);
      border-radius: 0 4px 4px 0;
      padding: 0.8rem 1rem;
      margin: 1rem 0;
    }
    .challenge-title {
      color: var(--green);
      font-size: 0.8rem;
      text-transform: uppercase;
      letter-spacing: 0.1em;
      margin-bottom: 0.4rem;
    }
    /* Hint */
    .hint-toggle {
      background: none;
      border: 1px solid var(--grey);
      color: var(--grey);
      border-radius: 4px;
      padding: 3px 10px;
      font-size: 0.75rem;
      cursor: pointer;
      font-family: var(--font);
      margin-top: 0.6rem;
    }
    .hint-toggle:hover { border-color: var(--green-dim); color: var(--green-dim); }
    .hint-body {
      display: none;
      margin-top: 0.6rem;
      font-size: 0.85rem;
      color: #aaa;
      line-height: 1.6;
    }
    .hint-body.visible { display: block; }
    /* Mark done button */
    .mark-done-btn {
      display: block;
      width: 100%;
      margin-top: 1.2rem;
      padding: 0.7rem;
      background: none;
      border: 2px solid var(--green);
      color: var(--green);
      border-radius: 6px;
      font-size: 0.9rem;
      cursor: pointer;
      font-family: var(--font);
      transition: background 0.2s, color 0.2s;
    }
    .mark-done-btn:hover { background: var(--green); color: #000; }
    .mark-done-btn:disabled {
      border-color: var(--grey);
      color: var(--grey);
      cursor: default;
      background: none;
    }
    /* localStorage warning */
    .ls-warning {
      color: #ff8844;
      font-size: 0.75rem;
      padding: 0.5rem;
      border: 1px solid #ff8844;
      border-radius: 4px;
      margin-bottom: 1rem;
      display: none;
    }
  </style>
</head>
<body>
  <h1>&gt; AI Engineer Training</h1>
  <p class="subtitle">5 missions. Complete them in order. No skipping.</p>
  <div class="ls-warning" id="ls-warning">
    Warning: progress won't be saved in private/incognito mode.
  </div>
  <div class="progress-wrap">
    <div class="progress-label" id="progress-label">0 of 5 modules complete</div>
    <div class="progress-bar-bg">
      <div class="progress-bar-fill" id="progress-bar"></div>
    </div>
  </div>
  <div id="modules-container"></div>
  <script>
    // placeholder — logic added in Task 2
  </script>
</body>
</html>
```

- [ ] **Step 3: Open in Firefox and verify**

Open `kid-ai-tutorial/index.html` in Firefox.
Expected: Dark page with green title "> AI Engineer Training", subtitle, empty progress bar, no modules yet (container is empty).

- [ ] **Step 4: Commit**

```bash
cd kid-ai-tutorial
git init
git add index.html README.md
git commit -m "feat: scaffold HTML shell with dark terminal theme"
```

---

### Task 2: Module data + rendering engine

**Files:**
- Modify: `kid-ai-tutorial/index.html` — replace `<script>` placeholder with full JS engine

- [ ] **Step 1: Replace the script placeholder with the module engine**

Find the `<script>` tag containing `// placeholder — logic added in Task 2` and replace it entirely with:

```html
  <script>
    // ── Storage ──────────────────────────────────────────────────────
    let lsAvailable = true;
    try { localStorage.setItem('_test', '1'); localStorage.removeItem('_test'); }
    catch(e) { lsAvailable = false; document.getElementById('ls-warning').style.display = 'block'; }

    function getProgress() {
      if (!lsAvailable) return {};
      try { return JSON.parse(localStorage.getItem('tutorialProgress') || '{}'); }
      catch(e) { return {}; }
    }
    function saveProgress(moduleId) {
      if (!lsAvailable) return;
      const p = getProgress();
      p[moduleId] = true;
      localStorage.setItem('tutorialProgress', JSON.stringify(p));
    }

    // ── Module definitions (content added in Tasks 3-7) ──────────────
    const MODULES = []; // populated in later tasks

    // ── Render ───────────────────────────────────────────────────────
    function renderModules() {
      const progress = getProgress();
      const container = document.getElementById('modules-container');
      container.innerHTML = '';
      let completedCount = 0;

      MODULES.forEach((mod, i) => {
        const isDone = !!progress[mod.id];
        const prevDone = i === 0 || !!progress[MODULES[i-1].id];
        const isUnlocked = prevDone;
        if (isDone) completedCount++;

        const el = document.createElement('div');
        el.className = 'module' +
          (isDone ? ' done' : '') +
          (isUnlocked ? ' unlocked' : ' locked') +
          (i === 0 && !isDone ? ' open' : '') +
          (isDone && i === completedCount - 1 ? '' : '');

        // Auto-open the first unlocked incomplete module
        const isCurrentModule = isUnlocked && !isDone &&
          (i === 0 || !!progress[MODULES[i-1].id]);

        el.innerHTML = `
          <div class="module-header" onclick="toggleModule('mod-${mod.id}')">
            <span class="module-title">Mission ${i+1}: ${mod.title}</span>
            <span class="module-status">${isDone ? '✓ complete' : isUnlocked ? 'unlocked' : '🔒 locked'}</span>
          </div>
          <div class="module-body" id="mod-${mod.id}">
            ${mod.content}
            <button class="mark-done-btn" id="btn-${mod.id}"
              onclick="markDone('${mod.id}')"
              ${isDone ? 'disabled' : ''}>
              ${isDone ? '✓ Already completed' : '✓ Mark as done — unlock next mission'}
            </button>
          </div>
        `;

        if (isCurrentModule || isDone) {
          el.classList.add('open');
        }

        container.appendChild(el);
      });

      // Update progress bar
      const pct = MODULES.length ? (completedCount / MODULES.length) * 100 : 0;
      document.getElementById('progress-bar').style.width = pct + '%';
      document.getElementById('progress-label').textContent =
        completedCount + ' of ' + MODULES.length + ' modules complete';
    }

    function toggleModule(bodyId) {
      const body = document.getElementById(bodyId);
      const card = body.closest('.module');
      if (card.classList.contains('locked')) return;
      card.classList.toggle('open');
    }

    function markDone(moduleId) {
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

    function copyCode(btn) {
      const pre = btn.closest('.code-block').querySelector('pre');
      const text = pre.innerText;
      if (navigator.clipboard) {
        navigator.clipboard.writeText(text).then(() => {
          btn.textContent = 'copied!';
          setTimeout(() => btn.textContent = 'copy', 1500);
        });
      } else {
        // fallback: select text
        const range = document.createRange();
        range.selectNode(pre);
        window.getSelection().removeAllRanges();
        window.getSelection().addRange(range);
        btn.textContent = 'selected!';
        setTimeout(() => btn.textContent = 'copy', 1500);
      }
    }

    function toggleHint(btn) {
      const hint = btn.nextElementSibling;
      hint.classList.toggle('visible');
      btn.textContent = hint.classList.contains('visible') ? 'hide hint' : 'show hint';
    }

    // Initial render
    renderModules();
  </script>
```

- [ ] **Step 2: Verify in browser console**

Open `index.html` in Firefox. Open DevTools console (F12).
Run: `getProgress()`
Expected: `{}` (empty object, no errors)

Run: `MODULES`
Expected: `[]` (empty array — modules added in next tasks)

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add module render engine and localStorage progress tracking"
```

---

### Task 3: Module 1 — The Terminal

**Files:**
- Modify: `kid-ai-tutorial/index.html` — add Module 1 to the `MODULES` array

- [ ] **Step 1: Add Module 1 to the MODULES array**

Find `const MODULES = []; // populated in later tasks` and replace it with:

```javascript
    const MODULES = [
      {
        id: 'm1',
        title: 'The Terminal',
        content: `
          <p>The terminal is how you talk directly to your computer — no buttons, just text.
          Every AI engineer uses it every day. Let's learn the basics.</p>

          <h3>Open a terminal</h3>
          <p>On Ubuntu: press <strong style="color:var(--green)">Ctrl + Alt + T</strong>.
          A black window with a blinking cursor appears. That's your terminal.</p>

          <h3>Commands to learn</h3>
          <p><code style="color:var(--green)">pwd</code> — prints where you are (your current folder)</p>
          <div class="code-block">
            <pre>pwd</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>
          <p><code style="color:var(--green)">ls</code> — lists everything in the current folder</p>
          <div class="code-block">
            <pre>ls</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>
          <p><code style="color:var(--green)">mkdir</code> — makes a new folder</p>
          <div class="code-block">
            <pre>mkdir ai-lab</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>
          <p><code style="color:var(--green)">cd</code> — moves you into a folder</p>
          <div class="code-block">
            <pre>cd ai-lab</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>
          <p>Run your first Python line:</p>
          <div class="code-block">
            <pre>python3 -c "print('hello world')"</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Mission 1 Challenge</div>
            <p>1. Open a terminal.<br>
            2. Create a folder called <strong>ai-lab</strong>.<br>
            3. Go inside it with <code>cd ai-lab</code>.<br>
            4. Run <code>ls</code> — it should show nothing (empty folder).<br>
            5. Run <code>python3 -c "print('I am an AI engineer')"</code></p>
            <p>When you see your message printed, come back here and mark it done.</p>
            <button class="hint-toggle" onclick="toggleHint(this)">show hint</button>
            <div class="hint-body">
              Open terminal with Ctrl+Alt+T, then type each command and press Enter:<br><br>
              <code style="color:var(--green)">mkdir ai-lab</code><br>
              <code style="color:var(--green)">cd ai-lab</code><br>
              <code style="color:var(--green)">ls</code><br>
              <code style="color:var(--green)">python3 -c "print('I am an AI engineer')"</code>
            </div>
          </div>
        `
      },
    ]; // populated in later tasks
```

- [ ] **Step 2: Verify in browser**

Refresh `index.html`.
Expected:
- "Mission 1: The Terminal" card visible and open
- Content shows terminal commands with copy buttons
- Challenge section with hint toggle
- "Mark as done" button at bottom
- Progress bar shows "0 of 1 modules complete" (only 1 module exists so far)

- [ ] **Step 3: Test mark-done flow**

Click "Mark as done".
Expected: Button becomes disabled with "✓ Already completed". Progress bar fills to 100% (of 1 module).
Refresh page. Expected: Module 1 still shows as done.

- [ ] **Step 4: Test copy button**

Click "copy" on any code block.
Expected: Button text changes to "copied!" then back to "copy". Check clipboard has the code.

- [ ] **Step 5: Reset progress for clean testing**

Open DevTools console (F12), run:
```javascript
localStorage.removeItem('tutorialProgress'); location.reload();
```
Expected: Module 1 shows as incomplete again.

- [ ] **Step 6: Commit**

```bash
git add index.html
git commit -m "feat: add Module 1 - The Terminal"
```

---

### Task 4: Module 2 — Python in 30 min

**Files:**
- Modify: `kid-ai-tutorial/index.html` — add Module 2 to MODULES array

- [ ] **Step 1: Add Module 2 after Module 1 in the MODULES array**

Find `    ]; // populated in later tasks` and replace it with:

```javascript
      {
        id: 'm2',
        title: 'Python in 30 min',
        content: `
          <p>Python is the language of AI. It reads almost like English. Let's learn just
          enough to do real things.</p>

          <h3>Variables — storing information</h3>
          <div class="code-block">
            <pre>name = "Ada"
age = 13
print(name, "is", age, "years old")</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>Functions — reusable blocks of code</h3>
          <div class="code-block">
            <pre>def greet(name):
    print("Hello,", name)

greet("Ada")
greet("World")</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>Loops — doing things many times</h3>
          <div class="code-block">
            <pre>for i in range(5):
    print("Line", i)</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>Input — asking the user something</h3>
          <div class="code-block">
            <pre>name = input("What is your name? ")
print("Welcome,", name)</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>How to run a Python file</h3>
          <p>Save your code in a file called <code style="color:var(--green)">hello.py</code>,
          then run it:</p>
          <div class="code-block">
            <pre>python3 hello.py</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Mission 2 Challenge</div>
            <p>Inside your <strong>ai-lab</strong> folder, create a file called
            <strong>hello.py</strong> with this code:</p>
            <div class="code-block">
              <pre>name = input("What is your name? ")
print("Hello,", name + "! You are training to be an AI engineer.")</pre>
              <button class="copy-btn" onclick="copyCode(this)">copy</button>
            </div>
            <p>Run it with <code>python3 hello.py</code>. Enter your name when it asks.
            See your message? Come back and mark it done.</p>
            <button class="hint-toggle" onclick="toggleHint(this)">show hint</button>
            <div class="hint-body">
              To create the file, open a text editor:<br><br>
              <code style="color:var(--green)">cd ai-lab</code><br>
              <code style="color:var(--green)">nano hello.py</code><br><br>
              Type or paste the code. Save with Ctrl+O, Enter, then exit with Ctrl+X.<br>
              Then run: <code style="color:var(--green)">python3 hello.py</code>
            </div>
          </div>
        `
      },
    ]; // populated in later tasks
```

- [ ] **Step 2: Verify in browser**

Reset progress: in console run `localStorage.removeItem('tutorialProgress'); location.reload();`

Expected: Module 1 open and unlocked. Module 2 appears locked (greyed out, padlock icon).

Click "Mark as done" on Module 1. Expected: Module 2 unlocks and scrolls into view.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Module 2 - Python in 30 min"
```

---

### Task 5: Module 3 — What is AI?

**Files:**
- Modify: `kid-ai-tutorial/index.html` — add Module 3 to MODULES array

- [ ] **Step 1: Add Module 3 after Module 2**

Find `    ]; // populated in later tasks` and replace it with:

```javascript
      {
        id: 'm3',
        title: 'What is AI?',
        content: `
          <p>Before we talk to AI with code, let's understand what AI actually is.
          Spoiler: it's not magic, and it's not a robot brain.</p>

          <h3>The autocomplete analogy</h3>
          <p>You know how your phone suggests the next word when you type a message?
          AI language models (like me — Claude) are like that, but trained on almost
          every book, article, and website ever written. They get very good at predicting
          what a helpful, smart answer looks like.</p>

          <h3>What is a prompt?</h3>
          <p>A <strong style="color:var(--green)">prompt</strong> is the message you send to an AI.
          The AI reads your prompt and generates a response. The better your prompt,
          the better the response. This skill is called <em>prompt engineering</em>.</p>

          <h3>What is an API?</h3>
          <p>An <strong style="color:var(--green)">API</strong> (Application Programming Interface)
          is a way for programs to talk to each other. Instead of opening a chat window,
          you send a message from your Python code and get the AI's response back as text
          your program can use.</p>

          <h3>What can you build with it?</h3>
          <p>Chatbots, writing assistants, code reviewers, data analyzers, game characters
          that can talk — anything that needs language understanding. That's what you're
          going to help dad build.</p>

          <div class="challenge">
            <div class="challenge-title">⚡ Mission 3 Challenge</div>
            <p>Open a browser and go to <strong>claude.ai</strong> (or ask dad which AI chat to use).
            Type this prompt:</p>
            <div class="code-block">
              <pre>Explain what a Python function is in 2 sentences, for a 13-year-old.</pre>
              <button class="copy-btn" onclick="copyCode(this)">copy</button>
            </div>
            <p>Read the answer. Does it make sense? When you're done, come back and mark it done.</p>
            <button class="hint-toggle" onclick="toggleHint(this)">show hint</button>
            <div class="hint-body">
              Just open any web browser (Firefox), go to claude.ai or chat.openai.com,
              and type the prompt. No account needed for basic use on some platforms.
              Ask dad if you're not sure which one to use.
            </div>
          </div>
        `
      },
    ]; // populated in later tasks
```

- [ ] **Step 2: Verify in browser**

Refresh. Complete modules 1 and 2 by clicking their "Mark as done" buttons. Module 3 should unlock.
Expected: Module 3 content visible with no code errors in console.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Module 3 - What is AI?"
```

---

### Task 6: Module 4 — Your first API call

**Files:**
- Modify: `kid-ai-tutorial/index.html` — add Module 4 to MODULES array

- [ ] **Step 1: Add Module 4 after Module 3**

Find `    ]; // populated in later tasks` and replace it with:

```javascript
      {
        id: 'm4',
        title: 'Your first API call',
        content: `
          <p>This is the big one. You're going to write a Python script that talks to
          Claude directly — no browser, just code.</p>

          <h3>Step 1: Get an API key</h3>
          <p>Ask dad for an Anthropic API key. It looks like:
          <code style="color:var(--green)">sk-ant-...</code>
          Keep it secret — never share it or put it on the internet.</p>

          <h3>Step 2: Install the library</h3>
          <p>In your terminal:</p>
          <div class="code-block">
            <pre>pip3 install anthropic</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>Step 3: Write the script</h3>
          <p>Create a file called <code style="color:var(--green)">ask_claude.py</code>
          in your ai-lab folder:</p>
          <div class="code-block">
            <pre>import anthropic

client = anthropic.Anthropic(api_key="YOUR_API_KEY_HERE")

message = client.messages.create(
    model="claude-haiku-4-5-20251001",
    max_tokens=256,
    messages=[
        {"role": "user", "content": "Tell me one cool fact about computers."}
    ]
)

print(message.content[0].text)</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <p>Replace <code style="color:var(--green)">YOUR_API_KEY_HERE</code> with the
          key dad gave you. Then run it:</p>
          <div class="code-block">
            <pre>python3 ask_claude.py</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>Step 4: Customize the prompt</h3>
          <p>Change the message text to anything you want to ask. Try:
          <em>"Explain loops in Python in one sentence."</em></p>

          <div class="challenge">
            <div class="challenge-title">⚡ Mission 4 Challenge</div>
            <p>1. Get the API key from dad.<br>
            2. Install the library with pip3.<br>
            3. Create ask_claude.py and run it — you should see Claude's answer in the terminal.<br>
            4. Change the prompt to your own question and run it again.</p>
            <p>When Claude answers your custom question, come back and mark it done.</p>
            <button class="hint-toggle" onclick="toggleHint(this)">show hint</button>
            <div class="hint-body">
              If you get an error like <em>ModuleNotFoundError: No module named 'anthropic'</em>,
              run <code style="color:var(--green)">pip3 install anthropic</code> again and make sure
              it says "Successfully installed".<br><br>
              If you get an <em>AuthenticationError</em>, double-check the API key — make sure you
              copied the whole thing including the <code>sk-ant-</code> part.
            </div>
          </div>
        `
      },
    ]; // populated in later tasks
```

- [ ] **Step 2: Verify in browser**

Refresh. Mark modules 1–3 done. Module 4 should unlock.
Check: all code blocks have working copy buttons. Console shows no JS errors.

- [ ] **Step 3: Commit**

```bash
git add index.html
git commit -m "feat: add Module 4 - first API call"
```

---

### Task 7: Module 5 — Build something real

**Files:**
- Modify: `kid-ai-tutorial/index.html` — add Module 5, remove `// populated in later tasks` comment

- [ ] **Step 1: Add Module 5 and finalize the MODULES array**

Find `    ]; // populated in later tasks` (the last one) and replace it with:

```javascript
      {
        id: 'm5',
        title: 'Build something real',
        content: `
          <p>You've got all the pieces. Now you're going to build something that actually
          helps dad's AI project.</p>

          <h3>What you've learned</h3>
          <p>✓ Navigate the terminal<br>
          ✓ Write Python (variables, functions, loops, input)<br>
          ✓ Understand what AI and APIs are<br>
          ✓ Call Claude from Python code</p>

          <h3>Your mission</h3>
          <p>Dad will give you a specific task for the project. It might be:</p>
          <ul style="margin: 0.5rem 0 0.5rem 1.5rem; line-height: 2;">
            <li>Writing a script that summarizes text with Claude</li>
            <li>Building a small tool that answers questions about a topic</li>
            <li>Automating something repetitive using AI</li>
          </ul>
          <p>Use your <code style="color:var(--green)">ask_claude.py</code> as a starting point.
          Modify the prompt, add an <code>input()</code> so users can type questions,
          or loop through a list of items.</p>

          <h3>Template to start from</h3>
          <div class="code-block">
            <pre>import anthropic

client = anthropic.Anthropic(api_key="YOUR_API_KEY_HERE")

def ask(prompt):
    message = client.messages.create(
        model="claude-haiku-4-5-20251001",
        max_tokens=512,
        messages=[{"role": "user", "content": prompt}]
    )
    return message.content[0].text

# Your code here
user_input = input("Ask something: ")
response = ask(user_input)
print("Claude says:", response)</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Mission 5 Challenge</div>
            <p>Build the tool dad describes. Make it run without errors.
            Show dad it works. Come back and mark it done.</p>
            <button class="hint-toggle" onclick="toggleHint(this)">show hint</button>
            <div class="hint-body">
              Start simple. Get it working with one hard-coded prompt first,
              then add <code>input()</code> to make it interactive.
              If you're stuck, paste your error message into Claude and ask it
              to explain what's wrong.
            </div>
          </div>
        `
      },
      {
        id: 'm6',
        title: 'Save your work with Git',
        content: `
          <p>Every engineer saves their work with Git. It's like a time machine for your code —
          you can always go back to any version you saved. Let's set it up.</p>

          <h3>What is Git?</h3>
          <p>Git tracks changes to your files. Every time you save a "commit", Git
          remembers exactly what your code looked like at that moment. If you break
          something, you can go back.</p>

          <h3>Set up Git (first time only)</h3>
          <div class="code-block">
            <pre>git config --global user.name "Your Name"
git config --global user.email "your@email.com"</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>Initialize a repo in your project</h3>
          <div class="code-block">
            <pre>cd ~/ai-lab
git init</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <h3>Save your work</h3>
          <p>Tell Git which files to include:</p>
          <div class="code-block">
            <pre>git add .</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>
          <p>Save a snapshot with a message describing what you did:</p>
          <div class="code-block">
            <pre>git commit -m "my first AI tool"</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>
          <p>See your history:</p>
          <div class="code-block">
            <pre>git log --oneline</pre>
            <button class="copy-btn" onclick="copyCode(this)">copy</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Mission 6 Challenge</div>
            <p>1. Go into your <strong>ai-lab</strong> folder in the terminal.<br>
            2. Run <code>git init</code>.<br>
            3. Run <code>git add .</code> then <code>git commit -m "my first AI tool"</code>.<br>
            4. Run <code>git log --oneline</code> — you should see your commit.</p>
            <p>When you see your commit in the log, you're a real engineer. Mark it done.</p>
            <button class="hint-toggle" onclick="toggleHint(this)">show hint</button>
            <div class="hint-body">
              If git is not installed: <code style="color:var(--green)">sudo apt install git</code><br><br>
              If git complains about your name/email, run the config commands at the top first,
              replacing "Your Name" and "your@email.com" with your actual details.<br><br>
              <code style="color:var(--green)">git config --global user.name "Ada"</code><br>
              <code style="color:var(--green)">git config --global user.email "ada@example.com"</code>
            </div>
          </div>
        `
      }
    ];
```

- [ ] **Step 2: Full end-to-end test in browser**

Refresh `index.html`.
- [ ] All 6 modules visible
- [ ] Module 1 open and unlocked
- [ ] Modules 2–6 locked
- [ ] Mark Module 1 done → Module 2 unlocks, scrolls into view
- [ ] Mark Module 2 done → Module 3 unlocks
- [ ] Mark all 6 done → progress bar fills to 100%, shows "6 of 6 modules complete"
- [ ] Refresh page → all 6 still show as complete
- [ ] Reset in console: `localStorage.removeItem('tutorialProgress'); location.reload();` → back to 0

- [ ] **Step 3: Test localStorage unavailable warning**

In Firefox: open a private window, open `index.html`.
Expected: Orange warning banner appears at top: "Warning: progress won't be saved in private/incognito mode."

- [ ] **Step 4: Commit**

```bash
git add index.html
git commit -m "feat: add Module 5 and Module 6 - build real tool + git"
```

---

## Checklist — Spec Coverage

| Spec requirement | Task |
|---|---|
| Single index.html, no dependencies | Task 1 |
| Dark terminal theme, green text | Task 1 |
| Progress bar | Task 2 |
| localStorage persistence | Task 2 |
| Module lock/unlock flow | Task 2 |
| Copy buttons | Task 2 |
| Hint toggles | Tasks 3–7 |
| Mark as done button | Task 2 |
| Module 1: Terminal | Task 3 |
| Module 2: Python | Task 4 |
| Module 3: What is AI? | Task 5 |
| Module 4: API call | Task 6 |
| Module 5: Build real tool | Task 7 |
| Module 6: Git | Task 7 |
| localStorage unavailable warning | Task 7 step 3 |
| Works offline (no CDN) | Task 1 — no external URLs |
| README | Task 1 |
