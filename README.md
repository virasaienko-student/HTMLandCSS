# 🍕 Pizza Party

A single-page website built with plain HTML and CSS. A full-width pizza photo stays pinned at the top of the screen, and the article scrolls up over it like a sheet of paper sliding across a table.

## Preview

https://github.com/user-attachments/assets/2ad831db-471c-4bbf-a95e-5bd2c0138d42

<img width="978" height="929" alt="Screenshot of the Pizza Party website" src="https://github.com/user-attachments/assets/e033ccfa-ee3b-425b-ba80-a3c389b6aa2e" />

## Inspiration and moodboard

The layout was inspired by this designboom article, where a large image stays in view while the story scrolls over it:
[Art everywhere: Yinka Ilori's Chairman community center in London](https://www.designboom.com/design/art-everywhere-yinka-ilori-chairman-community-center-london-exhibition-interview/)

My moodboard and design exploration are in Figma:
[View the moodboard on Figma](https://www.figma.com/design/3tjwB2YXrUsLsChYr2UDq3/Untitled?node-id=0-1&t=3RNtPzh2EskB6CzE-1)

## What I learnt

**The scroll-over effect.** The image uses `position: sticky` with `top: 0`, so it scrolls normally until it reaches the top of the screen and then stays there. The content sits on a higher layer (`z-index: 2` vs `1`) and has a white background, so it covers the image as you scroll. Without the background colour, the text would just overlap the picture.

**The five `position` values.**
- `static` is the default. The element sits in the normal flow and can't be moved with `top`/`left`.
- `relative` keeps the element in place but lets you nudge it, and makes `z-index` work.
- `absolute` takes the element out of the flow and positions it inside its nearest positioned parent.
- `fixed` pins the element to the browser window, even when scrolling.
- `sticky` behaves normally until it reaches a set point, then sticks while its parent is on screen.

**Sticky vs fixed.** The same effect can be made with `position: fixed`, but the content then needs `margin-top: 70vh` because a fixed element no longer takes up space. Sticky needs less code and stops when its section ends, so it's the tidier choice here.

**Custom fonts with `@font-face`.** Fonts are loaded from `.woff2` files in the `fonts` folder. A mistake I made: inside `@font-face`, `font-family` must be a single name. Writing `font-family: "FormaDJRText", sans-serif;` there makes the whole rule invalid, and the browser silently falls back to the default font. Fallback fonts only go where the font is *used*, e.g. in `body`.

**Centering an absolute element.** `padding: auto` isn't valid CSS, so it does nothing. To center an absolutely positioned element, use `left: 0; right: 0; text-align: center;`, or `top: 50%; left: 50%; transform: translate(-50%, -50%);` for the exact middle.

**Vertical text.** `writing-mode: vertical-rl` lays the logo out vertically, and `transform: rotate(180deg)` flips it to read bottom to top. This is easier to position than rotating with `transform: rotate(-90deg)`, because the browser treats the text as genuinely vertical.

**Letter spacing.** `letter-spacing: 0.2em` spreads the letters out. Using `em` means the spacing scales with the font size.

**Images.** `object-fit: cover` stops an image from being stretched or squashed when its box has different proportions from the photo.

## Patterns to remember

A quick cheat sheet for future projects. Copy, paste and adapt.

### 1. Reset the page edges
Browsers add an 8px margin by default. Remove it for full-width layouts.
```css
body {
    margin: 0;
    padding: 0;
}
```

### 2. Sticky image with content scrolling over it
```css
.sticky-media {
    position: sticky;
    top: 0;
    height: 70vh;
    z-index: 1;
}
.content {
    position: relative;      /* needed for z-index to work */
    z-index: 2;              /* higher layer = on top */
    background-color: #fff;  /* without this, text overlaps the image */
}
```
Remember: sticky only works while its parent is on screen, and it breaks if a parent has `overflow: hidden`.

### 3. Relative parent, absolute child
Use this to place something (a logo, badge or label) on top of something else.
```css
.parent { position: relative; }   /* the anchor (sticky also works) */
.child  {
    position: absolute;
    top: 1rem;
    left: 1rem;
}
```

### 4. Centering
```css
/* Center horizontally (absolute element) */
.child { position: absolute; left: 0; right: 0; text-align: center; }

/* Dead center (absolute element) */
.child {
    position: absolute;
    top: 50%;
    left: 50%;
    transform: translate(-50%, -50%);
}

/* Center a normal block element with a set width */
.box { width: 600px; margin: 0 auto; }
```
Remember: `padding: auto` is not valid CSS, and `margin: 0 auto` doesn't work on absolute elements.

### 5. Custom fonts
```css
@font-face {
    font-family: "FontName";          /* ONE name only, no fallbacks here */
    src: url("fonts/FontName.woff2") format("woff2");
    font-weight: normal;
    font-style: normal;
    font-display: swap;
}

body {
    font-family: "FontName", sans-serif;   /* fallbacks go here */
}
```
For bold or italic files, add another `@font-face` with the **same name** and a different `font-weight` or `font-style`.

### 6. Vertical text
```css
.logo {
    writing-mode: vertical-rl;    /* top to bottom */
    transform: rotate(180deg);    /* flip to read bottom to top */
}
```

### 7. Letter spacing
```css
.logo { letter-spacing: 0.2em; }  /* em scales with font size */
```

### 8. Images that fill a box without stretching
```css
img {
    width: 100%;
    height: 100%;
    object-fit: cover;   /* fills and crops; "contain" shows all but may leave gaps */
}
```

### Units cheat sheet
| Unit | Means | Good for |
|---|---|---|
| `px` | fixed pixels | borders, tiny details |
| `rem` | root font size (usually 16px) | padding, margins, font sizes |
| `em` | current element's font size | letter-spacing, spacing tied to text |
| `%` | percentage of the parent | widths |
| `vh` / `vw` | 1% of the screen height / width | full-screen sections |

### Debugging checklist
When something doesn't work:
1. **Invalid CSS fails silently.** The browser skips any line it doesn't understand without an error. Check spelling and values first.
2. **Open DevTools (F12).** In the Elements tab, crossed-out styles are being overridden, and a yellow warning icon means a value is invalid.
3. **Check the Network tab** for red 404 errors on images and fonts. That means the file path is wrong.
4. **Paths are case-sensitive** online: `Pizza.jpg` and `pizza.jpg` are different files. Paths in CSS are relative to the CSS file.
5. **Avoid spaces in file names.** Use `my-font.woff2` instead of `my font.woff2`.
6. **`z-index` not working?** The element probably needs a `position` other than `static`.

## Project structure

```
HTMLandCSS/
├── fonts/          custom .woff2 font files
├── images/         pizza photo
├── index.html      page content
├── styles.css      all styling
└── README.md
```

## How to reuse this

1. Clone or download the repository and open `index.html` in a browser. No build tools or installs are needed.
2. Replace `images/pizza.jpg` with your own image, and update the `src` and `alt` in `index.html`.
3. Change the logo text inside `<div class="logo">` and swap the article content inside `<main class="content">`.
4. Adjust how much of the screen the image covers by changing `height: 70vh` in `.sticky-media`.
5. To use different fonts, add your `.woff2` files to `fonts/` and update the `@font-face` rules and `font-family` values in `styles.css`.

The pattern works for any image-led article: recipes, travel stories, portfolios or event pages.

**Note on fonts:** FormaDJR and OO Theran are commercial typefaces. If you reuse this project, use fonts you have a licence for, or swap in free ones from [Google Fonts](https://fonts.google.com).
