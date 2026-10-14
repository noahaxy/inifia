# Portfolio — Future Informatics Teacher

A single-page, responsive portfolio site. No build step, no framework —
just `index.html` + `css/style.css` + `js/script.js`. Open `index.html`
in a browser (or serve the folder) and it works.

## How to edit content

**You almost never need to touch the HTML.** Nearly everything on the page
is rendered from a data object at the top of `js/script.js`:

- `semester1` / `semester2` — one object per course: title, reflection text,
  worksheet PDF path, artifact PDF path, documentation photo path.
- `projects` — your Innovative Works cards.
- `ppgEntries` — your PPG Activity Reflections timeline.

To edit a reflection, change the text in the matching object. To add or
remove a course, add/remove an object from the array — the accordion
re-numbers itself automatically.

## The video hero

The Home section opens with a scroll-scrubbed video: as you scroll down,
the video's playback position is tied directly to how far you've scrolled
(not autoplay). This is the same technique used on many product/portfolio
sites (Apple's product pages, for example).

- The video is `assets/video/hero.webm` (primary) and `hero.mp4` (fallback,
  for browsers without WebM support). Both are encoded with every frame as
  a keyframe, which is what makes the scrubbing feel instant rather than
  laggy — if you replace this video, re-encode any replacement the same
  way (`ffmpeg -i input.mp4 -c:v libx264 -g 1 ... ` for mp4, or
  `-c:v libvpx-vp9 -g 1 ...` for webm) or scrubbing will visibly lag on
  seek.
- The scroll track height (currently `320vh` in `.hero-video-wrap` in
  `style.css`) controls how much scrolling it takes to play through the
  whole clip. Increase it to make the scrub feel slower/longer, decrease
  it for a snappier scroll.
- **Important for deployment:** video seeking over HTTP requires the
  server to support byte-range requests (`Accept-Ranges: bytes`). Every
  mainstream static host (Netlify, Vercel, GitHub Pages, Nginx, Apache,
  S3+CloudFront) supports this by default, so this should just work. If
  you test locally with Python's `python -m http.server`, scrubbing will
  NOT work — that server doesn't support ranges. Use `npx serve .` instead
  for local testing.
- The headline text ("Calon Guru Profesional...") uses the Caveat
  handwriting font, self-hosted at `assets/fonts/Caveat-VariableFont.woff2`
  so it doesn't depend on Google Fonts being reachable. Swap the text in
  `index.html`'s `.hero-handwriting` element directly.

## Font loading

Space Grotesk, Inter, and JetBrains Mono load from Google Fonts (needs
internet access). Caveat (the handwriting hero title) is self-hosted in
`assets/fonts/` so the hero always renders correctly even offline or
behind restrictive networks.

## Replacing placeholders

Everything under `assets/` other than `hero.mp4`/`hero.webm`/`profile.jpg`
is still a placeholder generated for this first pass:

- `assets/images/profile.svg` → replace with your real photo. Keep the
  filename, or update the `src` in `index.html`'s hero section.
- `assets/images/s1-doc-*.svg`, `s2-doc-*.svg` → your documentation photos,
  referenced by `photo` in each course object in `script.js`.
- `assets/images/project-*.svg` → Innovative Works thumbnails.
- `assets/pdf/*.pdf` → dummy one-page PDFs. Replace with your real
  Worksheet/Artifact PDFs, same filenames (or update the paths in
  `script.js`).

Photos and PDFs both accept `.jpg`, `.png`, or `.pdf` — just update the
file extension in the relevant path if you're not using SVG placeholders.

## Notes on the two "six submenus" in the brief

The brief said Semester 2 has six submenus but only listed five. The site
currently ships with the five listed courses. If there's a sixth, add it
to the `semester2` array in `script.js` following the same shape as the
others — the layout handles it automatically.

## Theme

Light/dark mode toggle is in the top-right of the nav, persisted via
`localStorage`. Respects `prefers-color-scheme` on first visit and
`prefers-reduced-motion` for anyone with motion sensitivity.

## Deploying

This is fully static — drag the `portfolio/` folder into Netlify, Vercel,
GitHub Pages, or any static host, and it's live.
