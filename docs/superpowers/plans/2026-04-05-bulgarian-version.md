# Bulgarian Version (index-bg.html) Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Create `index-bg.html` — a full Bulgarian translation of `index.html` for a 13-year-old beginner, with English words in brackets throughout as a bilingual dictionary.

**Architecture:** Copy `index.html` to `index-bg.html`. Translate all user-facing text (page chrome, JS UI strings, all 10 mission contents + quizzes). Code examples stay in English. Technical terms and common action/noun words appear in Bulgarian with English in brackets. No changes to CSS or JS engine logic.

**Tech Stack:** Pure HTML/CSS/JS — single file, no build tools.

---

### Task 1: Create base file with chrome + JS strings translated

**Files:**
- Create: `index-bg.html`

- [ ] **Step 1: Copy the file**

```bash
cp /Users/derwin4o/kid-ai-tutorial/index.html /Users/derwin4o/kid-ai-tutorial/index-bg.html
```

- [ ] **Step 2: Translate HTML head and body chrome**

In `index-bg.html`, make these replacements:

| Find | Replace with |
|---|---|
| `<html lang="en">` | `<html lang="bg">` |
| `<title>AI Engineer Training</title>` | `<title>Обучение за AI инженер</title>` |
| `&gt; AI Engineer Training` | `&gt; Обучение за AI инженер` |
| `10 missions. Complete them in order. No skipping.` | `10 мисии [missions]. Изпълнявай [complete] ги по ред [in order]. Без прескачане [no skipping].` |
| `Warning: progress won't be saved in private/incognito mode.` | `Внимание [warning]: прогресът [progress] няма да се запази [save] в режим инкогнито [private/incognito].` |
| `0 of 0 modules complete` | `0 от 0 мисии завършени` |
| `aria-label="Tutorial progress"` | `aria-label="Прогрес на обучението"` |
| `aria-label="Ask Claude anything"` | `aria-label="Попитай [ask] Claude нещо"` |
| `Ask Claude anything about what you learned →` (in HTML) | `Попитай [ask] Claude нещо за наученото [what you learned] →` |
| `placeholder="Type a question..."` | `placeholder="Напиши [type] въпрос [question]..."` |
| `Ask →` (ask-bar-btn HTML) | `Питай [ask] →` |

- [ ] **Step 3: Translate JS UI strings**

In `index-bg.html`, replace these JS string values (use Find & Replace in your editor):

| Find | Replace |
|---|---|
| `'✓ complete'` | `'✓ завършено [complete]'` |
| `'unlocked'` | `'отключено [unlocked]'` |
| `'🔒 locked'` | `'🔒 заключено [locked]'` |
| `'✓ Quiz passed'` | `'✓ Тестът [quiz] е минат [passed]'` |
| `'✓ Complete'` (in renderQuizQuestion) | `'✓ Завършено [complete]'` |
| `` `Question ${currentQ + 1} of ${quiz.length}` `` | `` `Въпрос [question] ${currentQ + 1} от [of] ${quiz.length}` `` |
| `Complete Mission 4 first to unlock Claude chat.` | `Завърши [complete] Мисия [mission] 4 първо, за да отключиш [unlock] Claude чат [chat].` |
| `Enter your API key in the bar at the bottom of the page.` | `Въведи [enter] API ключа [key] си в лентата [bar] долу на страницата [page].` |
| `💡 What would YOU add to this mission? Tell Claude your idea.` | `💡 Какво би добавил [add] ТИ към тази мисия [mission]? Кажи идеята [idea] си на Claude.` |
| `placeholder="e.g. I'd add a module about making a website with AI..."` | `placeholder="напр. Бих добавил [add] мисия [mission] за правене на уебсайт [website] с AI..."` |
| `Send idea →` | `Изпрати [send] идея [idea] →` |
| `'Sending...'` | `'Изпращане [sending]...'` |
| `'Enter your API key in the bar below first.'` | `'Въведи [enter] API ключа [key] си в лентата [bar] долу първо [first].'` |
| `'Slow down — you\'re sending too many requests. Wait a moment and try again.'` | `'Забави [slow down] — изпращаш [sending] твърде много [too many] заявки [requests]. Изчакай [wait] малко и опитай [try] отново [again].'` |
| `'Something went wrong — check your API key.'` (both occurrences) | `'Нещо се обърка [went wrong] — провери [check] API ключа [key] си.'` |
| `completedCount + ' of ' + MODULES.length + ' modules complete'` | `completedCount + ' от [of] ' + MODULES.length + ' мисии [missions] завършени [complete]'` |
| `'Enter your Anthropic API key to unlock Claude chat:'` | `'Въведи [enter] Anthropic API ключа [key] си, за да отключиш [unlock] Claude чат [chat]:'` |
| `'Ask Claude anything about what you learned →'` | `'Попитай [ask] Claude нещо за наученото [what you learned] →'` |
| `'Save →'` | `'Запази [save] →'` |
| `'Type a question...'` (in updateAskBarVisibility JS) | `'Напиши [type] въпрос [question]...'` |
| `'Ask →'` (in updateAskBarVisibility JS) | `'Питай [ask] →'` |
| `'thinking...'` | `'мисля [thinking]...'` |
| `'You asked: '` | `'Ти попита [you asked]: '` |
| `btn.textContent = 'copied!'` | `btn.textContent = 'копирано [copied]!'` |
| `btn.textContent = 'selected!'` (both occurrences) | `btn.textContent = 'избрано [selected]!'` |
| `btn.textContent = 'copy'` (both occurrences in copyCode) | `btn.textContent = 'копирай [copy]'` |
| `btn.textContent = hint.classList.contains('visible') ? 'hide hint' : 'show hint'` | `btn.textContent = hint.classList.contains('visible') ? 'скрий [hide] подсказка [hint]' : 'покажи [show] подсказка [hint]'` |

Also translate the two Claude system prompts so Claude responds in Bulgarian:

Find:
```javascript
'You are encouraging a 13-year-old learning to code. They just finished a programming lesson and shared an idea. Respond warmly in 2 sentences max.',
```
Replace with:
```javascript
'You are encouraging a 13-year-old learning to code. They just finished a programming lesson and shared an idea. Respond warmly in 2 sentences max. Always respond in Bulgarian.',
```

Find:
```javascript
'You are helping a 13-year-old learn programming and AI. Answer clearly in plain language, maximum 3 sentences.',
```
Replace with:
```javascript
'You are helping a 13-year-old learn programming and AI. Answer clearly in plain language, maximum 3 sentences. Always respond in Bulgarian.',
```

- [ ] **Step 4: Set MODULES to empty array temporarily**

Find the line:
```javascript
const MODULES = [
```
...and replace the entire MODULES array definition (from `const MODULES = [` to the closing `];` at line ~1108) with:
```javascript
const MODULES = [];
```

- [ ] **Step 5: Verify base file loads**

Open `index-bg.html` in a browser. You should see:
- Title: "Обучение за AI инженер"
- Subtitle with Bulgarian text
- Progress label: "0 от 0 мисии завършени"
- No module cards (empty MODULES)

- [ ] **Step 6: Commit**

```bash
cd /Users/derwin4o/kid-ai-tutorial
git add index-bg.html
git commit -m "feat: add index-bg.html base with Bulgarian chrome and JS strings"
```

---

### Task 2: Translate Missions 1–3 (Terminal, Python, What is AI)

**Files:**
- Modify: `index-bg.html` (add missions m1, m2, m3 to MODULES array)

- [ ] **Step 1: Replace the empty MODULES array with missions 1–3**

Find `const MODULES = [];` and replace with:

```javascript
const MODULES = [
      {
        id: 'm1',
        title: 'The Terminal',
        content: `
          <p>Terminal [терминал] е начинът [the way], по който говориш [talk] директно [directly] с компютъра [computer] — без бутони [buttons], само с текст [text].
          Всеки [every] AI инженер [engineer] го използва [uses] всеки ден [every day]. Нека научим [learn] основите [the basics].</p>

          <h3>Отвори [open] terminal</h3>
          <p>В Ubuntu: натисни [press] <strong style="color:var(--green)">Ctrl + Alt + T</strong>.
          Появява се [appears] черен прозорец [black window] с мигащ курсор [blinking cursor]. Това е твоят terminal.</p>

          <h3>Команди [commands] за учене [to learn]</h3>
          <p><code style="color:var(--green)">pwd</code> — показва [shows] текущата ти папка [current folder]</p>
          <div class="code-block">
            <pre>pwd</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>
          <p><code style="color:var(--green)">ls</code> — показва [lists] всичко [everything] в текущата папка [current folder]</p>
          <div class="code-block">
            <pre>ls</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>
          <p><code style="color:var(--green)">mkdir</code> — прави [makes] нова папка [new folder]</p>
          <div class="code-block">
            <pre>mkdir ai-lab</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>
          <p><code style="color:var(--green)">cd</code> — влиза [moves you] в папка [folder]</p>
          <div class="code-block">
            <pre>cd ai-lab</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>
          <p>Стартирай [run] първия си Python ред [line]:</p>
          <div class="code-block">
            <pre>python3 -c "print('hello world')"</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Мисия 1 Предизвикателство [Challenge]</div>
            <p>1. Отвори [open] terminal.<br>
            2. Създай [create] папка [folder] с ime <strong>ai-lab</strong>.<br>
            3. Влез [go] в нея с <code>cd ai-lab</code>.<br>
            4. Изпълни [run] <code>ls</code> — трябва да е празно [empty] (празна папка [empty folder]).<br>
            5. Изпълни [run] <code>python3 -c "print('I am an AI engineer')"</code></p>
            <p>Когато видиш [see] съобщението [message] отпечатано [printed], върни се [come back] тук и го отбележи [mark it].</p>
            <button class="mark-done-btn" onclick="markDone('m1')">Отбележи като готово [mark done] ✓</button>
            <button class="hint-toggle" onclick="toggleHint(this)">покажи [show] подсказка [hint]</button>
            <div class="hint-body">
              Отвори [open] terminal с Ctrl+Alt+T, после напиши [type] всяка команда [command] и натисни [press] Enter:<br><br>
              <code style="color:var(--green)">mkdir ai-lab</code><br>
              <code style="color:var(--green)">cd ai-lab</code><br>
              <code style="color:var(--green)">ls</code><br>
              <code style="color:var(--green)">python3 -c "print('I am an AI engineer')"</code>
            </div>
          </div>
        `,
        quiz: [
          { q: 'Коя команда [command] показва [shows] текущата ти папка [current folder]?', options: ['ls', 'pwd', 'cd', 'mkdir'], answer: 1 },
          { q: 'Коя команда [command] създава [creates] нова папка [new folder] ai-lab?', options: ['pwd ai-lab', 'cd ai-lab', 'mkdir ai-lab', 'touch ai-lab'], answer: 2 },
          { q: 'Как стартираш [run] Python скрипт [script] hello.py?', options: ['run hello.py', 'python3 hello.py', 'start hello.py', 'execute hello.py'], answer: 1 }
        ]
      },
      {
        id: 'm2',
        title: 'Python in 30 min',
        content: `
          <p>Python е езикът [the language] на AI. Чете се [reads] почти [almost] като английски [English]. Нека научим [learn] само
          толкова, колкото ни трябва [just enough] за истински неща [real things].</p>

          <h3>Променливи [variables] — запазване [storing] на информация [information]</h3>
          <div class="code-block">
            <pre>name = "Ada"
age = 13
print(name, "is", age, "years old")</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Функции [functions] — блокове код [code blocks] за многократно използване [reuse]</h3>
          <div class="code-block">
            <pre>def greet(name):
    print("Hello,", name)

greet("Ada")
greet("World")</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Цикли [loops] — правене [doing] на нещо много пъти [many times]</h3>
          <div class="code-block">
            <pre>for i in range(5):
    print("Line", i)</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Input [вход] — питане [asking] на потребителя [user] за нещо</h3>
          <div class="code-block">
            <pre>name = input("What is your name? ")
print("Welcome,", name)</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Как да стартираш [run] Python файл [file]</h3>
          <p>Запази [save] кода [code] в файл [file] с имe <code style="color:var(--green)">hello.py</code>,
          после стартирай [run] го:</p>
          <div class="code-block">
            <pre>python3 hello.py</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Мисия 2 Предизвикателство [Challenge]</div>
            <p>В папката [folder] <strong>ai-lab</strong>, създай [create] файл [file] с имe
            <strong>hello.py</strong> с този код [code]:</p>
            <div class="code-block">
              <pre>name = input("What is your name? ")
print("Hello,", name + "! You are training to be an AI engineer.")</pre>
              <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
            </div>
            <p>Стартирай [run] го с <code>python3 hello.py</code>. Въведи [enter] името [name] си когато поиска [asks].
            Видя ли съобщението [message]? Върни се [come back] и го отбележи [mark it].</p>
            <button class="mark-done-btn" onclick="markDone('m2')">Отбележи като готово [mark done] ✓</button>
            <button class="hint-toggle" onclick="toggleHint(this)">покажи [show] подсказка [hint]</button>
            <div class="hint-body">
              За да създадеш [create] файла [file], отвори [open] текстов редактор [text editor]:<br><br>
              <code style="color:var(--green)">cd ai-lab</code><br>
              <code style="color:var(--green)">nano hello.py</code><br><br>
              Напиши [type] или постни [paste] кода [code]. Запази [save] с Ctrl+O, Enter, после излез [exit] с Ctrl+X.<br>
              После стартирай [run]: <code style="color:var(--green)">python3 hello.py</code>
            </div>
          </div>
        `,
        quiz: [
          { q: 'Коя ключова дума [keyword] дефинира [defines] функция [function] в Python?', options: ['func', 'define', 'def', 'function'], answer: 2 },
          { q: 'Какво прави [does] input()?', options: ['Отпечатва [prints] текст [text] на екрана [screen]', 'Пита [asks] потребителя [user] да напише [type] нещо', 'Създава [creates] променлива [variable]', 'Цикли [loops] завинаги [forever]'], answer: 1 },
          { q: 'Какъв е изходът [output] от: print(2 + 3)?', options: ['23', '2+3', '5', 'Error'], answer: 2 }
        ]
      },
      {
        id: 'm3',
        title: 'What is AI?',
        content: `
          <p>Преди да говорим [talk] с AI с код [code], нека разберем [understand] какво всъщност [actually] е AI.
          Спойлер [spoiler]: не е магия [magic] и не е роботски мозък [robot brain].</p>

          <h3>Аналогията [analogy] с автодовършването [autocomplete]</h3>
          <p>Знаеш ли как телефонът [phone] ти предлага [suggests] следващата дума [next word] когато пишеш [type] съобщение [message]?
          AI езиковите модели [language models] (като Claude или ChatGPT) са като това, но обучени [trained] на почти [almost]
          всяка книга [book], статия [article] и уебсайт [website] писани [written] някога. Те стават [become] много добри [very good] в предсказването [predicting]
          как изглежда [looks like] полезен [helpful], умен отговор [smart answer].</p>

          <h3>Какво е prompt [запитване]?</h3>
          <p>Един <strong style="color:var(--green)">prompt</strong> е съобщението [message], което изпращаш [send] на AI.
          AI чете [reads] твоя prompt и генерира [generates] отговор [response]. Колкото по-добър [better] е prompt-ът ти,
          толкова по-добър [better] е отговорът [response]. Това умение [skill] се казва [is called] <em>prompt engineering</em>.</p>

          <h3>Какво е API?</h3>
          <p>Един <strong style="color:var(--green)">API</strong> (Application Programming Interface)
          е начин [a way] програмите [programs] да говорят [talk] помежду си [to each other]. Вместо да отваряш [open] прозорец [window] за чат [chat],
          изпращаш [send] съобщение [message] от твоя Python код [code] и получаваш [receive] отговора [response] на AI обратно [back] като текст [text],
          който програмата [program] ти може да използва [use].</p>

          <h3>Какво можеш да строиш [build] с него?</h3>
          <p>Чат-ботове [chatbots], писателски асистенти [writing assistants], ревизори [reviewers] на код [code], анализатори [analyzers] на данни [data], игрови герои [game characters]
          — всичко [anything], което се нуждае [needs] от разбиране на език [language understanding]. Точно това [exactly this] ще помагаш [help] на татко да строи [build].</p>

          <div class="challenge">
            <div class="challenge-title">⚡ Мисия 3 Предизвикателство [Challenge]</div>
            <p>Отвори [open] браузър [browser] и отиди [go] на <strong>claude.ai</strong> (или попитай [ask] татко кой AI чат [chat] да използваш [use]).
            Напиши [type] този prompt:</p>
            <div class="code-block">
              <pre>Explain what a Python function is in 2 sentences, for a 13-year-old.</pre>
              <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
            </div>
            <p>Прочети [read] отговора [answer]. Разбра ли го [did you understand it]? Когато си готов [ready], върни се [come back] и го отбележи [mark it].</p>
            <button class="mark-done-btn" onclick="markDone('m3')">Отбележи като готово [mark done] ✓</button>
            <button class="hint-toggle" onclick="toggleHint(this)">покажи [show] подсказка [hint]</button>
            <div class="hint-body">
              Отвори [open] Firefox, отиди [go] на claude.ai или chat.openai.com.
              И двата изискват [require] акаунт [account] — помоли [ask] татко да влезе [log in], или създайте [create] акаунт [account] заедно [together].
              Попитай [ask] татко ако не си сигурен [sure] кой да използваш [use].
            </div>
          </div>
        `,
        quiz: [
          { q: 'Какво е prompt?', options: ['Вид [type of] Python грешка [error]', 'Съобщението [message], което изпращаш [send] на AI', 'Компютърна програма [computer program]', 'Заявка [query] към база данни [database]'], answer: 1 },
          { q: 'Какво означава [stands for] API?', options: ['Artificial Programming Intelligence', 'Application Programming Interface', 'Automated Process Integration', 'Advanced Python Interface'], answer: 1 },
          { q: 'Какво предсказват [predict] LLM-ите?', options: ['Времето [weather]', 'Цени [prices] на акции [stocks]', 'Най-полезната [most helpful] следваща дума [next word] или изречение [sentence]', 'Паролата [password] ти'], answer: 2 }
        ]
      }
    ];
```

- [ ] **Step 2: Verify in browser**

Open `index-bg.html`. You should see 3 mission cards. Mission 1 should be open and unlocked. Click through the quiz in Mission 1 — Bulgarian questions should appear.

- [ ] **Step 3: Commit**

```bash
cd /Users/derwin4o/kid-ai-tutorial
git add index-bg.html
git commit -m "feat(bg): add missions 1-3 (Terminal, Python, What is AI)"
```

---

### Task 3: Translate Missions 4–5 (First API Call, Build Something Real)

**Files:**
- Modify: `index-bg.html` (add m4, m5 to MODULES array)

- [ ] **Step 1: Add missions 4 and 5 after the closing `}` of m3**

Find the line ` ];` (the closing of the MODULES array after m3) and replace it with:

```javascript
      ,{
        id: 'm4',
        title: 'Your first API call',
        content: `
          <p>Това е голямото [this is the big one]. Ще напишеш [write] Python скрипт [script], който говори [talks] директно [directly] с
          Claude — без браузър [browser], само код [code].</p>

          <h3>Стъпка [step] 1: Вземи [get] API ключ [key]</h3>
          <p>Помоли [ask] татко за Anthropic API ключ [key]. Изглежда [looks] така:
          <code style="color:var(--green)">sk-ant-...</code>
          Пази го в тайна [keep it secret] — никога [never] не го споделяй [share] или слагай [put] в интернет [internet].</p>

          <h3>Стъпка [step] 2: Инсталирай [install] библиотеката [library]</h3>
          <p>В terminal-а:</p>
          <div class="code-block">
            <pre>pip3 install anthropic</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Стъпка [step] 3: Напиши [write] скрипта [script]</h3>
          <p>Създай [create] файл [file] с имe <code style="color:var(--green)">ask_claude.py</code>
          в папката [folder] ai-lab:</p>
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
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <p>Замени [replace] <code style="color:var(--green)">YOUR_API_KEY_HERE</code> с
          ключа [key], който татко ти даде [gave you]. После стартирай [run]:</p>
          <div class="code-block">
            <pre>python3 ask_claude.py</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Стъпка [step] 4: Персонализирай [customize] prompt-а</h3>
          <p>Промени [change] текста на съобщението [message text] на каквото [anything] искаш да питаш [ask]. Опитай [try]:
          <em>"Explain loops in Python in one sentence."</em></p>

          <div class="challenge">
            <div class="challenge-title">⚡ Мисия 4 Предизвикателство [Challenge]</div>
            <p>1. Вземи [get] API ключа [key] от татко.<br>
            2. Инсталирай [install] библиотеката [library] с pip3.<br>
            3. Създай [create] ask_claude.py и го стартирай [run] — трябва да видиш [see] отговора [answer] на Claude в terminal-а.<br>
            4. Промени [change] prompt-а с твой въпрос [question] и го стартирай [run] отново [again].</p>
            <p>Когато Claude отговори [answers] на твоя въпрос [question], върни се [come back] и го отбележи [mark it].</p>
            <button class="mark-done-btn" onclick="markDone('m4')">Отбележи като готово [mark done] ✓</button>
            <button class="hint-toggle" onclick="toggleHint(this)">покажи [show] подсказка [hint]</button>
            <div class="hint-body">
              Ако получиш [get] грешка [error] като <em>ModuleNotFoundError: No module named 'anthropic'</em>,
              изпълни [run] <code style="color:var(--green)">pip3 install anthropic</code> отново [again] и се увери [make sure],
              че пише [says] "Successfully installed".<br><br>
              Ако pip каже [says] <em>"externally-managed-environment"</em>, опитай [try]:
              <code style="color:var(--green)">pip3 install --user anthropic</code><br><br>
              Ако получиш [get] <em>AuthenticationError</em>, провери [check] API ключа [key] — увери се [make sure], че си
              копирал [copied] целия [the whole thing], включително [including] частта [part] <code>sk-ant-</code>.
            </div>
          </div>
        `,
        quiz: [
          { q: 'Коя Python библиотека [library] инсталираш [install] за Claude?', options: ['openai', 'anthropic', 'claude', 'requests'], answer: 1 },
          { q: 'Какво контролира [controls] max_tokens?', options: ['Скоростта [speed] на отговора [response]', 'Колко дълъг [how long] може да е отговорът [response]', 'Цената [cost] на API', 'Версията [version] на модела [model]'], answer: 1 },
          { q: 'Къде НИКОГА [never] не трябва да слагаш [put] реалния [real] API ключ [key]?', options: ['В скрипт [script], който commit-ваш в Git', 'В .env файл [file]', 'В паметта [memory] си', 'В конфигурационен [config] файл [file], пазен [kept] в тайна [private]'], answer: 0 }
        ]
      },
      {
        id: 'm5',
        title: 'Build something real',
        content: `
          <p>Имаш всички части [all the pieces]. Сега ще построиш [build] нещо, което наистина [actually]
          помага [helps] на проекта [project] на татко за AI.</p>

          <h3>Какво научи [what you learned]</h3>
          <p>✓ Навигация [navigate] в terminal<br>
          ✓ Писане [write] на Python (променливи [variables], функции [functions], цикли [loops], input)<br>
          ✓ Разбиране [understand] какво са AI и API<br>
          ✓ Извикване [call] на Claude от Python код [code]</p>

          <h3>Твоята мисия [your mission]</h3>
          <p>Татко ще ти даде [give] конкретна задача [specific task] за проекта [project]. Ако нямаш [don't have] такава още [yet],
          построи [build] това: скрипт [script], с който можеш [can] да напишеш [type] всякакъв въпрос [any question] и Claude да го отговори [answers]. Може да е:</p>
          <ul style="margin: 0.5rem 0 0.5rem 1.5rem; line-height: 2;">
            <li>Скрипт [script], който резюмира [summarizes] текст [text] с Claude</li>
            <li>Малък инструмент [small tool], който отговаря [answers] на въпроси [questions] по дадена тема [topic]</li>
            <li>Автоматизиране [automating] на нещо повтарящо се [repetitive] с AI</li>
          </ul>
          <p>Използвай [use] <code style="color:var(--green)">ask_claude.py</code> като начална точка [starting point].
          Промени [modify] prompt-а, добави [add] <code>input()</code> за да могат потребителите [users] да пишат [type] въпроси [questions],
          или обходи [loop through] списък [list] от неща.</p>

          <h3>Шаблон [template] за начало [to start from]</h3>
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
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Мисия 5 Предизвикателство [Challenge]</div>
            <p>Построй [build] инструмента [tool], описан [described] от татко. Накарай [make] го да работи [work] без грешки [errors].
            Покажи [show] на татко, че работи [works]. Върни се [come back] и го отбележи [mark it].</p>
            <button class="mark-done-btn" onclick="markDone('m5')">Отбележи като готово [mark done] ✓</button>
            <button class="hint-toggle" onclick="toggleHint(this)">покажи [show] подсказка [hint]</button>
            <div class="hint-body">
              Започни [start] просто [simple]. Накарай [make] го да работи [work] с един твърдо написан [hard-coded] prompt първо [first],
              после добави [add] <code>input()</code>, за да стане интерактивен [interactive].
              Ако си заседнал [stuck], постни [paste] грешката [error message] в Claude и го попитай [ask]
              да обясни [explain] какво не е наред [what's wrong].
            </div>
          </div>
        `,
        quiz: [
          { q: 'В шаблона [template], коя функция [function] обвива [wraps] Claude API извикването [call]?', options: ['call()', 'ask()', 'claude()', 'query()'], answer: 1 },
          { q: 'Коя Python функция [function] получава [gets] вход [input] от потребителя [user]?', options: ['read()', 'scan()', 'input()', 'get()'], answer: 2 },
          { q: 'Какво трябва да правиш [do], ако скриптът [script] дава грешка [error]?', options: ['Изтрий [delete] го и започни отново [start over]', 'Постни [paste] грешката [error] в Claude и попитай [ask] какво означава [means]', 'Се откажи [give up]', 'Игнорирай [ignore] я'], answer: 1 }
        ]
      }
    ];
```

- [ ] **Step 2: Verify in browser**

Open `index-bg.html`. Complete Mission 1 quiz to unlock Mission 2. Verify Mission 4 shows the API key steps in Bulgarian. The idea box in completed missions should show in Bulgarian.

- [ ] **Step 3: Commit**

```bash
cd /Users/derwin4o/kid-ai-tutorial
git add index-bg.html
git commit -m "feat(bg): add missions 4-5 (API call, Build something real)"
```

---

### Task 4: Translate Missions 6–7 (Git, GitHub)

**Files:**
- Modify: `index-bg.html` (add m6, m7 to MODULES array)

- [ ] **Step 1: Add missions 6 and 7 — replace `];` at end of MODULES with:**

```javascript
      ,{
        id: 'm6',
        title: 'Save your work with Git',
        content: `
          <p>Всеки инженер [engineer] запазва [saves] работата [work] си с Git. То е като машина на времето [time machine] за кода [code] ти —
          можеш [can] винаги [always] да се върнеш [go back] към всяка запазена версия [saved version]. Нека го настроим [set it up].</p>

          <h3>Какво е Git?</h3>
          <p>Git следи [tracks] промените [changes] в твоите файлове [files]. Всеки път [every time], когато запазиш [save] "commit", Git
          помни [remembers] точно [exactly] как е изглеждал [looked] кодът [code] ти в този момент [at that moment]. Ако счупиш [break] нещо, можеш [can] да се върнеш [go back].</p>

          <h3>Настрой [set up] Git (само при първи път [first time only])</h3>
          <div class="code-block">
            <pre>git config --global user.name "Your Name"
git config --global user.email "your@email.com"</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Инициализирай [initialize] repo в проекта [project] си</h3>
          <div class="code-block">
            <pre>cd ~/ai-lab
git init</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Запази [save] работата [work] си</h3>
          <p>Кажи [tell] на Git кои файлове [files] да включи [include]:</p>
          <div class="code-block">
            <pre>git add .</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>
          <p>Запази [save] снимка [snapshot] с описание [message] какво си направил [what you did]:</p>
          <div class="code-block">
            <pre>git commit -m "my first AI tool"</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>
          <p>Виж [see] историята [history] си:</p>
          <div class="code-block">
            <pre>git log --oneline</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Мисия 6 Предизвикателство [Challenge]</div>
            <p style="color:#ff8844;border:1px solid #ff8844;border-radius:4px;padding:0.4rem 0.6rem;font-size:0.85rem;">
              ⚠️ Преди [before] да направиш [make] commit: отвори [open] Python файловете [files] и замени [replace] реалния [real] API ключ [key] с текста [text]
              <code>YOUR_API_KEY_HERE</code> отново [again]. Никога [never] не commit-вай реален [real] API ключ [key] в Git.
            </p>
            <p>1. Влез [go] в папката [folder] <strong>ai-lab</strong> в terminal-а.<br>
            2. Изпълни [run] <code>git init</code>.<br>
            3. Отвори [open] Python файловете [files] и замени [replace] API ключа [key] с <code>YOUR_API_KEY_HERE</code>.<br>
            4. Изпълни [run] <code>git add .</code> после <code>git commit -m "my first AI tool"</code>.<br>
            5. Изпълни [run] <code>git log --oneline</code> — трябва да видиш [see] commit-а си.</p>
            <p>Когато видиш [see] commit-а в историята [log], ти си истински инженер [real engineer]. Отбележи [mark] го.</p>
            <button class="mark-done-btn" onclick="markDone('m6')">Отбележи като готово [mark done] ✓</button>
            <button class="hint-toggle" onclick="toggleHint(this)">покажи [show] подсказка [hint]</button>
            <div class="hint-body">
              Ако git не е инсталиран [installed]: <code style="color:var(--green)">sudo apt install git</code><br><br>
              Ако git се оплаква [complains] за твоето [name/email], изпълни [run] config командите [commands] горе [above] първо [first],
              замествайки [replacing] "Your Name" и "your@email.com" с твоите данни [details].<br><br>
              <code style="color:var(--green)">git config --global user.name "Ada"</code><br>
              <code style="color:var(--green)">git config --global user.email "ada@example.com"</code>
            </div>
          </div>
        `,
        quiz: [
          { q: 'Коя команда [command] създава [creates] нов git репозитори [repository]?', options: ['git start', 'git create', 'git init', 'git new'], answer: 2 },
          { q: 'Коя команда [command] добавя [stages] всички файлове [files] за commit?', options: ['git save .', 'git add .', 'git stage .', 'git push .'], answer: 1 },
          { q: 'Какво показва [shows] git log?', options: ['Текущите [current] ти файлове [files]', 'Историята [history] на commit-ите ти', 'Git настройките [settings]', 'Отдалечени репозитории [remote repositories]'], answer: 1 }
        ]
      },
      {
        id: 'm7',
        title: 'GitHub — Put Your Code Online',
        content: `
          <p>Git запазва [saves] кода [code] ти локално [locally]. GitHub го качва [puts it] в интернет [internet] — backup, споделен [shareable] и в безопасност [safe].
          Всеки [every] истински [real] проект [project] живее [lives] в GitHub.</p>

          <h3>Какво е GitHub?</h3>
          <p>GitHub е уебсайт [website], който съхранява [stores] Git репозитории [repositories]. Мисли [think] за него като Google Drive
          за код [code] — но по-умен [smarter], защото разбира [understands] commit-и и история [history].</p>

          <h3>Стъпка [step] 1: Създай [create] акаунт [account]</h3>
          <p>Отиди [go] на <strong>github.com</strong> и създай [create] безплатен [free] акаунт [account].
          Помоли [ask] татко да помогне [help] ако имаш нужда [need].</p>

          <h3>Стъпка [step] 2: Създай [create] нов репозитори [repository]</h3>
          <p>Натисни [click] бутона [button] <strong>+</strong> → "New repository". Назови [name] го <code style="color:var(--green)">ai-lab</code>.
          Остави [leave] го публичен [public]. НЕ добавяй [add] README (вече имаш [already have] файловете [files] си). Натисни [click] "Create repository".</p>

          <h3>Стъпка [step] 3: Свържи [connect] и качи [push]</h3>
          <p>GitHub ще ти покаже [show] команди [commands]. Копирай [copy] тези, започващи [starting] с <code style="color:var(--green)">git remote add</code>.
          Изглеждат [look] така:</p>
          <div class="code-block">
            <pre>git remote add origin https://github.com/YOUR_USERNAME/ai-lab.git
git branch -M main
git push -u origin main</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Стъпка [step] 4: Pull — вземи [get] последните промени [latest changes]</h3>
          <p>Ако някой друг [someone else] промени [changes] кода [code] ти (или ти редактираш [edit] директно [directly] в GitHub), използвай [use]:</p>
          <div class="code-block">
            <pre>git pull</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Мисия 7 Предизвикателство [Challenge]</div>
            <p>1. Създай [create] GitHub акаунт [account] (или използвай [use] на татко).<br>
            2. Създай [create] нов repo с имe <strong>ai-lab</strong>.<br>
            3. Качи [push] локалната [local] папка [folder] ai-lab в него.<br>
            4. Отвори [open] GitHub страницата [page] и виж [see] кода [code] си онлайн [online].</p>
            <button class="mark-done-btn" onclick="markDone('m7')">Отбележи като готово [mark done] ✓</button>
            <button class="hint-toggle" onclick="toggleHint(this)">покажи [show] подсказка [hint]</button>
            <div class="hint-body">
              Увери се [make sure], че си в папката [folder] ai-lab първо [first]:<br>
              <code style="color:var(--green)">cd ~/ai-lab</code><br><br>
              После изпълни [run] трите команди [three commands], показани [shown] от GitHub след [after] създаването [creating] на repo.
              Първата [first] е <code style="color:var(--green)">git remote add origin ...</code>
            </div>
          </div>
        `,
        quiz: [
          { q: 'Коя команда [command] свързва [connects] локалния [local] repo с GitHub?', options: ['git connect', 'git remote add origin', 'git link', 'git attach'], answer: 1 },
          { q: 'Коя команда [command] качва [uploads] commit-ите ти в GitHub?', options: ['git upload', 'git send', 'git push', 'git sync'], answer: 2 },
          { q: 'Коя команда [command] изтегля [downloads] последните промени [latest changes] от GitHub?', options: ['git download', 'git fetch', 'git pull', 'git get'], answer: 2 }
        ]
      }
    ];
```

- [ ] **Step 2: Verify in browser**

Progress through to Mission 6 (or manually set localStorage). Confirm Bulgarian text renders correctly, warning box in Mission 6 shows in Bulgarian.

- [ ] **Step 3: Commit**

```bash
cd /Users/derwin4o/kid-ai-tutorial
git add index-bg.html
git commit -m "feat(bg): add missions 6-7 (Git, GitHub)"
```

---

### Task 5: Translate Missions 8–10 (Prompt Engineering, Python+Files, Multi-turn Chat)

**Files:**
- Modify: `index-bg.html` (add m8, m9, m10 to MODULES array)

- [ ] **Step 1: Add missions 8, 9, and 10 — replace `];` at end of MODULES with:**

```javascript
      ,{
        id: 'm8',
        title: 'Prompt Engineering',
        content: `
          <p>Можеш [can] да получиш [get] драматично [dramatically] по-добри [better] отговори [answers] от Claude само като промениш [change] начина [the way], по който питаш [ask].
          Това умение [skill] — писане [writing] на добри prompt-и — се казва [is called] <strong style="color:var(--green)">prompt engineering</strong>.</p>

          <h3>Бъди конкретен [be specific]</h3>
          <p>Лошо [bad]: <em>"Tell me about dogs."</em><br>
          Добро [good]: <em>"In 3 bullet points, explain what makes dogs good pets for families with young children."</em></p>

          <h3>Дай [give] на Claude роля [role] (system prompt)</h3>
          <p>Можеш [can] да зададеш [set] <strong style="color:var(--green)">system prompt</strong>, за да кажеш [tell] на Claude кой [who] да бъде [to be]:</p>
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
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Few-shot примери [examples]</h3>
          <p>Покажи [show] на Claude какво искаш [want], като дадеш [give] примери [examples] преди [before] въпроса [question] си:</p>
          <div class="code-block">
            <pre>prompt = """
Translate these sentences to pirate speak:
Hello → Ahoy!
How are you? → How be ye?
Now translate this: What time is it?
"""</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Мисия 8 Предизвикателство [Challenge]</div>
            <p>Напиши [write] две версии [two versions] на prompt, питащ [asking] Claude да обясни [explain] какво е променлива [variable]:<br>
            1. Неясна версия [vague version] (кратка [short], без контекст [no context])<br>
            2. Конкретна версия [specific version] (роля [role] + аудитория [audience] + формат [format])<br>
            Стартирай [run] и двете с твоя скрипт [script] и сравни [compare] отговорите [answers].</p>
            <button class="mark-done-btn" onclick="markDone('m8')">Отбележи като готово [mark done] ✓</button>
            <button class="hint-toggle" onclick="toggleHint(this)">покажи [show] подсказка [hint]</button>
            <div class="hint-body">
              Неясно [vague]: <code style="color:var(--green)">"What is a variable?"</code><br><br>
              Конкретно [specific]: <code style="color:var(--green)">"You are a teacher for 13-year-olds.
              Explain what a variable is in Python using one real-world analogy. Keep it under 3 sentences."</code>
            </div>
          </div>
        `,
        quiz: [
          { q: 'Какво е system prompt?', options: ['Съобщение за грешка [error message] от terminal', 'Инструкции [instructions], казващи [telling] на AI как да се държи [behave] преди [before] разговора [conversation] започне [starts]', 'Вид [type of] Python функция [function]', 'Първото съобщение [first message] на потребителя [user]'], answer: 1 },
          { q: 'Какво е few-shot prompting?', options: ['Използване [using] на малък [small] AI модел [model]', 'Даване [giving] на AI примери [examples] от това [of what], което искаш [want], преди [before] въпроса [question] си', 'Бърз [fast] начин [way] за стартиране [run] на код [code]', 'Изпращане [sending] на кратки [short] съобщения [messages]'], answer: 1 },
          { q: 'Кой prompt ще получи [get] по-добър [better] отговор [answer]?', options: ['Tell me about loops', 'In 2 sentences for a beginner, explain what a Python for loop does', 'loops?', 'loop info'], answer: 1 }
        ]
      },
      {
        id: 'm9',
        title: 'Python + Files',
        content: `
          <p>Истинските програми [real programs] четат [read] и пишат [write] файлове [files] — логове [logs], данни [data], резултати [results]. Нека научим [learn] как
          Python борави [handles] с файлове [files], после ще подаваме [feed] съдържание [content] на Claude.</p>

          <h3>Запис [writing] на файл [file]</h3>
          <div class="code-block">
            <pre>with open("notes.txt", "w") as f:
    f.write("Line one\\n")
    f.write("Line two\\n")</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Четене [reading] на файл [file]</h3>
          <div class="code-block">
            <pre>with open("notes.txt", "r") as f:
    content = f.read()
    print(content)</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Четене [reading] ред по ред [line by line]</h3>
          <div class="code-block">
            <pre>with open("notes.txt", "r") as f:
    for line in f.readlines():
        print(line.strip())</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Изпращане [send] на всеки ред [each line] към Claude</h3>
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
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Мисия 9 Предизвикателство [Challenge]</div>
            <p>1. Създай [create] файл [file] с имe <strong>facts.txt</strong> с 3 интересни факта [interesting facts] (по един на ред [one per line]).<br>
            2. Напиши [write] Python скрипт [script], който чете [reads] всеки ред [each line] и пита [asks] Claude за подходящ [matching] emoji.<br>
            3. Отпечатай [print] оригиналния факт [original fact] + emoji-то, избрано [chosen] от Claude.</p>
            <button class="mark-done-btn" onclick="markDone('m9')">Отбележи като готово [mark done] ✓</button>
            <button class="hint-toggle" onclick="toggleHint(this)">покажи [show] подсказка [hint]</button>
            <div class="hint-body">
              Създай [create] facts.txt с nano:<br>
              <code style="color:var(--green)">nano facts.txt</code><br><br>
              Твоят prompt към Claude може да е:<br>
              <code style="color:var(--green)">f"Give exactly one emoji that matches this fact: {line}"</code>
            </div>
          </div>
        `,
        quiz: [
          { q: 'Коя Python функция [function] отваря [opens] файл [file]?', options: ['read()', 'file()', 'open()', 'load()'], answer: 2 },
          { q: 'Какво прави [does] "with open(filename) as f:"?', options: ['Създава [creates] нов файл [new file]', 'Отваря [opens] файл [file] и автоматично [automatically] го затваря [closes] когато свърши [done]', 'Изтрива [deletes] файл [file]', 'Отпечатва [prints] файл [file]'], answer: 1 },
          { q: 'Как четеш [read] всички редове [all lines] на файл [file] в списък [list]?', options: ['f.read()', 'f.lines()', 'f.readlines()', 'f.all()'], answer: 2 }
        ]
      },
      {
        id: 'm10',
        title: 'Multi-turn Chat',
        content: `
          <p>Засега [so far] всеки скрипт [script] изпраща [sends] едно съобщение [one message] и получава [receives] един отговор [one answer]. Истинските чат-ботове [real chatbots]
          помнят [remember] какво [what] си казал [said] преди [before]. Нека построим [build] такъв.</p>

          <h3>Как работи [works] историята на разговора [conversation history]</h3>
          <p>Anthropic API не запомня [remembers] автоматично [automatically] предишни [previous] съобщения [messages].
          Пазиш [keep] списък [list] с имe <code style="color:var(--green)">messages</code> и
          добавяш [add] към него всеки ход [every turn]:</p>
          <div class="code-block">
            <pre>messages = []

# Turn 1
messages.append({"role": "user", "content": "My name is Ada."})
# ... get response, add it too ...
messages.append({"role": "assistant", "content": "Nice to meet you, Ada!"})

# Turn 2 — Claude now knows your name
messages.append({"role": "user", "content": "What's my name?"})</pre>
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <h3>Пълен [full] чат-бот [chatbot] цикъл [loop]</h3>
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
            <button class="copy-btn" onclick="copyCode(this)">копирай [copy]</button>
          </div>

          <div class="challenge">
            <div class="challenge-title">⚡ Мисия 10 Предизвикателство [Challenge]</div>
            <p>1. Копирай [copy] скрипта за чат-бот [chatbot script] горе в файл [file] с имe <strong>chatbot.py</strong>.<br>
            2. Стартирай [run] го и проведи [have] разговор [conversation] от 5 съобщения [messages] с Claude.<br>
            3. В съобщение [message] 5, попитай [ask] Claude нещо, което изисква [requires] да помни [remember] съобщение [message] 1.<br>
            4. Помни ли [does it remember]?</p>
            <button class="mark-done-btn" onclick="markDone('m10')">Отбележи като готово [mark done] ✓</button>
            <button class="hint-toggle" onclick="toggleHint(this)">покажи [show] подсказка [hint]</button>
            <div class="hint-body">
              Опитай [try] да кажеш [tell] на Claude името [name] си в първото съобщение [first message], после да питаш [ask]
              "What's my name?" по-късно [later]. Ако историята [history] работи [works], ще знае [will know].<br><br>
              Ако получиш [get] грешка [error] за token лимит [token limit], намали [reduce] MAX_HISTORY на 4.
            </div>
          </div>
        `,
        quiz: [
          { q: 'В Anthropic API, кой списък [list] пази [stores] историята на разговора [conversation history]?', options: ['history', 'chat', 'messages', 'conversation'], answer: 2 },
          { q: 'Каква роля [role] получава [gets] отговорът [response] на AI в списъка [list] messages?', options: ['ai', 'bot', 'assistant', 'claude'], answer: 2 },
          { q: 'Защо ограничаваме [limit] дължината [length] на историята [history] на разговора [conversation]?', options: ['Изисква се [required] от API', 'За да спестим [save] API токени [tokens] и намалим [reduce] разходите [cost]', 'Защото Claude забравя [forgets] така или иначе [anyway]', 'За да ускорим [speed up] отговорите [responses]'], answer: 1 }
        ]
      }
    ];
```

- [ ] **Step 2: Verify in browser**

Open `index-bg.html`. All 10 missions should now render. Click through a few missions and verify Bulgarian text with English brackets appears correctly throughout.

- [ ] **Step 3: Final commit**

```bash
cd /Users/derwin4o/kid-ai-tutorial
git add index-bg.html
git commit -m "feat(bg): add missions 8-10 (Prompt Engineering, Files, Chat) — Bulgarian version complete"
```

---

## Self-Review Checklist

- [x] **Spec coverage:** All spec requirements covered — page chrome, JS strings, all 10 missions, inline bilingual dictionary, Claude responds in Bulgarian, code examples unchanged, separate file
- [x] **No placeholders:** All Bulgarian translations written inline — no TBDs
- [x] **Type consistency:** `markDone('mN')` IDs match across all tasks
- [x] **Dictionary density:** Action words and nouns consistently bracketed throughout all missions
