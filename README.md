# NiceGuysPlayer for Webflow

A beautiful, customizable video player for easy embedding in Webflow (or any site) via iframe. Supports custom video links via URL parameter.

---

## 🚀 Deploy to Vercel

1. **Push this repo to GitHub.**
2. **Import your repo to [Vercel](https://vercel.com/import).**
   - No special config needed for static HTML/CSS/JS.
   - You can rename `videoplay.html` to `index.html` if you want it as the root page.
3. **After deploy, you’ll get a link like:**
   
   `https://your-vercel-project.vercel.app/videoplay.html`

---

## 🔗 Embed in Webflow

1. **In Webflow, add an Embed element** where you want the player.
2. **Paste this code:**

```html
<iframe 
  src="https://your-vercel-project.vercel.app/videoplay.html?src=YOUR_VIDEO_LINK" 
  width="100%" height="400" frameborder="0" allowfullscreen>
</iframe>
```
- Replace `YOUR_VIDEO_LINK` with a direct link to your `.mp4` or `.webm` video (from CDN, S3, Cloudinary, etc).
- Example:
  ```html
  <iframe src="https://your-vercel-project.vercel.app/videoplay.html?src=https://www.w3schools.com/html/mov_bbb.mp4" width="100%" height="400" frameborder="0" allowfullscreen></iframe>
  ```

---

## ⚡ Features
- Custom glassmorphism UI
- Play/pause by clicking anywhere on video
- 15s skip buttons, volume, mute, fullscreen
- Progress bar auto-hides with controls
- Responsive and mobile-friendly
- "NiceGuysPlayer" label with link (can be customized)

---

## 🛠️ Tips
- You can style the player further by editing `style.css`.
- The player will use the video from the `src` URL parameter. If not provided, it will use the default demo video.
- For best results, use direct video links (not YouTube/Vimeo pages).

---

## 🧑‍💻 Author
Made with ❤️ by [NiceGuys](https://www.niceguys.agency/) 