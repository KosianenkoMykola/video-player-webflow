# NiceGuysPlayer for Webflow (multi‑player, responsive)

Made with ❤️ by [Nice Guys Digital Design Agency](https://www.niceguys.agency/)

## Важливі оновлення (UA)
- Рекомендуємо завантажувати відео на Cloudinary і використовувати прямі `.mp4` посилання (наприклад: `https://res.cloudinary.com/<cloud>/video/upload/.../file.mp4`).
- Плеєрів на сторінці може бути декілька. Дублюємо тільки HTML‑блок плеєра; CSS та JS мають бути в одному екземплярі.
- Для кожного плеєра можна задати посилання через `data-video-src` на контейнері `div.glass-player`. Також підтримується глобальний параметр `?src=...` у URL, який перекриває всі плеєри.
- Адаптив: використовується `aspect-ratio` для висоти плеєра та `object-fit: contain` для коректного відображення вертикальних відео. На мобільних піднято `progress-bar` вище.
- iPhone: кнопка «fullscreen» використовує `video.webkitEnterFullscreen()`; Picture‑in‑Picture приховано на iOS і доступно на платформах, де підтримується.

## Important updates (EN)
- Prefer hosting videos on Cloudinary and use direct `.mp4` URLs.
- Multiple players per page are supported. Duplicate only the HTML block per player; keep CSS and JS as a single instance on the page.
- Per‑player source via `data-video-src` on `.glass-player`. Global `?src=...` in page URL overrides per‑player sources.
- Responsive: `aspect-ratio` fixes player height; `object-fit: contain` centers video; progress bar raised on mobile breakpoints.
- iOS fullscreen uses `video.webkitEnterFullscreen()`; PiP is enabled where supported and hidden on iOS.

## Використання через iframe (рекомендовано)

```html
<iframe src="https://your-vercel-domain.vercel.app/?src=YOUR_VIDEO_LINK" width="100%" height="400" frameborder="0" allowfullscreen></iframe>
```
Замініть `YOUR_VIDEO_LINK` на пряме посилання на .mp4/.webm відео.

---

## Використання як div (Embed) у Webflow

> Рекомендація: створіть 3 окремі Embed Code блоки — 1) HTML, 2) CSS, 3) JS.

### Приклад структури у Webflow:

![Webflow Embed Structure](./video/Screenshot%202025-07-18%20at%2015.27.22.png)

### Як виглядає плеєр:

![Player Example](./video/Screenshot%202025-07-18%20at%2015.27.41.png)

---

### 1. HTML (Embed Code Block 1)
```html
<div class="glass-player" style="--player-aspect: 16/9" data-video-src="https://res.cloudinary.com/<cloud>/video/upload/.../file.mp4">
  <a class="niceguys-label" id="niceguysLabel" href="https://www.niceguys.agency/" target="_blank" rel="noopener noreferrer">NiceGuysPlayer</a>
  <video id="myVideo" preload="metadata" playsinline></video>
  <button class="center-play" id="centerPlayBtn" aria-label="Play">…</button>
  <div class="controls visible" id="controlsBar">
    <div class="controls-container">
      <button class="glass-btn" id="rewindBtn" title="Back 15s">…</button>
      <button class="glass-btn" id="playPauseBtn" title="Play/Pause">…</button>
      <button class="glass-btn" id="forwardBtn" title="Forward 15s">…</button>
    </div>
    <div class="controls-container">
      <div class="volume-container">
        <input type="range" min="0" max="1" step="0.01" value="1" class="volume-slider" id="volumeSlider" title="Volume">
        <button class="glass-btn" id="muteBtn" title="Mute/Unmute">…</button>
      </div>
      <button class="glass-btn" id="pipBtn" title="Picture in Picture">…</button>
      <button class="glass-btn" id="fullscreenBtn" title="Fullscreen">…</button>
    </div>
  </div>
  <div class="progress-bar" id="progressBar"><div class="progress" id="progress"></div></div>
  <div class="time" id="timeDisplay">00:00 / 00:00</div>
</div>
```

Примітки:
- Дублюйте цей HTML для кожного плеєра. CSS та JS — один раз на сторінці.
- Для вертикальних відео задайте `style="--player-aspect: 9/16"`.

### 2. CSS (Embed Code Block 2)
```css
/* Використайте вміст style.css з репозиторію. Головне: */
.glass-player { aspect-ratio: var(--player-aspect, 16/9); }
.glass-player video { height: 100%; object-fit: contain; object-position: center; }
/* Breakpoints ≤991px, ≤767px, ≤479px; progress-bar піднятий на мобільних */
```

### 3. JS (Embed Code Block 3)
```html
<script>
  // Ініціалізує всі .glass-player, працює у scope контейнера,
  // ставить на паузу інші плеєри при відтворенні одного,
  // бере джерело з ?src або data-video-src, має iOS fullscreen та PiP (де можливо).
  // Повний актуальний код див. у index.html цього репозиторію.
</script>
```

**Увага:**
- На сторінці може бути декілька плеєрів — дублюйте тільки HTML. CSS/JS лишайте один раз.
- Рекомендуємо Cloudinary для стабільних `.mp4` посилань.
- Вертикальні відео: задавайте `--player-aspect: 9/16` на контейнері.

---

## English

### Usage via iframe (recommended)

```html
<iframe src="https://your-vercel-domain.vercel.app/?src=YOUR_VIDEO_LINK" width="100%" height="400" frameborder="0" allowfullscreen></iframe>
```
Replace `YOUR_VIDEO_LINK` with a direct .mp4/.webm link.

### Usage as div (Embed) in Webflow

- Duplicate only the HTML block per player. Keep CSS/JS single.
- Prefer Cloudinary direct `.mp4` URLs.
- Per‑player source via `data-video-src`; global override via `?src=...`.
- Vertical videos: set `--player-aspect: 9/16`.