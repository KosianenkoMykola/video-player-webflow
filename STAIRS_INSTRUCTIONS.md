# Інструкція з налаштування квізу розрахунку вартості сходів та перил в Webflow

## Структура кроків

Квіз сходів та перил має 6 кроків:

1. **Розміри сходів** (повзунки для висоти, ширини та кількості сходинок)
2. **Тип перил** (радіо-кнопки)
3. **Покриття** (радіо-кнопки)
4. **Монтаж та доставка** (чекбокс та радіо-кнопки)
5. **Додаткові опції** (чекбокси та радіо-кнопка "Не потрібно")
6. **Підсумок з ціною та формою**

---

## Атрибути для HTML елементів

### Загальна структура

**Форма:**

- Клас: `calculate_form_stairs`

**Контейнер форми:**

- Клас: `calculate_form_stairs_block`

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

## Крок 1: Розміри сходів (висота, ширина, кількість сходинок)

**Висота:**

```html
<input
  type="range"
  data-quiz="height"
  min="1"
  max="5"
  value="2.5"
  step="0.1"
  required
/>
<span data-quiz="height-display">2.5 м</span>
```

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

**Кількість сходинок:**

```html
<input
  type="range"
  data-quiz="steps-count"
  min="1"
  max="50"
  value="1"
  step="1"
  required
/>
<span data-quiz="steps-count-display">1</span>
```

**Примітки:**

- Використовуйте `type="range"` для всіх повзунків (висота, ширина, кількість сходинок)
- `data-quiz="height-display"`, `data-quiz="width-display"` та `data-quiz="steps-count-display"` - елементи для відображення поточних значень
- Встановіть `min`, `max`, `value` та `step` відповідно до ваших потреб
- Всі три поля мають бути `required`

**Розрахунок:**

- Кількість сходинок береться з поля `data-quiz="steps-count"` (введене користувачем значення)
- Довжина перил = `√(ширина² + висота²)` (гіпотенуза прямокутного трикутника)

---

## Крок 2: Оберіть тип перил

Формули (з ТЗ):

1. **Класичні з металу:** `7500 грн/м.п.`
2. **Метал + дерево:** `8000-9000 грн/м.п.` (середнє: 8500)
3. **Скло + метал:** `12000-13000 грн/м.п.` (середнє: 12500)
4. **З нержавіючої сталі:** `4000-4600 грн/м.п.` (середнє: 4300)

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="stairs-railing"
  data-quiz="stairs-railing"
  data-price="7500"
  required
/>
<label>Класичні з металу (7500 грн/м.п.)</label>

<input
  type="radio"
  name="stairs-railing"
  data-quiz="stairs-railing"
  data-price-min="8000"
  data-price-max="9000"
  required
/>
<label>Метал + дерево (8000-9000 грн/м.п.)</label>

<input
  type="radio"
  name="stairs-railing"
  data-quiz="stairs-railing"
  data-price-min="12000"
  data-price-max="13000"
  required
/>
<label>Скло + метал (12000-13000 грн/м.п.)</label>

<input
  type="radio"
  name="stairs-railing"
  data-quiz="stairs-railing"
  data-price-min="4000"
  data-price-max="4600"
  required
/>
<label>З нержавіючої сталі (4000-4600 грн/м.п.)</label>
```

**Примітки:**

- `name="stairs-railing"` - однаковий для всіх варіантів
- `data-quiz="stairs-railing"` - обов'язковий атрибут
- Для фіксованої ціни використовуйте `data-price`
- Для діапазону цін використовуйте `data-price-min` та `data-price-max` (код автоматично обчислить середнє значення)
- `required` - обов'язкове поле

**Формула в коді:**

```
Вартість перил = data-price × довжина_перил
або
Вартість перил = ((data-price-min + data-price-max) / 2) × довжина_перил
```

---

## Крок 3: Оберіть покриття

Формули (з ТЗ):

1. **RAL фарба:** `220 грн/м²`
2. **Порошкова покраска:** `400 грн/м²`
3. **Антикорозійне покриття:** `340 грн/м²`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="stairs-coating"
  data-quiz="stairs-coating"
  data-price="220"
  required
/>
<label>RAL фарба (220 грн/м²)</label>

<input
  type="radio"
  name="stairs-coating"
  data-quiz="stairs-coating"
  data-price="400"
  required
/>
<label>Порошкова покраска (400 грн/м²)</label>

<input
  type="radio"
  name="stairs-coating"
  data-quiz="stairs-coating"
  data-price="340"
  required
/>
<label>Антикорозійне покриття (340 грн/м²)</label>
```

**Примітки:**

- `name="stairs-coating"` - однаковий для всіх варіантів
- `data-quiz="stairs-coating"` - обов'язковий атрибут
- `data-price` - ціна покриття за м²
- `required` - обов'язкове поле

**Формула в коді:**

```
Площа сходів = ширина × довжина_перил
Вартість покриття = data-price × площа_сходів
```

---

## Крок 4: Монтаж та доставка

Формули (з ТЗ):

1. **Монтаж перил:** `2000 грн/м.п.` (чекбокс)
2. **Доставка:**
   - По Києву: `1000 грн`
   - По Київській області: `1500 грн`
   - Не потрібно: `0 грн`

**Чекбокс та радіо-кнопки:**

```html
<!-- Монтаж перил -->
<label>
  <input type="checkbox" data-quiz="stairs-railing-installation" />
  Монтаж перил (2000 грн/м.п.)
</label>

<!-- Доставка (радіо-кнопки) -->
<input
  type="radio"
  name="stairs-delivery"
  data-quiz="stairs-delivery"
  data-delivery-type="kyiv"
  required
/>
<label>Доставка по Києву (1000 грн)</label>

<input
  type="radio"
  name="stairs-delivery"
  data-quiz="stairs-delivery"
  data-delivery-type="kyiv-region"
  required
/>
<label>Доставка по Київській області (1500 грн)</label>

<input
  type="radio"
  name="stairs-delivery"
  data-quiz="stairs-delivery"
  data-delivery-type="none"
  required
/>
<label>Не потрібно</label>
```

**Примітки:**

- `data-quiz="stairs-railing-installation"` - чекбокс для монтажу перил
- `name="stairs-delivery"` - однаковий для всіх варіантів доставки
- `data-quiz="stairs-delivery"` - обов'язковий атрибут
- `data-delivery-type` - тип доставки: `"kyiv"`, `"kyiv-region"`, або `"none"`
- Доставка - обов'язковий вибір (радіо-кнопки з `required`)

**Формула в коді:**

```
Монтаж перил = 2000 × довжина_перил (якщо вибрано)
Доставка = 1000 (Київ) або 1500 (Київська область) або 0 (Не потрібно)
```

---

## Крок 5: Додаткові опції

Формули (з ТЗ):

1. **Антиковзкі накладки:** `800 грн/м.п.` з монтажем та матеріалом
2. **Підсвітка:** `480 грн/м.п.` з монтажем та матеріалом
3. **Термінове виконання:** `+20%` від загального чеку
4. **Індивідуальний дизайн:** `30000 грн`

**Чекбокси та радіо-кнопка:**

```html
<label>
  <input type="checkbox" data-quiz="stairs-anti-slip" />
  Антиковзкі накладки (800 грн/м.п. з монтажем та матеріалом)
</label>

<label>
  <input type="checkbox" data-quiz="stairs-lighting" />
  Підсвітка (480 грн/м.п. з монтажем та матеріалом)
</label>

<label>
  <input type="checkbox" data-quiz="stairs-urgent" />
  Термінове виконання (+20% від загального чеку)
</label>

<label>
  <input type="checkbox" data-quiz="stairs-custom-design" />
  Індивідуальний дизайн / нестандартні рішення (30000 грн)
</label>

<input
  type="radio"
  name="stairs-options-none"
  data-quiz="stairs-options-none"
  data-options-none="true"
/>
<label>Не потрібно</label>
```

**Примітки:**

- `data-quiz="stairs-anti-slip"` - чекбокс для антиковзких накладок
- `data-quiz="stairs-lighting"` - чекбокс для підсвітки
- `data-quiz="stairs-urgent"` - чекбокс для термінового виконання
- `data-quiz="stairs-custom-design"` - чекбокс для індивідуального дизайну
- `data-quiz="stairs-options-none"` - радіо-кнопка "Не потрібно" (скасовує всі чекбокси)
- Можна вибрати кілька чекбоксів одночасно
- Якщо вибрано "Не потрібно", всі чекбокси автоматично знімаються
- Якщо вибрано хоча б один чекбокс, "Не потрібно" автоматично знімається

**Формула в коді:**

```
Якщо вибрано "Не потрібно":
  Всі опції = 0

Якщо не вибрано "Не потрібно":
  Антиковзкі накладки = 800 × довжина_перил (якщо вибрано)
  Підсвітка = 480 × довжина_перил (якщо вибрано)
  Термінове виконання = загальна_вартість × 1.2 (якщо вибрано)
  Індивідуальний дизайн = 30000 (якщо вибрано)
```

---

## Підсумковий крок (сходи та перила)

**Елемент для відображення загальної ціни:**

```html
<div if-element="total-price">0 ₴</div>
```

**Форма для відправки даних:**

- Звичайні поля форми (ім'я, телефон, email тощо)
- Кнопка відправки: `type="submit"` (стандартна відправка Webflow)

---

## Формула розрахунку загальної вартості сходів та перил

Умовні позначення:

- `W` — ширина (м)
- `H` — висота (м)
- `S` — кількість сходинок (вводиться користувачем)
- `L` — довжина перил = `√(W² + H²)` (м)
- `A` — площа сходів = `W × L` (м²)

```
1. Вартість сходинок:
   steps = S × 4750

2. Вартість перил:
   railing = data-price_railing × L
   (або середнє значення для діапазону: (data-price-min + data-price-max) / 2 × L)

3. Вартість покриття:
   coating = data-price_coating × A

4. Монтаж перил:
   railing_installation = 2000 × L (якщо вибрано)

5. Доставка:
   delivery = 1000 (Київ)
   або
   delivery = 1500 (Київська область)
   або
   delivery = 0 (Не потрібно)

6. Додаткові опції (якщо НЕ вибрано "Не потрібно"):
   Якщо вибрано "Не потрібно":
     anti_slip = 0
     lighting = 0
     urgent = false
     custom_design = 0
   Якщо НЕ вибрано "Не потрібно":
     anti_slip = 800 × L (якщо вибрано)
     lighting = 480 × L (якщо вибрано)
     custom_design = 30000 (якщо вибрано)

7. Базова вартість:
   base_total = steps + railing + coating + railing_installation + delivery + anti_slip + lighting + custom_design

8. Термінове виконання (якщо вибрано і НЕ вибрано "Не потрібно"):
   Якщо вибрано "Не потрібно":
     total = base_total
   Якщо вибрано термінове виконання:
     total = base_total × 1.2
   Якщо не вибрано:
     total = base_total
```

---

## Підключення скрипту для сходів та перил в Webflow

1. Вставте JavaScript-код з файлу `quiz.html` для сходів (блок з коментарем `<!-- Калькулятор для сходів та перил -->`) у:

   - **Page Settings → Custom Code → Before </body>**
   - або в **Embed** блок внизу сторінки з сходами.

2. Переконайтеся, що:

   - Форма має клас `calculate_form_stairs`.
   - Обгортка форми має клас `calculate_form_stairs_block`.
   - Кроки мають клас `calc-step` та атрибут `if-step`.
   - Прогрес-бар має `if-element="progress-bar"`.
   - Елементи для цін мають `if-element="step-price"` та `if-element="total-price"`.

3. Додайте всі атрибути `data-quiz`, `data-price`, `data-price-min`, `data-price-max`, `data-delivery-type` згідно з цією інструкцією.

4. Опублікуйте сайт, щоб Webflow застосував скрипт на продакшені.

---

## Приклад повної структури в Webflow

```html
<div class="calculate_form_stairs_block">
  <!-- Progress bar -->
  <div if-element="progress-bar" class="quiz_progress-bar"></div>

  <form class="calculate_form_stairs">
    <!-- Крок 1: Розміри -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <label>Висота: <span data-quiz="height-display">2.5 м</span></label>
      <input
        type="range"
        data-quiz="height"
        min="1"
        max="5"
        value="2.5"
        step="0.1"
        required
      />

      <label>Ширина: <span data-quiz="width-display">1 м</span></label>
      <input
        type="range"
        data-quiz="width"
        min="0.5"
        max="2"
        value="1"
        step="0.1"
        required
      />

      <label
        >Кількість сходинок:
        <span data-quiz="steps-count-display">1</span></label
      >
      <input
        type="range"
        data-quiz="steps-count"
        min="1"
        max="50"
        value="1"
        step="1"
        required
      />

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 2: Тип перил -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="stairs-railing"
        data-quiz="stairs-railing"
        data-price="7500"
        required
      />
      <label>Класичні з металу (7500 грн/м.п.)</label>

      <input
        type="radio"
        name="stairs-railing"
        data-quiz="stairs-railing"
        data-price-min="8000"
        data-price-max="9000"
        required
      />
      <label>Метал + дерево (8000-9000 грн/м.п.)</label>

      <input
        type="radio"
        name="stairs-railing"
        data-quiz="stairs-railing"
        data-price-min="12000"
        data-price-max="13000"
        required
      />
      <label>Скло + метал (12000-13000 грн/м.п.)</label>

      <input
        type="radio"
        name="stairs-railing"
        data-quiz="stairs-railing"
        data-price-min="4000"
        data-price-max="4600"
        required
      />
      <label>З нержавіючої сталі (4000-4600 грн/м.п.)</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 3: Покриття -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="stairs-coating"
        data-quiz="stairs-coating"
        data-price="220"
        required
      />
      <label>RAL фарба (220 грн/м²)</label>

      <input
        type="radio"
        name="stairs-coating"
        data-quiz="stairs-coating"
        data-price="400"
        required
      />
      <label>Порошкова покраска (400 грн/м²)</label>

      <input
        type="radio"
        name="stairs-coating"
        data-quiz="stairs-coating"
        data-price="340"
        required
      />
      <label>Антикорозійне покриття (340 грн/м²)</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 4: Монтаж та доставка -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <label>
        <input type="checkbox" data-quiz="stairs-railing-installation" />
        Монтаж перил (2000 грн/м.п.)
      </label>

      <input
        type="radio"
        name="stairs-delivery"
        data-quiz="stairs-delivery"
        data-delivery-type="kyiv"
        required
      />
      <label>Доставка по Києву (1000 грн)</label>

      <input
        type="radio"
        name="stairs-delivery"
        data-quiz="stairs-delivery"
        data-delivery-type="kyiv-region"
        required
      />
      <label>Доставка по Київській області (1500 грн)</label>

      <input
        type="radio"
        name="stairs-delivery"
        data-quiz="stairs-delivery"
        data-delivery-type="none"
        required
      />
      <label>Не потрібно</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 5: Додаткові опції -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <label>
        <input type="checkbox" data-quiz="stairs-anti-slip" />
        Антиковзкі накладки (800 грн/м.п. з монтажем та матеріалом)
      </label>

      <label>
        <input type="checkbox" data-quiz="stairs-lighting" />
        Підсвітка (480 грн/м.п. з монтажем та матеріалом)
      </label>

      <label>
        <input type="checkbox" data-quiz="stairs-urgent" />
        Термінове виконання (+20% від загального чеку)
      </label>

      <label>
        <input type="checkbox" data-quiz="stairs-custom-design" />
        Індивідуальний дизайн / нестандартні рішення (30000 грн)
      </label>

      <input
        type="radio"
        name="stairs-options-none"
        data-quiz="stairs-options-none"
        data-options-none="true"
      />
      <label>Не потрібно</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 8: Підсумок -->
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

## Важливо

1. Всі кроки мають клас `calc-step` та атрибут `if-step`
2. Радіо-кнопки в межах одного кроку мають однаковий `name`
3. Ціни вказуються в атрибутах `data-price`, `data-price-min`, `data-price-max` (число без символів)
4. Останній крок автоматично показує розраховану загальну ціну
5. Форма відправляється стандартним способом Webflow (POST)
6. Розрахунок відбувається в реальному часі при зміні будь-яких параметрів
7. Весь `quiz_option_item` клікабельний - не потрібно клікати саме на іконку радіо-кнопки
8. Кількість сходинок вводиться користувачем вручну (поле `data-quiz="steps-count"`)
9. Довжина перил розраховується автоматично: `√(ширина² + висота²)`
10. Для типів перил з діапазоном цін використовується середнє значення
11. Термінове виконання (+20%) застосовується до всієї суми перед додаванням доставки
