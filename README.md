# NiceGuysPlayer for Webflow (Vercel-ready)

### Авторство / Credits

Made with ❤️ by [Nice Guys Digital Design Agency](https://www.niceguys.agency/) 

## Використання через iframe (рекомендовано)

```html
<iframe src="https://your-vercel-domain.vercel.app/?src=YOUR_VIDEO_LINK" width="100%" height="400" frameborder="0" allowfullscreen></iframe>
```
Замініть `YOUR_VIDEO_LINK` на пряме посилання на .mp4/.webm відео.

---

## Використання як div (Embed) у Webflow

> **Рекомендація:** Для найкращої організації створіть **3 окремі Embed Code блоки** у Webflow:
> 1. Перший — для HTML (структура плеєра)
> 2. Другий — для CSS (стилі у `<style>...</style>`)
> 3. Третій — для JS (скрипт у `<script>...</script>`)

### Приклад структури у Webflow:

![Webflow Embed Structure](./video/Screenshot%202025-07-18%20at%2015.27.22.png)

### Як виглядає плеєр:

![Player Example](./video/Screenshot%202025-07-18%20at%2015.27.41.png)

---

### 1. HTML (Embed Code Block 1)
```html
<div class="glass-player" id="playerContainer">
  <a class="niceguys-label" id="niceguysLabel" href="https://www.niceguys.agency/" target="_blank" rel="noopener noreferrer">NiceGuysPlayer</a>
  <video id="myVideo" src="https://www.w3schools.com/html/mov_bbb.mp4" preload="metadata"></video>
  <button class="center-play" id="centerPlayBtn" aria-label="Play">
    <svg width="48" height="48" viewBox="0 0 48 48" fill="none">
      <circle cx="24" cy="24" r="24" fill="rgba(255,255,255,0.25)"/>
      <polygon points="18,14 36,24 18,34" fill="currentColor"/>
    </svg>
  </button>
  <div class="controls visible" id="controlsBar">
    <div class="controls-container">
      <button class="glass-btn" id="rewindBtn" title="Back 15s">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
        <path d="M12 5V1L7 6l5 5V7c3.31 0 6 2.69 6 6s-2.69 6-6 6-6-2.69-6-6" stroke="currentColor" stroke-width="2" fill="none"/>
      </svg>
    </button>
    <button class="glass-btn" id="playPauseBtn" title="Play/Pause">
      <svg id="playIcon" width="24" height="24" viewBox="0 0 24 24" fill="none">
        <polygon points="6,4 20,12 6,20" fill="currentColor"/>
      </svg>
      <svg id="pauseIcon" width="24" height="24" viewBox="0 0 24 24" fill="none" style="display:none;">
        <rect x="6" y="4" width="4" height="16" fill="currentColor"/>
        <rect x="14" y="4" width="4" height="16" fill="currentColor"/>
      </svg>
    </button>
    <button class="glass-btn" id="forwardBtn" title="Forward 15s">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
        <path d="M12 5V1l5 5-5 5V7c-3.31 0-6 2.69-6 6s2.69 6 6 6 6-2.69 6-6" stroke="currentColor" stroke-width="2" fill="none"/>
      </svg>
    </button>
    </div>
    <div class="controls-container">
      <div class="volume-container">
      <input type="range" min="0" max="1" step="0.01" value="1" class="volume-slider" id="volumeSlider" title="Volume">
    <button class="glass-btn" id="muteBtn" title="Mute/Unmute">
      <svg id="volumeIcon" width="24" height="24" viewBox="0 0 24 24" fill="none">
        <polygon points="5,9 9,9 13,5 13,19 9,15 5,15" fill="currentColor"/>
        <path d="M16 8c1.5 1.5 1.5 6 0 7.5" stroke="currentColor" stroke-width="2" fill="none"/>
      </svg>
      <svg id="muteIcon" width="24" height="24" viewBox="0 0 24 24" fill="none" style="display:none;">
        <polygon points="5,9 9,9 13,5 13,19 9,15 5,15" fill="currentColor"/>
        <line x1="17" y1="7" x2="21" y2="17" stroke="currentColor" stroke-width="2"/>
        <line x1="21" y1="7" x2="17" y2="17" stroke="currentColor" stroke-width="2"/>
      </svg>
    </button>
    </div>
    <button class="glass-btn" id="fullscreenBtn" title="Fullscreen">
      <svg width="24" height="24" viewBox="0 0 24 24" fill="none">
        <polyline points="4,8 4,4 8,4" stroke="currentColor" stroke-width="2" fill="none"/>
        <polyline points="16,4 20,4 20,8" stroke="currentColor" stroke-width="2" fill="none"/>
        <polyline points="20,16 20,20 16,20" stroke="currentColor" stroke-width="2" fill="none"/>
        <polyline points="8,20 4,20 4,16" stroke="currentColor" stroke-width="2" fill="none"/>
      </svg>
    </button>
    </div>
  </div>
  <div class="progress-bar" id="progressBar">
    <div class="progress" id="progress"></div>
  </div>
  <div class="time" id="timeDisplay">00:00 / 00:00</div>
</div>
```

### 2. CSS (Embed Code Block 2)
```css
.glass-player {
  position: relative;
  backdrop-filter: blur(16px) saturate(180%);
  -webkit-backdrop-filter: blur(16px) saturate(180%);
  border-radius: 12px;
  box-shadow: 0 8px 32px 0 rgba(31, 38, 135, 0.18);
  padding: 0;
  width: 100%;
  text-align: center;
  overflow: hidden;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
}
.niceguys-label {
  position: absolute;
  right: 12px;
  bottom: 4px;
  z-index: 100000;
  color: #ffaf3fa4;
  font-size: 12px;
  font-weight: 600;
  letter-spacing: 1px;
  padding: 4px 16px;
  user-select: none;
  font-family: 'Manrope', sans-serif;
  opacity: 1;
  transition: opacity 0.3s;
  text-decoration: none;
  cursor: pointer;
}
.glass-player video {
  width: 100%;
  border-radius: 12px;
  background: #000;
  box-shadow: 0 4px 16px rgba(0,0,0,0.12);
  display: block;
}
.controls {
  position: absolute;
  left: 24px;
  right: 24px;
  bottom: 48px;
  display: flex;
  justify-content: space-between;
  align-items: center;
  gap: 18px;
  z-index: 2;
  opacity: 1;
  transition: opacity 0.3s;
  pointer-events: none;
}
.controls.visible {
  opacity: 1;
  pointer-events: auto;
}
.controls.hidden {
  opacity: 0;
  pointer-events: none;
}
.glass-btn {
  background: rgba(255,255,255,0.35);
  border: 1px solid rgba(255,255,255,0.4);
  border-radius: 50%;
  box-shadow: 0 2px 8px rgba(31,38,135,0.10);
  color: #222;
  font-size: 22px;
  width: 48px;
  height: 48px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.2s, box-shadow 0.2s;
  outline: none;
  backdrop-filter: blur(8px);
  border: none;
}
.glass-btn svg {
  display: block;
  width: 24px;
  height: 24px;
}
.glass-btn:hover {
  background: rgba(255,255,255,0.55);
  box-shadow: 0 4px 16px rgba(31,38,135,0.18);
}
.volume-slider {
  width: 90px;
  margin: 0 10px;
  accent-color: #ffaf3fa4;
  vertical-align: middle;
}
.progress-bar {
  margin: 0 auto;
  position: absolute;
  left: 0;
  right: 0;
  bottom: 28px;
  width: 98%;
  height: 8px;
  background: rgba(255, 255, 255, 0.25);
  border-radius: 20px;
  cursor: pointer;
  z-index: 2;
  overflow: hidden;
  opacity: 1;
  transition: opacity 0.3s;
}
.progress-bar.hidden {
  opacity: 0;
  pointer-events: none;
}
.progress {
  height: 100%;
  background: linear-gradient(90deg, #FFA500 0%, #FFB347 100%);
  border-radius: 8px;
  width: 0%;
  transition: width 0.1s;
}
.time {
  position: absolute;
  font-family: 'Manrope', sans-serif;
  left: 24px;
  bottom: 8px;
  font-size: 14px;
  color: #fff;
  margin-top: 4px;
  letter-spacing: 0.5px;
  text-shadow: 0 2px 8px rgba(0,0,0,0.5);
  z-index: 2;
  pointer-events: none;
  opacity: 1;
  transition: opacity 0.3s;
}
.time.hidden {
  opacity: 0;
}
.center-play {
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%,-50%);
  z-index: 3;
  background: rgba(255,255,255,0.35);
  border: 1px solid rgba(255,255,255,0.4);
  border-radius: 50%;
  box-shadow: 0 2px 8px rgba(31,38,135,0.10);
  width: 80px;
  height: 80px;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  transition: background 0.2s, box-shadow 0.2s;
  outline: none;
  backdrop-filter: blur(8px);
  font-size: 40px;
  color: #ffaf3fa4;
  opacity: 1;
  border: none;
}
.center-play svg {
  width: 48px;
  height: 48px;
}
.center-play.hidden {
  opacity: 0;
  pointer-events: none;
  transition: opacity 0.3s;
}
body {
  background: linear-gradient(135deg, #e0e7ef 0%, #cfd9df 100%);
  min-height: 100vh;
  display: flex;
  align-items: center;
  justify-content: center;
}
.volume-container {
  display: flex;
  align-items: center;
  justify-content: space-between;
  background: rgba(255,255,255,0.20);
  border-radius: 50px;
  display: flex;
}
.controls-container {
  display: flex;
  align-items: center;
  gap: 10px;
  justify-content: space-between;
} 
```

### 3. JS (Embed Code Block 3)
```html
<script>
  const playerContainer = document.getElementById('playerContainer');
  const video = document.getElementById('myVideo');
  const playPauseBtn = document.getElementById('playPauseBtn');
  const rewindBtn = document.getElementById('rewindBtn');
  const forwardBtn = document.getElementById('forwardBtn');
  const fullscreenBtn = document.getElementById('fullscreenBtn');
  const muteBtn = document.getElementById('muteBtn');
  const volumeSlider = document.getElementById('volumeSlider');
  const progressBar = document.getElementById('progressBar');
  const progress = document.getElementById('progress');
  const timeDisplay = document.getElementById('timeDisplay');
  const centerPlayBtn = document.getElementById('centerPlayBtn');
  const controlsBar = document.getElementById('controlsBar');
  const niceguysLabel = document.getElementById('niceguysLabel');
  // SVGs for dynamic icons
  const playIcon = document.getElementById('playIcon');
  const pauseIcon = document.getElementById('pauseIcon');
  const volumeIcon = document.getElementById('volumeIcon');
  const muteIcon = document.getElementById('muteIcon');

  // --- NEW: Set video src from URL param if present ---
  const urlParams = new URLSearchParams(window.location.search);
  const videoSrc = urlParams.get('src');
  if (videoSrc) {
    video.src = videoSrc;
  }
  // --- END NEW ---

  let controlsTimeout;
  let isMouseOver = false;
  let hasStarted = false;

  // Remove native controls
  video.removeAttribute('controls');

  // Show/hide controls logic
  function showControls() {
    if (!hasStarted) return;
    controlsBar.classList.add('visible');
    controlsBar.classList.remove('hidden');
    timeDisplay.classList.remove('hidden');
    niceguysLabel.classList.remove('hidden');
    progressBar.classList.remove('hidden');
    clearTimeout(controlsTimeout);
    if (!video.paused) {
      controlsTimeout = setTimeout(hideControls, 2500);
    }
  }
  function hideControls() {
    if (!isMouseOver && !video.paused) {
      controlsBar.classList.remove('visible');
      controlsBar.classList.add('hidden');
      timeDisplay.classList.add('hidden');
      niceguysLabel.classList.add('hidden');
      progressBar.classList.add('hidden');
    }
  }
  playerContainer.addEventListener('mousemove', showControls);
  playerContainer.addEventListener('touchstart', showControls);
  controlsBar.addEventListener('mouseenter', () => { isMouseOver = true; });
  controlsBar.addEventListener('mouseleave', () => { isMouseOver = false; });
  video.addEventListener('play', showControls);
  video.addEventListener('pause', showControls);
  video.addEventListener('ended', showControls);

  // Hide controls after inactivity
  video.addEventListener('play', () => {
    controlsTimeout = setTimeout(hideControls, 2500);
  });
  video.addEventListener('pause', () => {
    clearTimeout(controlsTimeout);
    showControls();
  });

  // Center play button logic
  function showCenterPlay() {
    centerPlayBtn.classList.remove('hidden');
  }
  function hideCenterPlay() {
    centerPlayBtn.classList.add('hidden');
  }
  centerPlayBtn.addEventListener('click', () => {
    video.play();
    hideCenterPlay();
    hasStarted = true;
    controlsBar.classList.add('visible');
    controlsBar.classList.remove('hidden');
    timeDisplay.classList.remove('hidden');
    niceguysLabel.classList.remove('hidden');
    progressBar.classList.remove('hidden');
  });
  video.addEventListener('play', hideCenterPlay);
  video.addEventListener('pause', () => {
    if (video.currentTime === 0 || video.currentTime === video.duration) {
      showCenterPlay();
    } else {
      showControls();
    }
  });
  // Show center play on load, hide controls/time until play
  if (video.paused) {
    showCenterPlay();
    controlsBar.classList.remove('visible');
    controlsBar.classList.add('hidden');
    timeDisplay.classList.add('hidden');
    niceguysLabel.classList.add('hidden');
    progressBar.classList.add('hidden');
  }

  // Play/Pause toggle
  playPauseBtn.onclick = function() {
    if (video.paused) {
      video.play();
    } else {
      video.pause();
    }
  };
  video.onplay = function() {
    playIcon.style.display = 'none';
    pauseIcon.style.display = 'inline';
    hideCenterPlay();
    showControls();
  };
  video.onpause = function() {
    playIcon.style.display = 'inline';
    pauseIcon.style.display = 'none';
    if (video.currentTime === 0 || video.currentTime === video.duration) {
      showCenterPlay();
    }
  };

  // Rewind 15s
  rewindBtn.onclick = function() {
    video.currentTime = Math.max(0, video.currentTime - 15);
    showControls();
  };
  // Forward 15s
  forwardBtn.onclick = function() {
    video.currentTime = Math.min(video.duration, video.currentTime + 15);
    showControls();
  };

  // Fullscreen
  function isFullScreen() {
    return document.fullscreenElement === playerContainer || document.webkitFullscreenElement === playerContainer || document.msFullscreenElement === playerContainer;
  }
  fullscreenBtn.onclick = function() {
    if (!isFullScreen()) {
      if (playerContainer.requestFullscreen) {
        playerContainer.requestFullscreen();
      } else if (playerContainer.webkitRequestFullscreen) {
        playerContainer.webkitRequestFullscreen();
      } else if (playerContainer.msRequestFullscreen) {
        playerContainer.msRequestFullscreen();
      }
    } else {
      if (document.exitFullscreen) {
        document.exitFullscreen();
      } else if (document.webkitExitFullscreen) {
        document.webkitExitFullscreen();
      } else if (document.msExitFullscreen) {
        document.msExitFullscreen();
      }
    }
    showControls();
  };

  // Volume slider
  volumeSlider.oninput = function() {
    video.volume = this.value;
    updateVolumeIcon();
    showControls();
  };
  // Mute/Unmute
  muteBtn.onclick = function() {
    video.muted = !video.muted;
    updateVolumeIcon();
    volumeSlider.value = video.muted ? 0 : video.volume;
    showControls();
  };
  function updateVolumeIcon() {
    if (video.muted || video.volume === 0) {
      volumeIcon.style.display = 'none';
      muteIcon.style.display = 'inline';
    } else {
      volumeIcon.style.display = 'inline';
      muteIcon.style.display = 'none';
    }
  }

  // Progress bar update
  video.ontimeupdate = function() {
    const percent = (video.currentTime / video.duration) * 100;
    progress.style.width = percent + '%';
    updateTimeDisplay();
  };
  video.onloadedmetadata = updateTimeDisplay;

  // Seek on progress bar click
  progressBar.onclick = function(e) {
    const rect = progressBar.getBoundingClientRect();
    const x = e.clientX - rect.left;
    const percent = x / rect.width;
    video.currentTime = percent * video.duration;
    showControls();
  };

  function updateTimeDisplay() {
    const format = t => {
      if (isNaN(t)) return '00:00';
      const m = Math.floor(t / 60);
      const s = Math.floor(t % 60);
      return `${m.toString().padStart(2,'0')}:${s.toString().padStart(2,'0')}`;
    };
    timeDisplay.textContent = `${format(video.currentTime)} / ${format(video.duration)}`;
  }

  // Toggle play/pause on video click (not on controls)
  video.addEventListener('click', function(e) {
    // Prevent toggling if clicking on controls
    if (e.target.closest('.controls') || e.target.closest('.progress-bar')) return;
    if (video.paused) {
      video.play();
    } else {
      video.pause();
    }
  });
</script>
```

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

> **Tip:** For best organization, create **3 separate Embed Code blocks** in Webflow:
> 1. First — for HTML (player structure)
> 2. Second — for CSS (styles in `<style>...</style>`)
> 3. Third — for JS (script in `<script>...</script>`)

#### Example structure in Webflow:

![Webflow Embed Structure](./video/Screenshot%202025-07-18%20at%2015.27.22.png)

#### Player example:

![Player Example](./video/Screenshot%202025-07-18%20at%2015.27.41.png)

---

#### 1. HTML (Embed Code Block 1)
```html
<!-- ...same as above... -->
```

#### 2. CSS (Embed Code Block 2)
```css
/* ...same as above... */
```

#### 3. JS (Embed Code Block 3)
```html
<script>
/* ...same as above... */
</script>
```

5. Change the video src in `<video>` as needed or make it dynamic via CMS.

**Note:**
- If you use as div, the src parameter in URL will not work — you must set the src in code or via CMS.
- All three parts (HTML, CSS, JS) are required for full functionality.

---

---