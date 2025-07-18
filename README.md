# NiceGuysPlayer for Webflow (Vercel-ready)

## Використання через iframe (рекомендовано)

```html
<iframe src="https://your-vercel-domain.vercel.app/?src=YOUR_VIDEO_LINK" width="100%" height="400" frameborder="0" allowfullscreen></iframe>
```
Замініть `YOUR_VIDEO_LINK` на пряме посилання на .mp4/.webm відео.

---

## Використання як div (Embed) у Webflow

1. Додайте Embed-блок у Webflow на потрібну сторінку.
2. Скопіюйте HTML-код плеєра (див. index.html, частина 1) у Embed.
3. Додайте CSS-стилі (вміст style.css) у `<style>...</style>` у Embed або у налаштування сторінки.
4. Додайте JS-код (див. index.html, частина 3) у `<script>...</script>` у Embed або у налаштування сторінки.
5. За потреби змініть src відео у `<video>` або зробіть динамічним через CMS.

**Увага:**
- Якщо вставляєте як div, параметр src через URL не працює — потрібно змінювати src у коді або через CMS.
- Для повної функціональності потрібні всі три частини: HTML, CSS, JS.

---

## English

### Usage via iframe (recommended)

```html
<iframe src="https://your-vercel-domain.vercel.app/?src=YOUR_VIDEO_LINK" width="100%" height="400" frameborder="0" allowfullscreen></iframe>
```
Replace `YOUR_VIDEO_LINK` with a direct .mp4/.webm link.

### Usage as div (Embed) in Webflow

1. Add an Embed block in Webflow.
2. Copy the HTML code (see index.html, part 1) into the Embed.
3. Add CSS (from style.css) in a `<style>...</style>` tag in the Embed or in page settings.
4. Add JS (see index.html, part 3) in a `<script>...</script>` tag in the Embed or in page settings.
5. Change the video src in `<video>` as needed or make it dynamic via CMS.

**Note:**
- If you use as div, the src parameter in URL will not work — you must set the src in code or via CMS.
- All three parts (HTML, CSS, JS) are required for full functionality. 