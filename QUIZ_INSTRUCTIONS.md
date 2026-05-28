# Інструкція з налаштування квізу розрахунку вартості ангару в Webflow

## Структура кроків

Квіз має 7 кроків:

1. **Розрахунок розмірів** (повзунки для ширини, довжини та висоти)
2. **Вибір типу ангару** (радіо-кнопки)
3. **Вибір каркасу ангару** (радіо-кнопки)
4. **Супутні товари** (кількість вікон, дверей, воріт)
5. **Монтаж** (радіо-кнопки: Так/Ні)
6. **Фундамент** (радіо-кнопки: Так/Ні)
7. **Підсумок з ціною та формою**

---

## Атрибути для HTML елементів

### Загальна структура

**Форма:**

- Клас: `calculate_form_1`

**Контейнер форми:**

- Клас: `calculate_form_hanger`

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

## Крок 1: Розрахунок розмірів (повзунки)

**Повзунок ширини:**

```html
<input
  type="range"
  data-quiz="width"
  min="1"
  max="50"
  value="10"
  step="1"
  required
/>
<span data-quiz="width-display">10 м</span>
```

**Повзунок довжини:**

```html
<input
  type="range"
  data-quiz="length"
  min="1"
  max="100"
  value="20"
  step="1"
  required
/>
<span data-quiz="length-display">20 м</span>
```

**Повзунок висоти:**

```html
<input
  type="range"
  data-quiz="height"
  min="2"
  max="15"
  value="4"
  step="0.5"
  required
/>
<span data-quiz="height-display">4 м</span>
```

**Примітки:**

- Використовуйте `type="range"` для повзунків
- `data-quiz="width-display"`, `data-quiz="length-display"` та `data-quiz="height-display"` - елементи для відображення поточних значень
- Встановіть `min`, `max`, `value` та `step` відповідно до ваших потреб
- Всі три поля мають бути `required`

---

## Крок 2: Вибір типу ангару

**Радіо-кнопки з формулами розрахунку:**

```html
<input
  type="radio"
  name="hangar-type"
  data-quiz="hangar-type"
  data-hangar-type="profiled"
  data-price="270"
  required
/>
<label>Ангар з профнастилу</label>

<input
  type="radio"
  name="hangar-type"
  data-quiz="hangar-type"
  data-hangar-type="sandwich"
  data-price="1000"
  required
/>
<label>Ангар з сендвіч-панелей</label>

<input
  type="radio"
  name="hangar-type"
  data-quiz="hangar-type"
  data-hangar-type="refrigerated"
  data-price="1600"
  required
/>
<label>Холодильний ангар</label>
```

**Примітки:**

- `name` має бути однаковим для всіх варіантів (`name="hangar-type"`)
- `data-quiz="hangar-type"` - обов'язковий атрибут
- `data-hangar-type` - тип ангару: `"profiled"`, `"sandwich"`, або `"refrigerated"`
- `data-price` - ціна за м² підлоги для розрахунку
- `required` - обов'язкове поле

**Формула розрахунку типу ангару:**

```
Площа стін = 2 × (Довжина + Ширина) × Висота
Вартість типу = Площа стін + (Довжина × Ширина × data-price)
```

---

## Крок 3: Вибір каркасу ангару (метал)

**Радіо-кнопки з ціною за м² підлоги:**

```html
<input
  type="radio"
  name="frame-type"
  data-quiz="frame-type"
  data-frame-type="light"
  data-price="1200"
  required
/>
<label>Легкий (1200 ₴/м²)</label>

<input
  type="radio"
  name="frame-type"
  data-quiz="frame-type"
  data-frame-type="standard"
  data-price="11500"
  required
/>
<label>Стандарт (11500 ₴/м²)</label>

<input
  type="radio"
  name="frame-type"
  data-quiz="frame-type"
  data-frame-type="reinforced"
  data-price="11800"
  required
/>
<label>Посилений (11800 ₴/м²)</label>

<input
  type="radio"
  name="frame-type"
  data-quiz="frame-type"
  data-frame-type="industrial"
  data-price="12200"
  required
/>
<label>Промисловий (12200 ₴/м²)</label>
```

**Примітки:**

- `name` має бути однаковим для всіх варіантів (`name="frame-type"`)
- `data-quiz="frame-type"` - обов'язковий атрибут
- `data-frame-type` - тип каркасу: `"light"`, `"standard"`, `"reinforced"`, або `"industrial"`
- `data-price` - ціна за м² підлоги
- `required` - обов'язкове поле

**Формула розрахунку каркасу:**

```
Вартість каркасу = Довжина × Ширина × data-price
```

---

## Крок 4: Супутні товари

**Поле для кількості вікон:**

```html
<label>Кількість вікон:</label>
<input type="number" data-quiz="windows" min="0" value="0" step="1" />
<span>× 4000 ₴</span>
```

**Поле для кількості дверей:**

```html
<label>Кількість дверей:</label>
<input type="number" data-quiz="doors" min="0" value="0" step="1" />
<span>× 4000 ₴</span>
```

**Поле для кількості воріт:**

```html
<label>Кількість воріт:</label>
<input type="number" data-quiz="gates" min="0" value="0" step="1" />
<span>× 25000 ₴</span>
```

**Примітки:**

- Використовуйте `type="number"` для полів вводу кількості
- `data-quiz="windows"`, `data-quiz="doors"`, `data-quiz="gates"` - обов'язкові атрибути
- `min="0"` - мінімальна кількість
- Поля не обов'язкові (можна залишити 0)

**Формула розрахунку супутніх товарів:**

```
Вартість супутніх товарів = (Вікна × 4000) + (Двері × 4000) + (Ворота × 25000)
```

---

## Крок 5: Монтаж

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="installation"
  data-quiz="installation"
  data-installation-value="yes"
  required
/>
<label>Так (+20% до загальної вартості)</label>

<input
  type="radio"
  name="installation"
  data-quiz="installation"
  data-installation-value="no"
  required
/>
<label>Ні</label>
```

**Примітки:**

- `name` має бути однаковим для всіх варіантів (`name="installation"`)
- `data-quiz="installation"` - обов'язковий атрибут
- `data-installation-value` - значення: `"yes"` або `"no"`
- `required` - обов'язкове поле

**Формула:**

- Якщо вибрано "Так": Загальна вартість × 1.2 (+20%)
- Якщо вибрано "Ні": залишається без змін

---

## Крок 6: Фундамент

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="foundation"
  data-quiz="foundation"
  data-foundation-value="yes"
  required
/>
<label>Так (1500 ₴/м²)</label>

<input
  type="radio"
  name="foundation"
  data-quiz="foundation"
  data-foundation-value="no"
  required
/>
<label>Ні</label>
```

**Примітки:**

- `name` має бути однаковим для всіх варіантів (`name="foundation"`)
- `data-quiz="foundation"` - обов'язковий атрибут
- `data-foundation-value` - значення: `"yes"` або `"no"`
- `required` - обов'язкове поле

**Формула розрахунку фундаменту:**

```
Вартість фундаменту = Довжина × Ширина × 1500 (якщо вибрано "Так")
```

---

## Крок 7: Підсумок з ціною та формою

**Елемент для відображення загальної ціни:**

```html
<div if-element="total-price">0 ₴</div>
```

**Форма для відправки даних:**

- Звичайні поля форми (ім'я, телефон, email тощо)
- Кнопка відправки: `type="submit"` (буде працювати стандартна відправка Webflow)

---

## Формула розрахунку загальної вартості

```
1. Площа підлоги = Довжина × Ширина (м²)
2. Площа стін = 2 × (Довжина + Ширина) × Висота (м²)

3. Вартість типу ангару:
   = Площа стін + (Площа підлоги × Ціна типу за м²)

4. Вартість каркасу:
   = Площа підлоги × Ціна каркасу за м²

5. Вартість супутніх товарів:
   = (Вікна × 4000) + (Двері × 4000) + (Ворота × 25000)

6. Вартість фундаменту:
   = Площа підлоги × 1500 (якщо вибрано "Так")

7. Базова вартість:
   = Вартість типу + Вартість каркасу + Супутні товари + Фундамент

8. Загальна вартість:
   = Базова вартість × 1.2 (якщо вибрано монтаж "Так")
   = Базова вартість (якщо вибрано монтаж "Ні")
```

---

## Приклад повної структури в Webflow

```html
<div class="calculate_form_1_block">
  <!-- Progress bar -->
  <div if-element="progress-bar" class="quiz_progress-bar"></div>

  <form class="calculate_form_1">
    <!-- Крок 1: Розміри (повзунки) -->
    <div class="calc-step" if-step>
      <!-- Відображення поточної ціни -->
      <div if-element="step-price">0 ₴</div>

      <!-- Повзунок ширини -->
      <label>Ширина: <span data-quiz="width-display">10 м</span></label>
      <input
        type="range"
        data-quiz="width"
        min="1"
        max="50"
        value="10"
        step="1"
        required
      />

      <!-- Повзунок довжини -->
      <label>Довжина: <span data-quiz="length-display">20 м</span></label>
      <input
        type="range"
        data-quiz="length"
        min="1"
        max="100"
        value="20"
        step="1"
        required
      />

      <!-- Повзунок висоти -->
      <label>Висота: <span data-quiz="height-display">4 м</span></label>
      <input
        type="range"
        data-quiz="height"
        min="2"
        max="15"
        value="4"
        step="0.5"
        required
      />

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 2: Тип ангару -->
    <div class="calc-step" if-step>
      <!-- Відображення поточної ціни -->
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="hangar-type"
        data-quiz="hangar-type"
        data-hangar-type="profiled"
        data-price="270"
        required
      />
      <label>Ангар з профнастилу</label>

      <input
        type="radio"
        name="hangar-type"
        data-quiz="hangar-type"
        data-hangar-type="sandwich"
        data-price="1000"
        required
      />
      <label>Ангар з сендвіч-панелей</label>

      <input
        type="radio"
        name="hangar-type"
        data-quiz="hangar-type"
        data-hangar-type="refrigerated"
        data-price="1600"
        required
      />
      <label>Холодильний ангар</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 3: Каркас ангару -->
    <div class="calc-step" if-step>
      <!-- Відображення поточної ціни -->
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="frame-type"
        data-quiz="frame-type"
        data-frame-type="light"
        data-price="1200"
        required
      />
      <label>Легкий (1200 ₴/м²)</label>

      <input
        type="radio"
        name="frame-type"
        data-quiz="frame-type"
        data-frame-type="standard"
        data-price="11500"
        required
      />
      <label>Стандарт (11500 ₴/м²)</label>

      <input
        type="radio"
        name="frame-type"
        data-quiz="frame-type"
        data-frame-type="reinforced"
        data-price="11800"
        required
      />
      <label>Посилений (11800 ₴/м²)</label>

      <input
        type="radio"
        name="frame-type"
        data-quiz="frame-type"
        data-frame-type="industrial"
        data-price="12200"
        required
      />
      <label>Промисловий (12200 ₴/м²)</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 4: Супутні товари -->
    <div class="calc-step" if-step>
      <!-- Відображення поточної ціни -->
      <div if-element="step-price">0 ₴</div>

      <label>Кількість вікон:</label>
      <input type="number" data-quiz="windows" min="0" value="0" step="1" />
      <span>× 4000 ₴</span>

      <label>Кількість дверей:</label>
      <input type="number" data-quiz="doors" min="0" value="0" step="1" />
      <span>× 4000 ₴</span>

      <label>Кількість воріт:</label>
      <input type="number" data-quiz="gates" min="0" value="0" step="1" />
      <span>× 25000 ₴</span>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 5: Монтаж -->
    <div class="calc-step" if-step>
      <!-- Відображення поточної ціни -->
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="installation"
        data-quiz="installation"
        data-installation-value="yes"
        required
      />
      <label>Так (+20% до загальної вартості)</label>

      <input
        type="radio"
        name="installation"
        data-quiz="installation"
        data-installation-value="no"
        required
      />
      <label>Ні</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 6: Фундамент -->
    <div class="calc-step" if-step>
      <!-- Відображення поточної ціни -->
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="foundation"
        data-quiz="foundation"
        data-foundation-value="yes"
        required
      />
      <label>Так (1500 ₴/м²)</label>

      <input
        type="radio"
        name="foundation"
        data-quiz="foundation"
        data-foundation-value="no"
        required
      />
      <label>Ні</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 7: Підсумок -->
    <div class="calc-step" if-step>
      <div if-element="total-price">0 ₴</div>
      <input type="text" name="name" required placeholder="Ім'я" />
      <input type="tel" name="phone" required placeholder="Телефон" />
      <input type="email" name="email" required placeholder="Email" />
      <button type="submit">Відправити</button>
    </div>
  </form>
</div>
```

---

## Налаштування цін

Всі ціни встановлюються через атрибут `data-price` на відповідних елементах:

- **Тип ангару:** `data-price` - ціна за м² підлоги (270, 1000, 1600)
- **Каркас:** `data-price` - ціна за м² підлоги (1200, 11500, 11800, 12200)
- **Супутні товари:** фіксовані ціни в коді (4000 для вікон/дверей, 25000 для воріт)
- **Фундамент:** фіксована ціна в коді (1500 за м²)
- **Монтаж:** +20% до загальної вартості

---

## Важливо

1. Всі кроки мають клас `calc-step` та атрибут `if-step`
2. Радіо-кнопки в межах одного кроку мають однаковий `name`
3. Ціни вказуються в атрибуті `data-price` (число без символів)
4. Останній крок автоматично показує розраховану загальну ціну
5. Форма відправляється стандартним способом Webflow (POST)
6. Розрахунок відбувається в реальному часі при зміні будь-яких параметрів