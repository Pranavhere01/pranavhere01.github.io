# pranavhere01.github.io

Personal site of Pranav Kumar, Founder & CEO, Saarthi. A continuous-scroll portfolio for recruiters: one page, one stylesheet, no JavaScript, framework or build step.

```
index.html              Complete portfolio: hero and impact, work, experience, story, now, contact
saarthi/index.html      Redirect to the Saarthi section
work/index.html         Redirect to the work section
story/index.html        Redirect to the story section
css/site.css            The only stylesheet (tokens, reset, type, layout, components, media queries)
favicon.svg             PK monogram on an indigo tile
robots.txt  sitemap.xml
images/                 Served images (every raster under 300 KB)
images/src/             Originals, untouched, plus og.html (the source of og.jpg) and FACTS-old-build.py (the facts file)
files/Pranav_Kumar_Resume.pdf
```

## Editing

- Copy lives in `index.html`. Use facts from `images/src/FACTS-old-build.py` or existing site copy; do not invent numbers, clients, quotes, logos or credentials.
- The reading order is hero and impact, `#work` (Sagepilot `#sagepilot`, Saarthi `#saarthi`, SmartDeploy `#smartdeploy`, Darshan `#darshan`, then compact earlier work), `#experience`, `#story` including `#education`, `#now`, and `#contact`.
- Keep essential content visible in the scroll. The sticky header contains the name and résumé/email actions; there is no section menu or content hidden behind toggles.
- Preserve the existing `saarthi/`, `work/` and `story/` redirects and their fallback links when changing section IDs. Keep asset paths relative so previews also work below a repository path.
- When updating "Now", update its `<time datetime>` and visible date together. The footer year is hard-coded.
- Design tokens and responsive rules live in `css/site.css`. Preserve readable line lengths, visible keyboard focus and reduced-motion support. Phone layout is handled at 767px, 539px, and 380px: headings drop forced desktop line-breaks (`.break-lg`), the four Sagepilot metrics stay in a two-by-two grid without wrapping numbers, and notched phones get `viewport-fit=cover` plus safe-area padding.
- Fonts come from Google Fonts. Wrap Devanagari strings in `lang="hi"`.

## Images

Originals stay in `images/src/`. To regenerate the served files with Pillow:

```python
from PIL import Image
# photo: flatten alpha on white, JPEG q82, native 800x800 (never crop, never upscale)
im = Image.open("images/src/pranav-linkedin.png").convert("RGBA")
bg = Image.new("RGB", im.size, (255, 255, 255)); bg.paste(im, mask=im.split()[3])
bg.save("images/pranav.jpg", "JPEG", quality=82, progressive=True, optimize=True)
# phone screens: LANCZOS resize, displayed at no more than half the file's pixel width
Image.open("images/src/saarthi-sign-in.png").resize((600, 1298), Image.LANCZOS).save("images/saarthi-sign-in.png", optimize=True)
```

The served `saarthi-language`, `saarthi-welcome-en` and `saarthi-welcome-hi` PNGs are 560×1211; `saarthi-sign-in.png` is 600×1298. Keep their aspect ratios and display them at no more than half their file width. Use the matching high-resolution originals in `images/src/` when regenerating them.

`images/og.jpg` is rendered from `images/src/og.html` at 1200x630 with headless Chrome, then saved as JPEG q85.

## Previewing

Any static server works. From this folder:

```bash
python3 -m http.server 43127 --bind 127.0.0.1
```

Then open `http://127.0.0.1:43127/`. On a phone, also check 360×800 and 390×844, landscape, and a notched device if you can.

## Checks before publishing

- Review the complete scroll at 360, 768, 1200 and 1440 wide. Check for clipped content, page-level horizontal overflow and sections obscured by the sticky header.
- Test the résumé and email links, keyboard focus, section anchors and all three legacy redirects. Essential content must remain visible without clicks or JavaScript.
- Screenshot with headless Chrome: `"/Applications/Google Chrome.app/Contents/MacOS/Google Chrome" --headless=new --disable-gpu --hide-scrollbars --virtual-time-budget=8000 --window-size=1440,4000 --screenshot=out.png http://localhost:8766/`
- Check every factual change against `images/src/FACTS-old-build.py` or existing site copy. Do not add testimonials, logos, a phone number or a contact form.

## Deploying

The site is meant for GitHub Pages at the account root (`https://pranavhere01.github.io/`):

1. Put the contents of this folder at the root of the repository `Pranavhere01/pranavhere01.github.io` (branch `main`).
2. In the repository settings, Pages, choose "Deploy from a branch", branch `main`, folder `/ (root)`.
3. Keep `sitemap.xml` and the canonical URL aligned with the main portfolio URL, then push. Preserve the legacy redirect files so existing links still reach the relevant sections.

Nothing needs building. Do not add a bundler, a framework or analytics.
