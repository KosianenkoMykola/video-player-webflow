# Інструкція з налаштування квізу розрахунку вартості паркану в Webflow

## Структура кроків

Квіз паркану має 7 кроків:

1. **Розміри паркану** (повзунки для висоти та довжини)
2. **Тип металу** (радіо-кнопки)
3. **Вид паркану** (радіо-кнопки)
4. **Покриття** (радіо-кнопки)
5. **Монтаж** (чекбокси)
6. **Додаткові опції** (чекбокси та поля вводу)
7. **Підсумок з ціною та формою**

---

## Атрибути для HTML елементів

### Загальна структура

**Форма:**

- Клас: `calculate_form_fence`

**Контейнер форми:**

- Клас: `calculate_form_fence_block`

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

## Крок 1: Розміри паркану (висота, довжина)

**Висота:**

```html
<input
  type="range"
  data-quiz="height"
  min="1"
  max="5"
  value="2"
  step="0.1"
  required
/>
<span data-quiz="height-display">2 м</span>
```

**Довжина:**

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

**Примітки:**

- Використовуйте `type="range"` для повзунків
- `data-quiz="height-display"` та `data-quiz="length-display"` - елементи для відображення поточних значень
- Встановіть `min`, `max`, `value` та `step` відповідно до ваших потреб
- Обидва поля мають бути `required`

---

## Крок 2: Оберіть тип металу

Формули (з ТЗ):

1. **Економ:** `210 грн/м² + 450 грн/м² (матеріал заповнення) = 660 грн/м²`
2. **Стандарт:** `280 грн/м² + 450 грн/м² (матеріал заповнення) = 730 грн/м²`
3. **Преміум:** `345 грн/м² + 450 грн/м² (матеріал заповнення) = 795 грн/м²`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="fence-metal"
  data-quiz="fence-metal"
  data-price="210"
  required
/>
<label>Економ (210 грн/м² + 450 грн/м² матеріал)</label>

<input
  type="radio"
  name="fence-metal"
  data-quiz="fence-metal"
  data-price="280"
  required
/>
<label>Стандарт (280 грн/м² + 450 грн/м² матеріал)</label>

<input
  type="radio"
  name="fence-metal"
  data-quiz="fence-metal"
  data-price="345"
  required
/>
<label>Преміум (345 грн/м² + 450 грн/м² матеріал)</label>
```

**Примітки:**

- `name="fence-metal"` - однаковий для всіх варіантів
- `data-quiz="fence-metal"` - обов'язковий атрибут
- `data-price` - ціна типу металу за м² (450 грн/м² матеріал додається автоматично)

**Формула в коді:**

```
Вартість типу металу = (data-price + 450) × (висота × довжина)
```

---

## Крок 3: Оберіть вид паркану

Формули (з ТЗ):

1. **Профнастил стандарт:** `1300 грн/м²`
2. **Жалюзі Класік:** `1800 грн/м²`
3. **Жалюзі Престиж:** `2100 грн/м²`
4. **Жалюзі Ексклюзив:** `3000 грн/м²`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="fence-type"
  data-quiz="fence-type"
  data-price="1300"
  required
/>
<label>Профнастил стандарт з матеріалом та монтажем (1300 грн/м²)</label>

<input
  type="radio"
  name="fence-type"
  data-quiz="fence-type"
  data-price="1800"
  required
/>
<label
  >Жалюзі Класік покриття одношарове з матеріалом та монтажем (1800
  грн/м²)</label
>

<input
  type="radio"
  name="fence-type"
  data-quiz="fence-type"
  data-price="2100"
  required
/>
<label>Жалюзі Престиж з матеріалом та монтажем (2100 грн/м²)</label>

<input
  type="radio"
  name="fence-type"
  data-quiz="fence-type"
  data-price="3000"
  required
/>
<label>Жалюзі Ексклюзив (3000 грн/м²)</label>
```

**Примітки:**

- `name="fence-type"` - однаковий для всіх варіантів
- `data-quiz="fence-type"` - обов'язковий атрибут
- `data-price` - ціна виду паркану за м²

**Формула в коді:**

```
Вартість виду паркану = data-price × (висота × довжина)
```

---

## Крок 4: Оберіть покриття

Формули (з ТЗ):

1. **RAL фарба:** `220 грн/м²`
2. **Порошкова покраска:** `400 грн/м²`
3. **Антикорозійне покриття:** `340 грн/м²`

**Радіо-кнопки:**

```html
<input
  type="radio"
  name="fence-coating"
  data-quiz="fence-coating"
  data-price="220"
  required
/>
<label>RAL фарба (220 грн/м²)</label>

<input
  type="radio"
  name="fence-coating"
  data-quiz="fence-coating"
  data-price="400"
  required
/>
<label>Порошкова покраска (400 грн/м²)</label>

<input
  type="radio"
  name="fence-coating"
  data-quiz="fence-coating"
  data-price="340"
  required
/>
<label>Антикорозійне покриття (340 грн/м²)</label>
```

**Примітки:**

- `name="fence-coating"` - однаковий для всіх варіантів
- `data-quiz="fence-coating"` - обов'язковий атрибут
- `data-price` - ціна покриття за м²

**Формула в коді:**

```
Вартість покриття = data-price × (висота × довжина)
```

---

## Крок 5: Монтаж

Формули (з ТЗ):

1. **Фундамент під паркан:** `600 грн/м.п.`
2. **Монтаж паркану:** `800 грн/м²`
3. **Монтаж паркану і фундамент:** обидві послуги
4. **Не потрібно:** без монтажу

**Радіо-кнопки (вибір однієї опції):**

```html
<input
  type="radio"
  name="fence-installation"
  data-quiz="fence-installation"
  data-installation-mode="foundation"
  required
/>
<label>Фундамент під паркан 30см×70см (600 грн/м.п.)</label>

<input
  type="radio"
  name="fence-installation"
  data-quiz="fence-installation"
  data-installation-mode="installation"
  required
/>
<label>Монтаж паркану (800 грн/м²)</label>

<input
  type="radio"
  name="fence-installation"
  data-quiz="fence-installation"
  data-installation-mode="both"
  required
/>
<label>Монтаж паркану і фундамент</label>

<input
  type="radio"
  name="fence-installation"
  data-quiz="fence-installation"
  data-installation-mode="none"
  required
/>
<label>Не потрібно</label>
```

**Примітки:**

- `name="fence-installation"` - однаковий для всіх варіантів
- `data-quiz="fence-installation"` - обов'язковий атрибут
- `data-installation-mode` - режим монтажу: `"foundation"`, `"installation"`, `"both"`, або `"none"`
- `required` - обов'язкове поле

**Формула в коді:**

```
Якщо mode = "foundation": Фундамент = 600 × довжина
Якщо mode = "installation": Монтаж = 800 × (висота × довжина)
Якщо mode = "both": Фундамент + Монтаж
Якщо mode = "none": 0
```

---

## Крок 6: Додаткові опції

Формули (з ТЗ):

1. **Доставка по Києву:** `1000 грн`
2. **Доставка по Київській області:** `1500 грн`
3. **Автоматика:** `24000 грн` з матеріалом та монтажем
4. **Кладка цегляних стовпців:** `3200 грн за стовпчик` (2 метри висотою)
5. **Термінове виконання:** `+20% від загального чеку`
6. **Не потрібно:** без доставки

**Радіо-кнопки для доставки та чекбокси для інших опцій:**

```html
<!-- Доставка (радіо-кнопки) -->
<input
  type="radio"
  name="fence-delivery"
  data-quiz="fence-delivery"
  data-delivery-type="kyiv"
  required
/>
<label>Доставка по Києву (1000 грн)</label>

<input
  type="radio"
  name="fence-delivery"
  data-quiz="fence-delivery"
  data-delivery-type="kyiv-region"
  required
/>
<label>Доставка по Київській області (1500 грн)</label>

<input
  type="radio"
  name="fence-delivery"
  data-quiz="fence-delivery"
  data-delivery-type="none"
  required
/>
<label>Не потрібно</label>

<!-- Автоматика (чекбокс) -->
<label>
  <input type="checkbox" data-quiz="fence-automation" />
  Автоматика (24000 грн з матеріалом та монтажем)
</label>

<!-- Цегляні стовпці (поле вводу) -->
<label>Кількість цегляних стовпців:</label>
<input type="number" data-quiz="fence-pillars" min="0" value="0" step="1" />
<span>× 3200 грн</span>

<!-- Термінове виконання (чекбокс) -->
<label>
  <input type="checkbox" data-quiz="fence-urgent" />
  Термінове виконання (+20% від загального чеку)
</label>
```

**Примітки:**

- `name="fence-delivery"` - однаковий для всіх варіантів доставки
- `data-quiz="fence-delivery"` - обов'язковий атрибут для доставки
- `data-delivery-type` - тип доставки: `"kyiv"`, `"kyiv-region"`, або `"none"`
- `data-quiz="fence-automation"` - чекбокс для автоматики
- `data-quiz="fence-pillars"` - поле для кількості стовпців
- `data-quiz="fence-urgent"` - чекбокс для термінового виконання
- Доставка - обов'язковий вибір (радіо-кнопки з `required`)
- Інші опції - необов'язкові (можна вибрати кілька)

**Формула в коді:**

```
Доставка = 1000 (Київ) або 1500 (Київська область) або 0 (Не потрібно)
Автоматика = 24000 (якщо вибрано)
Цегляні стовпці = 3200 × кількість
Термінове виконання = загальна_сума × 1.2 (якщо вибрано)
```

---

## Підсумковий крок (паркан)

**Елемент для відображення загальної ціни:**

```html
<div if-element="total-price">0 ₴</div>
```

**Форма для відправки даних:**

- Звичайні поля форми (ім'я, телефон, email тощо)
- Кнопка відправки: `type="submit"` (стандартна відправка Webflow)

---

## Формула розрахунку загальної вартості паркану

Умовні позначення:

- `H` — висота (м)
- `L` — довжина (м)
- `S` — площа = `H × L` (м²)

```
1. Тип металу + матеріал заповнення:
   metal_by_area = (data-price_metal + 450) × S
   metal_by_length = 2000 × L (мінімальна вартість за погонний метр)
   metal = max(metal_by_area, metal_by_length)

2. Вид паркану:
   fence_type = data-price_fence × S

3. Покриття:
   coating = data-price_coating × S

4. Монтаж та фундамент (вибір однієї опції):
   Якщо mode = "foundation": foundation = 600 × L
   Якщо mode = "installation": installation = 800 × S
   Якщо mode = "both": foundation = 600 × L + installation = 800 × S
   Якщо mode = "none": 0

6. Цегляні стовпці:
   pillars = 3200 × кількість

7. Автоматика:
   automation = 24000 (якщо вибрано)

8. Базова вартість:
   base_total = metal + fence_type + coating + installation + foundation + pillars + automation

9. Термінове виконання (якщо вибрано):
   total_with_urgent = base_total × 1.2
   або
   total_with_urgent = base_total (якщо не вибрано)

10. Доставка:
    delivery = 1000 (Київ)
    або
    delivery = 1500 (Київська область)
    або
    delivery = 0 (Не потрібно)

11. Загальна вартість паркану:
    total = total_with_urgent + delivery
```

---

## Підключення скрипту для паркану в Webflow

1. Вставте JavaScript-код з файлу `quiz.html` для паркану (блок з коментарем `<!-- Калькулятор для парканів -->`) у:

   - **Page Settings → Custom Code → Before </body>**
   - або в **Embed** блок внизу сторінки з парканом.

2. Переконайтеся, що:

   - Форма має клас `calculate_form_fence`.
   - Обгортка форми має клас `calculate_form_fence_block`.
   - Кроки мають клас `calc-step` та атрибут `if-step`.
   - Прогрес-бар має `if-element="progress-bar"`.
   - Елементи для цін мають `if-element="step-price"` та `if-element="total-price"`.

3. Додайте всі атрибути `data-quiz`, `data-price`, `data-delivery-type` згідно з цією інструкцією.

4. Опублікуйте сайт, щоб Webflow застосував скрипт на продакшені.

---

## Приклад повної структури в Webflow

```html
<div class="calculate_form_fence_block">
  <!-- Progress bar -->
  <div if-element="progress-bar" class="quiz_progress-bar"></div>

  <form class="calculate_form_fence">
    <!-- Крок 1: Розміри -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <label>Висота: <span data-quiz="height-display">2 м</span></label>
      <input
        type="range"
        data-quiz="height"
        min="1"
        max="5"
        value="2"
        step="0.1"
        required
      />

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

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 2: Тип металу -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="fence-metal"
        data-quiz="fence-metal"
        data-price="210"
        required
      />
      <label>Економ (210 грн/м² + 450 грн/м² матеріал)</label>

      <input
        type="radio"
        name="fence-metal"
        data-quiz="fence-metal"
        data-price="280"
        required
      />
      <label>Стандарт (280 грн/м² + 450 грн/м² матеріал)</label>

      <input
        type="radio"
        name="fence-metal"
        data-quiz="fence-metal"
        data-price="345"
        required
      />
      <label>Преміум (345 грн/м² + 450 грн/м² матеріал)</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 3: Вид паркану -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="fence-type"
        data-quiz="fence-type"
        data-price="1300"
        required
      />
      <label>Профнастил стандарт (1300 грн/м²)</label>

      <input
        type="radio"
        name="fence-type"
        data-quiz="fence-type"
        data-price="1800"
        required
      />
      <label>Жалюзі Класік (1800 грн/м²)</label>

      <input
        type="radio"
        name="fence-type"
        data-quiz="fence-type"
        data-price="2100"
        required
      />
      <label>Жалюзі Престиж (2100 грн/м²)</label>

      <input
        type="radio"
        name="fence-type"
        data-quiz="fence-type"
        data-price="3000"
        required
      />
      <label>Жалюзі Ексклюзив (3000 грн/м²)</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 4: Покриття -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="fence-coating"
        data-quiz="fence-coating"
        data-price="220"
        required
      />
      <label>RAL фарба (220 грн/м²)</label>

      <input
        type="radio"
        name="fence-coating"
        data-quiz="fence-coating"
        data-price="400"
        required
      />
      <label>Порошкова покраска (400 грн/м²)</label>

      <input
        type="radio"
        name="fence-coating"
        data-quiz="fence-coating"
        data-price="340"
        required
      />
      <label>Антикорозійне покриття (340 грн/м²)</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 5: Монтаж -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <input
        type="radio"
        name="fence-installation"
        data-quiz="fence-installation"
        data-installation-mode="foundation"
        required
      />
      <label>Фундамент під паркан 30см×70см (600 грн/м.п.)</label>

      <input
        type="radio"
        name="fence-installation"
        data-quiz="fence-installation"
        data-installation-mode="installation"
        required
      />
      <label>Монтаж паркану (800 грн/м²)</label>

      <input
        type="radio"
        name="fence-installation"
        data-quiz="fence-installation"
        data-installation-mode="both"
        required
      />
      <label>Монтаж паркану і фундамент</label>

      <input
        type="radio"
        name="fence-installation"
        data-quiz="fence-installation"
        data-installation-mode="none"
        required
      />
      <label>Не потрібно</label>

      <button if-element="button-next">Далі</button>
    </div>

    <!-- Крок 6: Додаткові опції -->
    <div class="calc-step" if-step>
      <div if-element="step-price">0 ₴</div>

      <!-- Доставка (радіо-кнопки) -->
      <input
        type="radio"
        name="fence-delivery"
        data-quiz="fence-delivery"
        data-delivery-type="kyiv"
        required
      />
      <label>Доставка по Києву (1000 грн)</label>

      <input
        type="radio"
        name="fence-delivery"
        data-quiz="fence-delivery"
        data-delivery-type="kyiv-region"
        required
      />
      <label>Доставка по Київській області (1500 грн)</label>

      <input
        type="radio"
        name="fence-delivery"
        data-quiz="fence-delivery"
        data-delivery-type="none"
        required
      />
      <label>Не потрібно</label>

      <!-- Автоматика (чекбокс) -->
      <label>
        <input type="checkbox" data-quiz="fence-automation" />
        Автоматика (24000 грн з матеріалом та монтажем)
      </label>

      <!-- Цегляні стовпці (поле вводу) -->
      <label>Кількість цегляних стовпців:</label>
      <input
        type="number"
        data-quiz="fence-pillars"
        min="0"
        value="0"
        step="1"
      />
      <span>× 3200 грн</span>

      <!-- Термінове виконання (чекбокс) -->
      <label>
        <input type="checkbox" data-quiz="fence-urgent" />
        Термінове виконання (+20% від загального чеку)
      </label>

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

## Важливо

1. Всі кроки мають клас `calc-step` та атрибут `if-step`
2. Радіо-кнопки в межах одного кроку мають однаковий `name`
3. Ціни вказуються в атрибуті `data-price` (число без символів)
4. Останній крок автоматично показує розраховану загальну ціну
5. Форма відправляється стандартним способом Webflow (POST)
6. Розрахунок відбувається в реальному часі при зміні будь-яких параметрів
7. Весь `quiz_option_item` клікабельний - не потрібно клікати саме на іконку радіо-кнопки
8. Доставка - обов'язковий вибір (радіо-кнопки з `required`)
9. Термінове виконання (+20%) застосовується до всієї суми перед додаванням доставки
