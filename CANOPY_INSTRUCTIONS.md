# Інструкція з налаштування квізу розрахунку вартості навісу в Webflow

## Структура кроків

Квіз навісу має 7 кроків:

1. **Розміри навісу** (повзунки для довжини та ширини)
2. **Вибір типу навісу** (радіо-кнопки)
3. **Вибір фарбування** (радіо-кнопки)
4. **Монтаж** (радіо-кнопки: Так/Ні)
5. **Матеріал на дах** (радіо-кнопки)
6. **Монтаж покрівлі** (радіо-кнопки: Так/Ні)
7. **Декоративна зашивка + форма з контактами**

---

## Атрибути для HTML елементів

### Загальна структура

**Форма:**

- Клас: `calculate_form_canopy`

**Контейнер форми:**

- Клас: `calculate_form_canopy_block`

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

## Крок 1: Розміри навісу (довжина, ширина)

**Довжина:**

```html
<input
  type="range"
  data-quiz="length"
  min="1"
  max="20"
  value="5"
  step="0.5"
  required
/>
<span data-quiz="length-display">5 м</span>
```

**Ширина:**

```html
<input
  type="range"
  data-quiz="width"
  min="1"
  max="10"
  value="3"
  step="0.5"
  required
/>
<span data-quiz="width-display">3 м</span>
```

**Примітки:**

- Використовуйте `type="range"` для повзунків
- `data-quiz="length-display"` та `data-quiz="width-display"` - елементи для відображення поточних значень
- Встановіть `min`, `max`, `value` та `step` відповідно до ваших потреб
- Обидва поля мають бути `required`

---

## Крок 2: Оберіть тип навісу

Формули (з ТЗ):

1. **Односхилий (без ферм):** `(3500 + 2400) × м² = 5900 × м²`
2. **Односхилий (з фермами):** `(3500 + 2500) × м² = 6000 × м²`
3. **Напівкруглий (з фермами):** `(3500 + 3100) × м² = 6600 × м²`
4. **Двохсхилий з фермами:** `(3500 + 3400) × м² = 6900 × м²`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="canopy-type"
  data-quiz="canopy-type"
  data-base="3500"
  data-coef="2400"
  required
/>
<label>Односхилий (без ферм)</label>

<input
  type="radio"
  name="canopy-type"
  data-quiz="canopy-type"
  data-base="3500"
  data-coef="2500"
  required
/>
<label>Односхилий (з фермами)</label>

<input
  type="radio"
  name="canopy-type"
  data-quiz="canopy-type"
  data-base="3500"
  data-coef="3100"
  required
/>
<label>Напівкруглий (з фермами)</label>

<input
  type="radio"
  name="canopy-type"
  data-quiz="canopy-type"
  data-base="3500"
  data-coef="3400"
  required
/>
<label>Двохсхилий з фермами</label>
```

**Примітки:**

- `name="canopy-type"` - однаковий для всіх варіантів
- `data-quiz="canopy-type"` - обов'язковий атрибут
- `data-base` - базова частина формули
- `data-coef` - коефіцієнт для формули
- Формула в коді: `(data-base + data-coef) × м²`

**Формула в коді:**

```
Вартість типу навісу = (data-base + data-coef) × (довжина × ширина)
```

---

## Крок 3: Оберіть тип фарбування

Формули (з ТЗ):

1. **Каркас без фарбування:** `0 грн`
2. **Звичайне фарбування:** `625 грн × м²`
3. **Молоткове фарбування:** `1041 грн × м²`
4. **Порошкове фарбування:** `3500 грн × м²`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="canopy-paint"
  data-quiz="canopy-paint"
  data-coef="0"
  required
/>
<label>Каркас без фарбування</label>

<input
  type="radio"
  name="canopy-paint"
  data-quiz="canopy-paint"
  data-coef="625"
  required
/>
<label>Звичайне фарбування</label>

<input
  type="radio"
  name="canopy-paint"
  data-quiz="canopy-paint"
  data-coef="1041"
  required
/>
<label>Молоткове фарбування</label>

<input
  type="radio"
  name="canopy-paint"
  data-quiz="canopy-paint"
  data-coef="3500"
  required
/>
<label>Порошкове фарбування</label>
```

**Примітки:**

- `name="canopy-paint"` - однаковий для всіх варіантів
- `data-quiz="canopy-paint"` - обов'язковий атрибут
- `data-coef` - коефіцієнт за м²

**Формула в коді:**

```
Вартість фарбування = data-coef × (довжина × ширина)
```

---

## Крок 4: Монтаж даху / покрівлі

Питання (один варіант відповіді):

- Монтаж даху
- Монтаж покрівлі
- Монтаж даху і покрівлі
- Не потрібно

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="canopy-installation"
  data-quiz="canopy-installation"
  data-installation-mode="frame"
  required
/>
<label>Монтаж даху (+689 грн)</label>

<input
  type="radio"
  name="canopy-installation"
  data-quiz="canopy-installation"
  data-installation-mode="roof"
  required
/>
<label>Монтаж покрівлі (+550 грн)</label>

<input
  type="radio"
  name="canopy-installation"
  data-quiz="canopy-installation"
  data-installation-mode="both"
  required
/>
<label>Монтаж даху і покрівлі (+689 грн + 550 грн)</label>

<input
  type="radio"
  name="canopy-installation"
  data-quiz="canopy-installation"
  data-installation-mode="none"
  required
/>
<label>Не потрібно</label>
```

**Примітки:**

- `name="canopy-installation"` - однаковий для всіх варіантів
- `data-quiz="canopy-installation"` - обов'язковий атрибут
- `data-installation-mode` - значення: `"frame"`, `"roof"`, `"both"`, `"none"`
- `required` - обов'язкове поле

**Формула:**

- Якщо вибрано **Монтаж даху**: +689 грн
- Якщо вибрано **Монтаж покрівлі**: +550 грн
- Якщо вибрано **Монтаж даху і покрівлі**: +689 грн + 550 грн
- Якщо вибрано **Не потрібно**: 0 грн

---

## Крок 5: Матеріал на дах

Формули (з ТЗ):

1. **Без покриття:** `0 грн`
2. **Профнастил Італія 0.45 мм:** `250 грн × м²`
3. **Профнастил Німеччина 0.5 мм:** `460 грн × м²`
4. **Профнастил Україна 0.45 мм:** `200 грн × м²`
5. **Металочерепиця:** `450 грн × м²`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="canopy-roof-material"
  data-quiz="canopy-roof-material"
  data-coef="0"
  required
/>
<label>Без покриття</label>

<input
  type="radio"
  name="canopy-roof-material"
  data-quiz="canopy-roof-material"
  data-coef="250"
  required
/>
<label>Профнастил Італія 0.45 мм</label>

<input
  type="radio"
  name="canopy-roof-material"
  data-quiz="canopy-roof-material"
  data-coef="460"
  required
/>
<label>Профнастил Німеччина 0.5 мм</label>

<input
  type="radio"
  name="canopy-roof-material"
  data-quiz="canopy-roof-material"
  data-coef="200"
  required
/>
<label>Профнастил Україна 0.45 мм</label>

<input
  type="radio"
  name="canopy-roof-material"
  data-quiz="canopy-roof-material"
  data-coef="450"
  required
/>
<label>Металочерепиця</label>
```

**Примітки:**

- `name="canopy-roof-material"` - однаковий для всіх варіантів
- `data-quiz="canopy-roof-material"` - обов'язковий атрибут
- `data-coef` - коефіцієнт за м²

**Формула в коді:**

```
Вартість матеріалу на дах = data-coef × (довжина × ширина)
```

---

## Крок 6: Монтаж покрівлі

> Цей крок об'єднано з **Кроком 4: Монтаж даху / покрівлі** і окремо не використовується.

---

## Крок 7: Декоративна зашивка сторін (висота 0,5м)

Формули (з ТЗ):

1. **Ні:** `0 грн`
2. **Ранчо/жалюзі:** `(довжина + ширина) × 2 × 2000 грн`
3. **Клік-фальц:** `(довжина + ширина) × 2 × 1850 грн`
4. **Фасадні касети:** `(довжина + ширина) × 2 × 3000 грн`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="canopy-decorative"
  data-quiz="canopy-decorative"
  data-coef="0"
  required
/>
<label>Ні</label>

<input
  type="radio"
  name="canopy-decorative"
  data-quiz="canopy-decorative"
  data-coef="2000"
  required
/>
<label>Ранчо/жалюзі</label>

<input
  type="radio"
  name="canopy-decorative"
  data-quiz="canopy-decorative"
  data-coef="1850"
  required
/>
<label>Клік-фальц</label>

<input
  type="radio"
  name="canopy-decorative"
  data-quiz="canopy-decorative"
  data-coef="3000"
  required
/>
<label>Фасадні касети</label>
```

**Примітки:**

- `name="canopy-decorative"` - однаковий для всіх варіантів
- `data-quiz="canopy-decorative"` - обов'язковий атрибут
- `data-coef` - коефіцієнт для формули периметра

**Формула в коді:**

```
Вартість декоративної зашивки = (довжина + ширина) × 2 × data-coef
```

---

## Підсумковий крок (навіс)

**Елемент для відображення загальної ціни:**

```html
<div if-element="total-price">0 ₴</div>
```

**Форма для відправки даних:**

- Звичайні поля форми (ім'я, телефон, email тощо)
- Кнопка відправки: `type="submit"` (стандартна відправка Webflow)

---

## Формула розрахунку загальної вартості навісу

Умовні позначення:

- `L` — довжина (м)
- `W` — ширина (м)
- `S` — площа = `L × W` (м²)
- `P` — периметр = `2 × (L + W)` (м)

```
1. Тип навісу:
   type = (data-base + data-coef) × S

2. Фарбування:
   paint = data-coef_paint × S

3. Монтаж:
   installation = 689 (якщо вибрано "Так"), або 0

4. Матеріал на дах:
   roof_material = data-coef_roof × S

5. Монтаж покрівлі:
   roof_installation = 550 (якщо вибрано "Так"), або 0

6. Декоративна зашивка:
   decorative = P × data-coef_decorative (якщо вибрано опцію), або 0

7. Загальна вартість навісу:
   total = type + paint + installation + roof_material + roof_installation + decorative
```

---

## Підключення скрипту для навісу в Webflow

1. Вставте JavaScript-код з файлу `quiz.html` для навісу (блок з коментарем `<!-- Калькулятор для навісів -->`) у:

   - **Page Settings → Custom Code → Before </body>**
   - або в **Embed** блок внизу сторінки з навісом.

2. Переконайтеся, що:

   - Форма має клас `calculate_form_canopy`.
   - Обгортка форми має клас `calculate_form_canopy_block`.
   - Кроки мають клас `calc-step` та атрибут `if-step`.
   - Прогрес-бар має `if-element="progress-bar"`.
   - Елементи для цін мають `if-element="step-price"` та `if-element="total-price"`.

3. Додайте всі атрибути `data-quiz`, `data-base`, `data-coef`, `data-installation-value`, `data-roof-installation-value` згідно з цією інструкцією.

4. Опублікуйте сайт, щоб Webflow застосував скрипт на продакшені.

---

## Приклад повної структури в Webflow

```html
<div class="calculate_form_canopy_block">
  <!-- Progress bar -->
  <div if-element="progress-bar" class="quiz_progress-bar"></div>

  <form class="calculate_form_canopy">
    <!-- Крок 1: Розміри -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <label>Довжина: <span data-quiz="length-display">5 м</span></label>
      <input
        type="range"
        data-quiz="length"
        min="1"
        max="20"
        value="5"
        step="0.5"
        required
      />

      <label>Ширина: <span data-quiz="width-display">3 м</span></label>
      <input
        type="range"
        data-quiz="width"
        min="1"
        max="10"
        value="3"
        step="0.5"
        required
      />

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 2: Тип навісу -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="canopy-type"
        data-quiz="canopy-type"
        data-base="3500"
        data-coef="2400"
        required
      />
      <label>Односхилий (без ферм)</label>

      <input
        type="radio"
        name="canopy-type"
        data-quiz="canopy-type"
        data-base="3500"
        data-coef="2500"
        required
      />
      <label>Односхилий (з фермами)</label>

      <input
        type="radio"
        name="canopy-type"
        data-quiz="canopy-type"
        data-base="3500"
        data-coef="3100"
        required
      />
      <label>Напівкруглий (з фермами)</label>

      <input
        type="radio"
        name="canopy-type"
        data-quiz="canopy-type"
        data-base="3500"
        data-coef="3400"
        required
      />
      <label>Двохсхилий з фермами</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 3: Фарбування -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="canopy-paint"
        data-quiz="canopy-paint"
        data-coef="0"
        required
      />
      <label>Каркас без фарбування</label>

      <input
        type="radio"
        name="canopy-paint"
        data-quiz="canopy-paint"
        data-coef="625"
        required
      />
      <label>Звичайне фарбування</label>

      <input
        type="radio"
        name="canopy-paint"
        data-quiz="canopy-paint"
        data-coef="1041"
        required
      />
      <label>Молоткове фарбування</label>

      <input
        type="radio"
        name="canopy-paint"
        data-quiz="canopy-paint"
        data-coef="3500"
        required
      />
      <label>Порошкове фарбування</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 4: Монтаж -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="canopy-installation"
        data-quiz="canopy-installation"
        data-installation-value="yes"
        required
      />
      <label>Так (+689 грн)</label>

      <input
        type="radio"
        name="canopy-installation"
        data-quiz="canopy-installation"
        data-installation-value="no"
        required
      />
      <label>Ні</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 5: Матеріал на дах -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="canopy-roof-material"
        data-quiz="canopy-roof-material"
        data-coef="0"
        required
      />
      <label>Без покриття</label>

      <input
        type="radio"
        name="canopy-roof-material"
        data-quiz="canopy-roof-material"
        data-coef="250"
        required
      />
      <label>Профнастил Італія 0.45 мм</label>

      <input
        type="radio"
        name="canopy-roof-material"
        data-quiz="canopy-roof-material"
        data-coef="460"
        required
      />
      <label>Профнастил Німеччина 0.5 мм</label>

      <input
        type="radio"
        name="canopy-roof-material"
        data-quiz="canopy-roof-material"
        data-coef="200"
        required
      />
      <label>Профнастил Україна 0.45 мм</label>

      <input
        type="radio"
        name="canopy-roof-material"
        data-quiz="canopy-roof-material"
        data-coef="450"
        required
      />
      <label>Металочерепиця</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 6: Монтаж покрівлі -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="canopy-roof-installation"
        data-quiz="canopy-roof-installation"
        data-roof-installation-value="yes"
        required
      />
      <label>Так (+550 грн)</label>

      <input
        type="radio"
        name="canopy-roof-installation"
        data-quiz="canopy-roof-installation"
        data-roof-installation-value="no"
        required
      />
      <label>Ні</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 7: Декоративна зашивка + форма -->
    <div class="calc-step" if-step>
      <div if-element="total-price">0 ₴</div>

      <input
        type="radio"
        name="canopy-decorative"
        data-quiz="canopy-decorative"
        data-coef="0"
        required
      />
      <label>Ні</label>

      <input
        type="radio"
        name="canopy-decorative"
        data-quiz="canopy-decorative"
        data-coef="2000"
        required
      />
      <label>Ранчо/жалюзі</label>

      <input
        type="radio"
        name="canopy-decorative"
        data-quiz="canopy-decorative"
        data-coef="1850"
        required
      />
      <label>Клік-фальц</label>

      <input
        type="radio"
        name="canopy-decorative"
        data-quiz="canopy-decorative"
        data-coef="3000"
        required
      />
      <label>Фасадні касети</label>

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
3. Ціни вказуються в атрибутах `data-base`, `data-coef` (число без символів)
4. Останній крок автоматично показує розраховану загальну ціну
5. Форма відправляється стандартним способом Webflow (POST)
6. Розрахунок відбувається в реальному часі при зміні будь-яких параметрів
7. Весь `quiz_option_item` клікабельний - не потрібно клікати саме на іконку радіо-кнопки
