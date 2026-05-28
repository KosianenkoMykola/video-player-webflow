# Інструкція з налаштування квізу розрахунку вартості хвіртки в Webflow

## Структура кроків

Квіз хвіртки може мати, наприклад, 6 кроків:

1. **Розміри хвіртки** (повзунки / інпути для висоти та ширини)
2. **Вибір каркасу** (радіо-кнопки)
3. **Вибір фарбування** (радіо-кнопки)
4. **Вибір наповнення** (радіо-кнопки)
5. **Вибір замка** (радіо-кнопки)
6. **Монтажні роботи + форма з контактами**

> Кількість кроків та групування питань можна змінювати, головне — зберегти атрибути `data-quiz` для кожного типу вибору.

---

## Атрибути для HTML елементів (хвіртка)

### Загальна структура

**Форма:**

- Клас: `calculate_form_gate`

**Контейнер форми:**

- Клас: `calculate_form_gate_block`

**Кроки:**

- Клас: `calc-step`
- Атрибут: `if-step` (на кожному кроці)

**Progress bar:**

- Атрибут: `if-element="progress-bar"`

**Кнопка "Далі":**

- Атрибут: `if-element="button-next"` або клас: `quiz_button`

**Відображення загальної ціни:**

- Атрибут: `if-element="total-price"` (на останньому кроці)
- Атрибут: `if-element="step-price"` (на кожному кроці для відображення поточної вартості — **рекомендовано додати на всі кроки**)

**Важливо:** Елемент з `if-element="step-price"` на кожному кроці автоматично оновлюється при зміні будь-яких параметрів розрахунку, показуючи поточну орієнтовну вартість.

---

## Крок 1: Розміри хвіртки (висота, ширина)

**Ширина:**

```html
<input
  type="range"
  data-quiz="width"
  min="0.5"
  max="2"
  value="1"
  step="0.1"
  required
/>
<span data-quiz="width-display">1 м</span>
```

**Висота:**

```html
<input
  type="range"
  data-quiz="height"
  min="1"
  max="3"
  value="2"
  step="0.1"
  required
/>
<span data-quiz="height-display">2 м</span>
```

**Примітки:**

- Використовуйте `type="range"` або `type="number"` — головне зберегти `data-quiz="width"` та `data-quiz="height"`.
- `data-quiz="width-display"` та `data-quiz="height-display"` — елементи для відображення поточних значень.
- `min`, `max`, `value` та `step` налаштовуються під ваші реальні діапазони.
- Обидва поля мають бути `required`.

---

## Крок 2: Оберіть каркас

Формули (з ТЗ):

1. **Звари сам:** `2700 + 900 × висота × ширина`
2. **Стандарт:** `3600 + 1500 × висота × ширина`
3. **Стронг:** `6000 + 2500 × висота × ширина`
4. **Преміум:** `9000 + 4500 × висота × ширина`

Додаткова надбавка за висоту:

- якщо **висота ≤ 2 м** — **+2500 грн**
- якщо **висота > 2 м** — **+4000 грн**

**Радіо-кнопки (приклад):**

```html
<input
  type="radio"
  name="gate-frame"
  data-quiz="gate-frame"
  data-base="2700"
  data-coef="900"
  required
/>
<label>Звари сам</label>

<input
  type="radio"
  name="gate-frame"
  data-quiz="gate-frame"
  data-base="3600"
  data-coef="1500"
  required
/>
<label>Стандарт</label>

<input
  type="radio"
  name="gate-frame"
  data-quiz="gate-frame"
  data-base="6000"
  data-coef="2500"
  required
/>
<label>Стронг</label>

<input
  type="radio"
  name="gate-frame"
  data-quiz="gate-frame"
  data-base="9000"
  data-coef="4500"
  required
/>
<label>Преміум</label>
```

**Примітки:**

- `name="gate-frame"` — однаковий для всіх варіантів.
- `data-quiz="gate-frame"` — обов'язковий атрибут.
- `data-base` — фіксована частина формули.
- `data-coef` — множник для `висота × ширина`.
- Надбавка за висоту (`+2500` або `+4000`) вже реалізована в JavaScript.

**Формула в коді:**

```
Вартість каркасу =
  data-base + data-coef × (висота × ширина)
  + (2500, якщо висота ≤ 2 м)
  + (4000, якщо висота > 2 м)
```

---

## Крок 3: Оберіть фарбування

Формули (з ТЗ):

1. **Без фарбування:** `0 грн`
2. **Стандарт:** `2000 + 240 × висота × ширина`
3. **Преміум:** `3200 + 600 × висота × ширина`
4. **Порошкове:** `4200 + 1800 × висота × ширина`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="gate-paint"
  data-quiz="gate-paint"
  data-base="0"
  data-coef="0"
  required
/>
<label>Без фарбування</label>

<input
  type="radio"
  name="gate-paint"
  data-quiz="gate-paint"
  data-base="2000"
  data-coef="240"
  required
/>
<label>Стандарт</label>

<input
  type="radio"
  name="gate-paint"
  data-quiz="gate-paint"
  data-base="3200"
  data-coef="600"
  required
/>
<label>Преміум</label>

<input
  type="radio"
  name="gate-paint"
  data-quiz="gate-paint"
  data-base="4200"
  data-coef="1800"
  required
/>
<label>Порошкове</label>
```

**Примітки:**

- `name="gate-paint"` — однаковий для всіх варіантів.
- `data-quiz="gate-paint"` — обов'язковий атрибут.
- `data-base` — фіксована частина.
- `data-coef` — множник для `висота × ширина`.

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

**Радіо-кнопки (один варіант):**

```html
<input
  type="radio"
  name="gate-fill"
  data-quiz="gate-fill"
  data-coef="600"
  required
/>
<label>Профнастил</label>

<input
  type="radio"
  name="gate-fill"
  data-quiz="gate-fill"
  data-coef="900"
  required
/>
<label>Штахетник</label>

<input
  type="radio"
  name="gate-fill"
  data-quiz="gate-fill"
  data-coef="1450"
  required
/>
<label>Жалюзі</label>

<!-- інші варіанти за тим же принципом -->
```

**Примітки:**

- `name="gate-fill"` — однаковий для всіх варіантів.
- `data-quiz="gate-fill"` — обов'язковий атрибут.
- `data-coef` — множник для `висота × ширина`.

**Формула в коді:**

```
Вартість наповнення = data-coef × (висота × ширина)
```

---

## Крок 5: Оберіть замок

Формули (з ТЗ):

1. Механічний: `1200`
2. Механічний з гарантією: `2800`
3. Електромеханічний: `6300`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="gate-lock"
  data-quiz="gate-lock"
  data-price="1200"
  required
/>
<label>Механічний</label>

<input
  type="radio"
  name="gate-lock"
  data-quiz="gate-lock"
  data-price="2800"
  required
/>
<label>Механічний з гарантією</label>

<input
  type="radio"
  name="gate-lock"
  data-quiz="gate-lock"
  data-price="6300"
  required
/>
<label>Електромеханічний</label>
```

**Примітки:**

- `name="gate-lock"` — однаковий для всіх варіантів.
- `data-quiz="gate-lock"` — обов'язковий атрибут.
- `data-price` — фіксована ціна замка.

**Формула в коді:**

```
Вартість замка = data-price
```

---

## Крок 6: Монтажні роботи

Формули (з ТЗ):

1. Монтаж наповнення: `600 × висота × ширина`
2. Монтаж хвіртки: `4200`
3. Монтаж замка: `3300`
4. Монтаж стовпів: `3000`

**Чекбокси (можна вибрати кілька):**

```html
<label>
  <input type="checkbox" data-quiz="gate-work" data-base="0" data-coef="600" />
  Монтаж наповнення
</label>

<label>
  <input type="checkbox" data-quiz="gate-work" data-base="4200" data-coef="0" />
  Монтаж хвіртки
</label>

<label>
  <input type="checkbox" data-quiz="gate-work" data-base="3300" data-coef="0" />
  Монтаж замка
</label>

<label>
  <input type="checkbox" data-quiz="gate-work" data-base="3000" data-coef="0" />
  Монтаж стовпів
</label>
```

**Примітки:**

- `data-quiz="gate-work"` — обов'язковий атрибут.
- Можна обрати кілька варіантів одночасно.
- `data-base` — фіксована частина.
- `data-coef` — множник для `висота × ширина` (якщо потрібно).

**Формула в коді (для кожного чекбоксу):**

```
Вартість роботи = data-base + data-coef × (висота × ширина)
Загальна вартість монтажу = сума всіх вибраних робіт
```

---

## Підсумковий крок (хвіртка)

**Елемент для відображення загальної ціни:**

```html
<div if-element="total-price">0 ₴</div>
```

**Форма для відправки даних:**

- Звичайні поля форми (ім'я, телефон, email тощо)
- Кнопка відправки: `type="submit"` (стандартна відправка Webflow)

---

## Формула розрахунку загальної вартості хвіртки

Умовні позначення:

- `H` — висота (м)
- `W` — ширина (м)
- `S` — площа = `H × W`

```
1. Каркас:
   frame = data-base_frame + data-coef_frame × S
         + (2500, якщо H ≤ 2 м)
         + (4000, якщо H > 2 м)

2. Фарбування:
   paint = data-base_paint + data-coef_paint × S

3. Наповнення:
   fill = coef_fill × S

4. Замок:
   lock = data-price_lock

5. Монтажні роботи:
   works = Σ (data-base_work + data-coef_work × S) для всіх вибраних чекбоксів

6. Загальна вартість хвіртки:
   total = frame + paint + fill + lock + works
```

---

## Підключення скрипту для хвіртки в Webflow

1. Вставте JavaScript-код з файлу `quiz.html` для хвіртки (блок з коментарем `<!-- Калькулятор для хвірток -->`) у:
   - **Page Settings → Custom Code → Before </body>**
   - або в **Embed** блок внизу сторінки з хвірткою.
2. Переконайтеся, що:
   - Форма має клас `calculate_form_gate`.
   - Обгортка форми має клас `calculate_form_gate_block`.
   - Кроки мають клас `calc-step` та атрибут `if-step`.
   - Прогрес-бар має `if-element="progress-bar"`.
   - Елементи для цін мають `if-element="step-price"` та `if-element="total-price"`.
3. Додайте всі атрибути `data-quiz`, `data-base`, `data-coef`, `data-price` згідно з цією інструкцією.
4. Опублікуйте сайт, щоб Webflow застосував скрипт на продакшені.
