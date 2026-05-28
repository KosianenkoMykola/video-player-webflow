# Інструкція з налаштування квізу розрахунку вартості воріт в Webflow

## Структура кроків

Квіз воріт має 6 кроків:

1. **Розміри воріт** (повзунки для висоти та ширини)
2. **Вибір каркасу** (радіо-кнопки)
3. **Вибір фарбування** (радіо-кнопки)
4. **Вибір наповнення** (радіо-кнопки)
5. **Вибір замка** (радіо-кнопки)
6. **Монтажні роботи + форма з контактами**

---

## Атрибути для HTML елементів

### Загальна структура

**Форма:**

- Клас: `calculate_form_gates`

**Контейнер форми:**

- Клас: `calculate_form_gates_block`

**Кроки:**

- Клас: `calc-step`
- Атрибут: `if-step` (на кожному кроці)

**Progress bar:**

- Атрибут: `if-element="progress-bar"`

**Кнопка "Далі":**

- Атрибут: `if-element="button-next"` або клас: `quiz_button`

**Відображення загальної ціни:**

- Атрибут: `if-element="total-price"` (на останньому кроці)
- Атрибут: `if-element="step-price"` (на кожному кроці для відображення поточної вартості - **рекомендовано додати на всі кроки**)

**Важливо:** Елемент з `if-element="step-price"` на кожному кроці автоматично оновлюється при зміні будь-яких параметрів розрахунку, показуючи поточну орієнтовну вартість.

---

## Крок 1: Розміри воріт (висота, ширина)

**Ширина:**

```html
<input
  type="range"
  data-quiz="width"
  min="1"
  max="6"
  value="3"
  step="0.1"
  required
/>
<span data-quiz="width-display">3 м</span>
```

**Висота:**

```html
<input
  type="range"
  data-quiz="height"
  min="1.5"
  max="5"
  value="2.5"
  step="0.1"
  required
/>
<span data-quiz="height-display">2.5 м</span>
```

**Примітки:**

- Використовуйте `type="range"` для повзунків
- `data-quiz="width-display"` та `data-quiz="height-display"` - елементи для відображення поточних значень
- Встановіть `min`, `max`, `value` та `step` відповідно до ваших потреб
- Обидва поля мають бути `required`

---

## Крок 2: Оберіть каркас

Формули (з ТЗ):

1. **Звари сам:** `6300 + 875 × висота × ширина`
2. **Стандарт:** `5600 + 1700 × висота × ширина`
3. **Стронг:** `7800 + 2500 × висота × ширина`
4. **Преміум:** `22500 + 4500 × висота × ширина`

Додаткова надбавка за висоту:

- якщо **висота ≤ 4.1 м** — **+2500 грн**
- якщо **висота > 4.1 м** — **+4000 грн**

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="gates-frame"
  data-quiz="gates-frame"
  data-base="6300"
  data-coef="875"
  required
/>
<label>Звари сам</label>

<input
  type="radio"
  name="gates-frame"
  data-quiz="gates-frame"
  data-base="5600"
  data-coef="1700"
  required
/>
<label>Стандарт</label>

<input
  type="radio"
  name="gates-frame"
  data-quiz="gates-frame"
  data-base="7800"
  data-coef="2500"
  required
/>
<label>Стронг</label>

<input
  type="radio"
  name="gates-frame"
  data-quiz="gates-frame"
  data-base="22500"
  data-coef="4500"
  required
/>
<label>Преміум</label>
```

**Примітки:**

- `name="gates-frame"` - однаковий для всіх варіантів
- `data-quiz="gates-frame"` - обов'язковий атрибут
- `data-base` - фіксована частина формули
- `data-coef` - множник для `висота × ширина`
- Надбавка за висоту (`+2500` або `+4000`) вже реалізована в JavaScript

**Формула в коді:**

```
Вартість каркасу =
  data-base + data-coef × (висота × ширина)
  + (2500, якщо висота ≤ 4.1 м)
  + (4000, якщо висота > 4.1 м)
```

---

## Крок 3: Оберіть фарбування

Формули (з ТЗ):

1. **Без фарбування:** `0 грн`
2. **Стандарт:** `2000 + 240 × висота × ширина`
3. **Преміум:** `2750 + 650 × висота × ширина`
4. **Порошкове:** `6000 + 1900 × висота × ширина`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="gates-paint"
  data-quiz="gates-paint"
  data-base="0"
  data-coef="0"
  required
/>
<label>Без фарбування</label>

<input
  type="radio"
  name="gates-paint"
  data-quiz="gates-paint"
  data-base="2000"
  data-coef="240"
  required
/>
<label>Стандарт</label>

<input
  type="radio"
  name="gates-paint"
  data-quiz="gates-paint"
  data-base="2750"
  data-coef="650"
  required
/>
<label>Преміум</label>

<input
  type="radio"
  name="gates-paint"
  data-quiz="gates-paint"
  data-base="6000"
  data-coef="1900"
  required
/>
<label>Порошкове</label>
```

**Примітки:**

- `name="gates-paint"` - однаковий для всіх варіантів
- `data-quiz="gates-paint"` - обов'язковий атрибут
- `data-base` - фіксована частина
- `data-coef` - множник для `висота × ширина`

**Формула в коді:**

```
Вартість фарбування = data-base + data-coef × (висота × ширина)
```

---

## Крок 4: Оберіть наповнення

Формули (з ТЗ):

1. Профнастил: `600 × висота × ширина`
2. Штахетник: `900 × висота × ширина`
3. Жалюзі: `1450 × висота × ширина`
4. Ранчо: `1850 × висота × ширина`
5. Горизонт: `2400 × висота × ширина`
6. VOX: `3350 × висота × ширина`
7. Мустанг: `2100 × висота × ширина`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="gates-fill"
  data-quiz="gates-fill"
  data-coef="600"
  required
/>
<label>Профнастил</label>

<input
  type="radio"
  name="gates-fill"
  data-quiz="gates-fill"
  data-coef="900"
  required
/>
<label>Штахетник</label>

<input
  type="radio"
  name="gates-fill"
  data-quiz="gates-fill"
  data-coef="1450"
  required
/>
<label>Жалюзі</label>

<input
  type="radio"
  name="gates-fill"
  data-quiz="gates-fill"
  data-coef="1850"
  required
/>
<label>Ранчо</label>

<input
  type="radio"
  name="gates-fill"
  data-quiz="gates-fill"
  data-coef="2400"
  required
/>
<label>Горизонт</label>

<input
  type="radio"
  name="gates-fill"
  data-quiz="gates-fill"
  data-coef="3350"
  required
/>
<label>ВОХ</label>

<input
  type="radio"
  name="gates-fill"
  data-quiz="gates-fill"
  data-coef="2100"
  required
/>
<label>Мустанг</label>
```

**Примітки:**

- `name="gates-fill"` - однаковий для всіх варіантів
- `data-quiz="gates-fill"` - обов'язковий атрибут
- `data-coef` - множник для `висота × ширина`

**Формула в коді:**

```
Вартість наповнення = data-coef × (висота × ширина)
```

---

## Крок 5: Оберіть замок

Формули (з ТЗ):

1. Економ: `15000`
2. Стандарт: `20000`
3. Стронг: `25000`
4. Преміум: `45000`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="gates-lock"
  data-quiz="gates-lock"
  data-price="15000"
  required
/>
<label>Економ</label>

<input
  type="radio"
  name="gates-lock"
  data-quiz="gates-lock"
  data-price="20000"
  required
/>
<label>Стандарт</label>

<input
  type="radio"
  name="gates-lock"
  data-quiz="gates-lock"
  data-price="25000"
  required
/>
<label>Стронг</label>

<input
  type="radio"
  name="gates-lock"
  data-quiz="gates-lock"
  data-price="45000"
  required
/>
<label>Преміум</label>
```

**Примітки:**

- `name="gates-lock"` - однаковий для всіх варіантів
- `data-quiz="gates-lock"` - обов'язковий атрибут
- `data-price` - фіксована ціна замка

**Формула в коді:**

```
Вартість замка = data-price
```

---

## Крок 6: Монтажні роботи

Формули (з ТЗ):

1. Монтаж наповнення: `700 × висота × ширина`
2. Монтаж воріт: `11000`
3. Монтаж закладної з бетонуванням: `13800`
4. Монтаж автоматики: `11700`
5. Монтаж стовпів для кріплення: `6500`

**Чекбокси (можна вибрати кілька):**

```html
<label>
  <input type="checkbox" data-quiz="gates-work" data-base="0" data-coef="700" />
  Монтаж наповнення
</label>

<label>
  <input
    type="checkbox"
    data-quiz="gates-work"
    data-base="11000"
    data-coef="0"
  />
  Монтаж воріт
</label>

<label>
  <input
    type="checkbox"
    data-quiz="gates-work"
    data-base="13800"
    data-coef="0"
  />
  Монтаж закладної з бетонуванням
</label>

<label>
  <input
    type="checkbox"
    data-quiz="gates-work"
    data-base="11700"
    data-coef="0"
  />
  Монтаж автоматики
</label>

<label>
  <input
    type="checkbox"
    data-quiz="gates-work"
    data-base="6500"
    data-coef="0"
  />
  Монтаж стовпів для кріплення
</label>
```

**Примітки:**

- `data-quiz="gates-work"` - обов'язковий атрибут
- Можна обрати кілька варіантів одночасно
- `data-base` - фіксована частина
- `data-coef` - множник для `висота × ширина` (якщо потрібно)

**Формула в коді (для кожного чекбоксу):**

```
Вартість роботи = data-base + data-coef × (висота × ширина)
Загальна вартість монтажу = сума всіх вибраних робіт
```

---

## Підсумковий крок (ворота)

**Елемент для відображення загальної ціни:**

```html
<div if-element="total-price">0 ₴</div>
```

**Форма для відправки даних:**

- Звичайні поля форми (ім'я, телефон, email тощо)
- Кнопка відправки: `type="submit"` (стандартна відправка Webflow)

---

## Формула розрахунку загальної вартості воріт

Умовні позначення:

- `H` — висота (м)
- `W` — ширина (м)
- `S` — площа = `H × W`

```
1. Каркас:
   frame = data-base_frame + data-coef_frame × S
         + (2500, якщо H ≤ 4.1 м)
         + (4000, якщо H > 4.1 м)

2. Фарбування:
   paint = data-base_paint + data-coef_paint × S

3. Наповнення:
   fill = coef_fill × S

4. Замок:
   lock = data-price_lock

5. Монтажні роботи:
   works = Σ (data-base_work + data-coef_work × S) для всіх вибраних чекбоксів

6. Загальна вартість воріт:
   total = frame + paint + fill + lock + works
```

---

## Підключення скрипту для воріт в Webflow

1. Вставте JavaScript-код з файлу `quiz.html` для воріт (блок з коментарем `// Калькулятор для воріт`) у:

   - **Page Settings → Custom Code → Before </body>**
   - або в **Embed** блок внизу сторінки з воротами.

2. Переконайтеся, що:

   - Форма має клас `calculate_form_gates`.
   - Обгортка форми має клас `calculate_form_gates_block`.
   - Кроки мають клас `calc-step` та атрибут `if-step`.
   - Прогрес-бар має `if-element="progress-bar"`.
   - Елементи для цін мають `if-element="step-price"` та `if-element="total-price"`.

3. Додайте всі атрибути `data-quiz`, `data-base`, `data-coef`, `data-price` згідно з цією інструкцією.

4. Опублікуйте сайт, щоб Webflow застосував скрипт на продакшені.

---

## Приклад повної структури в Webflow

```html
<div class="calculate_form_gates_block">
  <!-- Progress bar -->
  <div if-element="progress-bar" class="quiz_progress-bar"></div>

  <form class="calculate_form_gates">
    <!-- Крок 1: Розміри -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <label>Ширина: <span data-quiz="width-display">3 м</span></label>
      <input
        type="range"
        data-quiz="width"
        min="1"
        max="6"
        value="3"
        step="0.1"
        required
      />

      <label>Висота: <span data-quiz="height-display">2.5 м</span></label>
      <input
        type="range"
        data-quiz="height"
        min="1.5"
        max="5"
        value="2.5"
        step="0.1"
        required
      />

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 2: Каркас -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="gates-frame"
        data-quiz="gates-frame"
        data-base="6300"
        data-coef="875"
        required
      />
      <label>Звари сам</label>

      <input
        type="radio"
        name="gates-frame"
        data-quiz="gates-frame"
        data-base="5600"
        data-coef="1700"
        required
      />
      <label>Стандарт</label>

      <input
        type="radio"
        name="gates-frame"
        data-quiz="gates-frame"
        data-base="7800"
        data-coef="2500"
        required
      />
      <label>Стронг</label>

      <input
        type="radio"
        name="gates-frame"
        data-quiz="gates-frame"
        data-base="22500"
        data-coef="4500"
        required
      />
      <label>Преміум</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 3: Фарбування -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="gates-paint"
        data-quiz="gates-paint"
        data-base="0"
        data-coef="0"
        required
      />
      <label>Без фарбування</label>

      <input
        type="radio"
        name="gates-paint"
        data-quiz="gates-paint"
        data-base="2000"
        data-coef="240"
        required
      />
      <label>Стандарт</label>

      <input
        type="radio"
        name="gates-paint"
        data-quiz="gates-paint"
        data-base="2750"
        data-coef="650"
        required
      />
      <label>Преміум</label>

      <input
        type="radio"
        name="gates-paint"
        data-quiz="gates-paint"
        data-base="6000"
        data-coef="1900"
        required
      />
      <label>Порошкове</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 4: Наповнення -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="gates-fill"
        data-quiz="gates-fill"
        data-coef="600"
        required
      />
      <label>Профнастил</label>

      <!-- Інші варіанти наповнення... -->

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 5: Замок -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="gates-lock"
        data-quiz="gates-lock"
        data-price="15000"
        required
      />
      <label>Економ</label>

      <!-- Інші варіанти замка... -->

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 6: Монтажні роботи + форма -->
    <div class="calc-step" if-step>
      <div if-element="total-price">0 ₴</div>

      <label>
        <input
          type="checkbox"
          data-quiz="gates-work"
          data-base="0"
          data-coef="700"
        />
        Монтаж наповнення
      </label>

      <!-- Інші монтажні роботи... -->

      <input type="text" name="name" required placeholder="Ім'я" />
      <input type="tel" name="phone" required placeholder="Телефон" />
      <input type="email" name="email" required placeholder="Email" />
      <button type="submit">Відправити</button>
    </div>
  </form>
</div>
```

---

## Важливо

1. Всі кроки мають клас `calc-step` та атрибут `if-step`
2. Радіо-кнопки в межах одного кроку мають однаковий `name`
3. Ціни вказуються в атрибутах `data-base`, `data-coef`, `data-price` (число без символів)
4. Останній крок автоматично показує розраховану загальну ціну
5. Форма відправляється стандартним способом Webflow (POST)
6. Розрахунок відбувається в реальному часі при зміні будь-яких параметрів
7. Весь `quiz_option_item` клікабельний - не потрібно клікати саме на іконку радіо-кнопки
