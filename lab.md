# sdlc-lab-Sheremetev
# Тема: Software Development Life Cycle (SDLC) — Життєвий цикл програмного забезпечення

---

# 1. Планування
**Мета:** створити офлайн міні-чат — користувач вводить повідомлення, бот відповідає фіксованими фразами відповідно до обраної теми. Користувач повинен мати змогу надсилати повідомлення, бачити історію чату та очищати її без підключення до інтернету.

**Коротке формулювання мети продукту:** Користувач повинен мати можливість вводити повідомлення, отримувати миттєву відповідь бота з набору фіксованих фраз та переглядати історію діалогу.

---

# 2. Аналіз вимог (User stories)
1. **Як користувач, я хочу вводити повідомлення в поле вводу, щоб почати діалог.** **(Must have)**  
2. **Як користувач, я хочу отримувати відповідь бота одразу після відправлення, щоб імітувати переписку.** **(Must have)**  
3. Як користувач, я хочу бачити історію повідомлень на екрані, щоб переглядати попередній діалог.  
4. Як користувач, я хочу очищати історію чату, щоб почати новий діалог.  
5. Як користувач, я хочу обирати тему відповідей бота (Привітання, Підтримка, Випадкові фрази), щоб змінювати стиль відповідей.

---

# 3. Дизайн

## Головна сторінка

Ми переходимо на сайт, де розміщений міні‑чат офлайн. У заголовку вказано Лабораторна робота №3 — Міні-чат офлайн, під заголовком — Тема: Software Development Life Cycle (SDLC).
Мета: створити офлайн міні‑чат — вводити повідомлення, отримувати відповідь бота, переглядати та очищати історію; обирати тему відповідей.

На сторінці видно область історії повідомлень з вертикальною прокруткою для зручного перегляду попередніх діалогів. Нижче цієї області розташована панель вводу: поле для введення повідомлення, поруч «Тема бота» для вибору стилю відповідей і кнопки для керування — «Відправити» та «Очистити».

## Рекомендована розкладка головного екрану:
- Заголовок сторінки
- Область історії повідомлень (скролиться)
- Панель вводу: select(тема) | input(текст) | button(Відправити) | button(Очистити)

# Зображення прототипу:
<img width="1058" height="702" alt="image" src="https://github.com/user-attachments/assets/bedd461d-6ff9-4957-9805-9d5e5d918333" />

---
# Ключова функція:
<img width="1206" height="830" alt="image" src="https://github.com/user-attachments/assets/705c4e79-6733-4c25-91a0-1a4dbeb9bdd9" />

На скрині зображено основну функцію сайту: офлайн міні‑чат, який дозволяє відправляти повідомлення, миттєво отримувати відповіді від бота, переглядати історію діалогу та керувати темою відповідей. Інтерфейс поєднує область історії, панель вводу та елементи керування темою і очищенням історії.

# Основні можливості (ключові функції)

**Ввід повідомлення**
Поле для введення тексту; підтримка відправки по кнопці «Відправити» або клавішею Enter.

**Генерація відповіді бота**
Повернення відповіді через коротку затримку; логіка включає тригери (привітання, питання) і вибір випадкової фрази з пулу теми.

**Перегляд історії**
Прокручуваний блок із бульбашками повідомлень (користувач — праворуч, бот — ліворуч); зберігання локально для збереження після перезавантаження.

**Очищення історії**
Кнопка «Очистити» видаляє всі повідомлення і очищує локальне сховище, повертаючи інтерфейс у початковий стан.

**Вибір теми бота**
Селект або набір кнопок для зміни стилю відповідей (Наприклад: Привітання, Підтримка, Випадкові фрази); нові випадкові відповіді формуються відповідно до обраної теми.

---

# 4. Реалізація

Система реалізована як клієнтський веб‑додаток, написаний мовами HTML, CSS та JavaScript (односторінковий інтерфейс без серверної частини).

### Chat bot.html
```html
<!doctype html>
<html lang="uk">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Міні-чат (офлайн) — SDLC Lab</title>
  <style>
    :root{
      --bg:#f4f7fb; --panel:#ffffff; --user:#d6eefc; --bot:#ffffff;
      --accent:#2563eb; --muted:#6b7280; --danger:#ef4444; --radius:12px;
      font-family: Inter, Roboto, system-ui, -apple-system, "Segoe UI", Arial;
    }
    *{box-sizing:border-box}
    body{margin:0;background:var(--bg);min-height:100vh;display:flex;justify-content:center;padding:28px}
    .wrap{width:100%;max-width:880px}
    header{display:flex;flex-direction:column;gap:6px;margin-bottom:12px}
    h1{margin:0;color:var(--accent);font-size:1.25rem}
    .subtitle{margin:0;color:var(--muted);font-size:0.95rem}

    .app{background:transparent;display:flex;flex-direction:column;gap:12px}
    .meta{color:var(--muted);font-size:0.95rem}

    .chat{
      background:linear-gradient(180deg, rgba(255,255,255,0.92), rgba(255,255,255,0.98));
      padding:14px;border-radius:14px;min-height:420px;max-height:66vh;overflow:auto;
      box-shadow:0 8px 28px rgba(15,23,42,0.06);display:flex;flex-direction:column;gap:8px;
    }
    .message{max-width:72%;padding:10px 14px;border-radius:14px;line-height:1.35;word-break:break-word}
    .message.bot{align-self:flex-start;background:var(--bot);border:1px solid rgba(16,24,40,0.04)}
    .message.user{align-self:flex-end;background:var(--user);border:1px solid rgba(37,99,235,0.06)}
    .meta-info{font-size:0.78rem;color:var(--muted);margin-top:4px}

    .controls{display:flex;gap:8px;align-items:center;flex-wrap:wrap;margin-top:8px}
    .controls select, .controls input{padding:10px;border-radius:10px;border:1px solid #e6e9ee;background:#fff}
    .controls input{flex:1;min-width:180px}
    .controls button{padding:10px 14px;border-radius:10px;border:none;background:var(--accent);color:#fff;cursor:pointer;font-weight:600}
    .controls button.clear{background:var(--danger)}
    .notes{color:var(--muted);font-size:0.87rem;margin-top:6px}

    /* small screens */
    @media (max-width:560px){
      .message{max-width:86%}
      .controls{gap:6px}
      .controls select{min-width:120px}
    }
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <h1>Лабораторна робота №3 — Міні-чат офлайн</h1>
      <p class="subtitle">Тема: Software Development Life Cycle (SDLC)</p>
    </header>

    <main class="app" role="main">
      <div class="meta"><strong>Мета:</strong> створити офлайн міні-чат — вводити повідомлення, отримувати відповідь бота, переглядати та очищати історію; обирати тему відповідей.</div>

      <div id="chat" class="chat" aria-live="polite" aria-label="Історія чату"></div>

      <div class="controls" role="region" aria-label="Панель вводу">
        <label for="theme" class="visually-hidden">Тема бота</label>
        <select id="theme" title="Оберіть тему відповідей">
          <option value="greeting">Привітання</option>
          <option value="support">Підтримка</option>
          <option value="random">Випадкові фрази</option>
        </select>

        <input id="input" type="text" placeholder="Введіть повідомлення..." aria-label="Поле вводу повідомлення" />

        <button id="send">Відправити</button>
        <button id="clear" class="clear">Очистити</button>
      </div>

      <div class="notes">Порада: натиснути Enter для відправки. Історія зберігається в поточній вкладці (localStorage).</div>
    </main>
  </div>

  <script>
    /* Простий офлайн міні-чат, який реалізує 5 user stories:
       1) Вводити повідомлення
       2) Отримувати миттєву відповідь бота
       3) Бачити історію повідомлень
       4) Очищати історію чату
       5) Обирати тему відповідей бота
    */

    // Елементи
    const chatEl = document.getElementById('chat');
    const inputEl = document.getElementById('input');
    const sendBtn = document.getElementById('send');
    const clearBtn = document.getElementById('clear');
    const themeSel = document.getElementById('theme');

    // Історія повідомлень (зберігаємо в localStorage для зручності)
    const STORAGE_KEY = 'offline-mini-chat-history-v1';
    let history = loadHistory();

    // Пули фраз для тем
    const pools = {
      greeting: ["Здрастуйте!", "Привіт, як справи?", "Радий вас бачити!"],
      support: ["Я вас слухаю.", "Розкажіть детальніше, будь ласка.", "Спробуємо знайти рішення разом."],
      random: ["Сьогодні чудовий день.", "Кава — найкращий друг.", "Не хвилюйтеся, все буде добре."]
    };

    // Відрендерити історію у DOM
    function render() {
      chatEl.innerHTML = '';
      history.forEach(item => {
        const d = document.createElement('div');
        d.className = 'message ' + (item.sender === 'user' ? 'user' : 'bot');
        d.textContent = item.text;
        chatEl.appendChild(d);
      });
      // прокрутка вниз
      chatEl.scrollTop = chatEl.scrollHeight;
    }

    // Завантажити історію з localStorage
    function loadHistory() {
      try {
        const raw = localStorage.getItem(STORAGE_KEY);
        if (!raw) return [];
        return JSON.parse(raw);
      } catch (e) {
        return [];
      }
    }

    // Зберегти історію
    function saveHistory() {
      try { localStorage.setItem(STORAGE_KEY, JSON.stringify(history)); } catch(e){}
    }

    // Перевірка тригерів у повідомленні
    function containsGreeting(text) {
      return /(^|\s)(прив|здрав|добро|hello|hi)/i.test(text);
    }
    function containsQuestion(text) {
      return /\?$/.test(text.trim());
    }

    // Вибір випадкового елементу
    function choice(arr){ return arr[Math.floor(Math.random()*arr.length)]; }

    // Генерація відповіді бота (логіка: тригери -> пул теми)
    function generateReply(userText, theme) {
      if (containsGreeting(userText)) return "Привіт! Чим можу допомогти?";
      if (containsQuestion(userText)) return "Цікаве питання. Треба подумати.";
      const pool = pools[theme] || pools.random;
      return choice(pool);
    }

    // Показ помилки (коротке повідомлення від бота)
    function showTemporaryError(text) {
      const err = { sender: 'bot', text };
      history.push(err);
      render();
      setTimeout(() => {
        // видалити останнє повідомлення, якщо воно те саме
        if (history.length && history[history.length-1] === err) {
          history.pop(); saveHistory(); render();
        }
      }, 2000);
    }

    // Обробник відправки
    function send() {
      const raw = inputEl.value;
      if (!raw || !raw.trim()) {
        showTemporaryError('Помилка: повідомлення не може бути пустим');
        return;
      }
      const userMsg = { sender: 'user', text: raw.trim(), t: Date.now() };
      history.push(userMsg);
      saveHistory(); render();
      inputEl.value = '';
      inputEl.focus();

      // Імітація невеликої затримки і генерація відповіді
      setTimeout(() => {
        const theme = themeSel.value;
        const botText = generateReply(raw, theme);
        const botMsg = { sender: 'bot', text: botText, t: Date.now() };
        history.push(botMsg);
        saveHistory(); render();
      }, 180);
    }

    // Очистити історію
    function clearHistory() {
      if (!history.length) return;
      history = [];
      try { localStorage.removeItem(STORAGE_KEY); } catch(e){}
      render();
    }

    // Події
    sendBtn.addEventListener('click', send);
    inputEl.addEventListener('keydown', e => { if (e.key === 'Enter') send(); });
    clearBtn.addEventListener('click', clearHistory);

    // Початковий рендер
    render();
  </script>
</body>
</html>

```
---
# 5. Тестування

Нижче — набір тест-кейсів для перевірки функціональності міні‑чату. Для кожного тесту вказано: мета, вхідні дані, очікуваний результат.

## Тест 1 — Відправка простого привітання
Мета: перевірити розпізнавання привітання й пріоритет тригера.

Вхід: "Привіт"

Очікуваний результат: У історії з’являється повідомлення користувача (вирівняне праворуч).

З’являється відповідь бота: "Привіт! Чим можу допомогти?".

## Скриншот:
<img width="811" height="578" alt="image" src="https://github.com/user-attachments/assets/5aff867b-0815-44bf-a6db-c7599d561e1e" />

## Нетипові форми привітання
Вхід: Хай 

Очікувана відповідь: випадкова фраза з пула «Привітання» (наприклад: Здрастуйте!) — якщо слово не розпізнано як тригер

## Скриншот:
<img width="808" height="579" alt="image" src="https://github.com/user-attachments/assets/801b3a9a-8c85-4c07-8389-adc4dec42a75" />

---

## Тест 2 — Відправка питання
Мета: перевірити розпізнавання питання у кінці рядка.

Вхід: "Що таке SDLC?"

Очікуваний результат: У історії з’являється повідомлення користувача.

З’являється відповідь бота на питання: "Цікаве питання. Треба подумати.".

## Скриншот:
<img width="808" height="585" alt="image" src="https://github.com/user-attachments/assets/6164977d-522e-4324-aa28-c30df2a50135" />

## Нейтральні повідомлення
Вхід: Я тут 

Очікувана відповідь: Сьогодні чудовий день. (або інша випадкова фраза з пулу)

Вхід: Що по погоді 

Очікувана відповідь: Кава — найкращий друг. (бот не робить реальних запитів, дає випадкову фразу)

## Скриншот:
<img width="809" height="591" alt="image" src="https://github.com/user-attachments/assets/67ff988a-9123-4cc3-b774-168c51994279" />

---

## Тест 3 — Зміна теми і вибір випадкової відповіді
Мета: перевірити, що вибір теми впливає на пул випадкових відповідей.

Тема: "Підтримка"

Вхід: Мені потрібна допомога 

Очікувана відповідь: Спробуємо знайти рішення разом.

Вхід: Не знаю, що робити 

Очікувана відповідь: Розкажіть детальніше, будь ласка.

Очікуваний результат: Бот дає відповіді з пулу «Підтримка» (ввічливі/підтримуючі фрази).

## Скриншот:
<img width="815" height="580" alt="image" src="https://github.com/user-attachments/assets/59b4c262-8a7b-432c-8535-600806c5ade3" />

---
