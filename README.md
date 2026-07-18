# 🍹 NUVE — Cocktail Bar Landing Page

A premium, animation-driven cocktail bar landing page built with **React 19**, **Tailwind CSS 4**, and **GSAP 3** (including the `SplitText` and `ScrollTrigger` plugins). The project delivers a scroll-pinned video scrubbing experience, a GSAP `SplitText` hero entrance, a parallax leaf system, a cocktail and mocktail menu list, an about section with an image grid and masked reveal, a clip-path art section, an interactive cocktail slider with GSAP entrance animations, and a footer contact section — all driven by scroll-triggered GSAP timelines.

---

## What This Project Does

The app renders a single-page layout for a fictional premium cocktail bar called **Nuvé / Velvet Pour**. Seven React sections are composed in `App.jsx`: `Navbar`, `Hero`, `Cocktails`, `About`, `Art`, `Menu`, and `Contact`. Every section uses `useGSAP` for animation setup and `ScrollTrigger` for scroll-aware playback. The hero features a `SplitText`-split character entrance and a scroll-scrubbed video that plays frame-by-frame as the user scrolls down the page. All data (menu items, social links, opening hours, cocktail details) is centralized in a `constants/index.js` file and consumed via named exports.

---

## Features

**1. Scroll-Scrubbed Video Hero** — `Hero.jsx` uses a `<video>` element with `muted`, `playsInline`, and `preload="auto"`. After metadata loads, a GSAP ScrollTrigger timeline is created on `videoRef.current.onloadedmetadata` that scrubs `currentTime` from 0 to `video.duration` as the user scrolls from the top to the bottom of the viewport. The video is pinned in place during the scroll range, creating the illusion of a scroll-controlled video playback.

**2. GSAP SplitText Hero Title** — The `.title` element ("Nuvé") is split into individual characters using `new SplitText(".title", { type: "chars, words" })`. Each character receives the `.text-gradient` class before animation begins. GSAP then animates all characters from `yPercent: 100` to `0` with a 1.8-second `expo.out` ease and a `0.06` stagger — creating a cascading character reveal. The `.subtitle` paragraph uses `SplitText` line splitting for a matching staggered entrance.

**3. Parallax Leaf System** — Decorative leaf images appear at section boundaries. The hero uses `.right-leaf` and `.left-leaf` images that are driven by a scroll timeline: `.right-leaf` moves `y: 200` while `.left-leaf` moves `y: -200` as the hero section scrolls out of view. The cocktails section has its own pair of leaf images (`#c-left-leaf`, `#c-right-leaf`) that animate from offset positions on scroll. The footer section animates `#f-right-leaf` and `#f-left-leaf` up by 50px in a coordinated timeline.

**4. Navbar with Scroll Background Transition** — `navbar.jsx` uses `useGSAP` with a `ScrollTrigger` that transitions the `nav` background from `transparent` to `rgba(0,0,0,0.5)` with a `blur(10px)` backdrop filter after the nav scrolls past its bottom edge. Navigation links are rendered from the `navLinks` constant array and link to each section via `href="#sectionId"` anchors.

**5. Cocktail & Mocktail Menu List** — `Cocktails.jsx` renders two side-by-side lists — "Most popular cocktails" and "Most loved mocktails" — by mapping over `cocktailLists` and `mockTailLists` from `constants/index.js`. Each item displays the drink name, country code, serving detail, and price. Leaf parallax animations run on scroll using a dedicated `parallaxTimeline`.

**6. About Section — Word-Split Heading + Image Grid** — `About.jsx` splits the `#about h2` heading into words using `SplitText` and animates them in with `yPercent: 100 → 0` and `opacity: 0 → 1`. A staggered animation follows immediately after for the `.top-grid div` and `.bottom-grid div` image containers. The layout uses a 12-column CSS grid with varied column spans: three images in the top row (3-col, 6-col, 3-col) and two in the bottom row (8-col, 4-col). A stat counter shows 4.5/5 rating with a customer count.

**7. Art Section — Scroll-Pinned Mask Reveal** — `Art.jsx` pins the `#art` section for the full scroll range between `top top` and `bottom center`. A GSAP timeline fades out `.will-fade` elements (heading and feature lists) one by one, then scales a `.masked-img` element from its initial size to `scale: 1.3` while animating its `maskSize` from the default to `400%` — creating a CSS mask-based zoom-into-image reveal. Once the image fills the frame, `#masked-content` (a subtitle and description block) fades in.

**8. Interactive Cocktail Slider (Menu)** — `Menu.jsx` maintains a `currentIndex` state and renders the current, previous, and next cocktail using a `getCocktailAt(offset)` helper that wraps around the `allCocktails` array cyclically. Clicking the previous or next arrow buttons calls `goToSlide(index)` with modular arithmetic. A tab navigation row at the top allows direct jump to any cocktail. On each `currentIndex` change, `useGSAP` replays entrance animations: the title fades in, the cocktail image slides from `xPercent: -100`, and the details block animates from `yPercent: 100`.

**9. Contact Footer with SplitText** — `Contact.jsx` splits the `#contact h2` into words and plays a scroll-triggered stagger entrance. Subsequent `h3` and `p` elements also animate in via `yPercent: 100` with stagger. Opening hours from `openingHours` constant are rendered as `<p>` pairs, and social icons (Instagram, X, Facebook) are rendered from the `socials` constant as accessible `<a>` elements.

**10. Mobile Responsiveness** — `useMediaQuery` from `react-responsive` is used in `Hero.jsx` and `Art.jsx` to adjust ScrollTrigger `start` and `end` values for mobile viewports. On mobile, the hero video pins at `"top 50%"` instead of `"center 60%"`, and the art section starts pinning at `"top 20%"` instead of `"top top"`.

---

## Tech Stack:

| Technology | Version | Role |
|---|---|---|
| React | 19.2.0 | Component architecture, `useState`, `useRef` for video scrubbing and slider state |
| Tailwind CSS | 4.1.18 | Utility-class styling via `@tailwindcss/vite` plugin |
| GSAP | 3.14.2 | All scroll animations — `ScrollTrigger`, `SplitText`, `Timeline`, `fromTo`, `to` |
| @gsap/react | 2.1.2 | `useGSAP` hook for lifecycle-safe GSAP setup and teardown |
| react-responsive | 10.0.1 | `useMediaQuery` for mobile breakpoint detection in animation setup |
| Vite | 7.2.4 | Build tool and dev server |
| ESLint | 9.39.4 | Linting with `eslint-plugin-react-hooks` and `eslint-plugin-react-refresh` |

---

## Data Constants (`constants/index.js`)

All content is separated from components into a single constants file:

| Export | Type | Used In |
|---|---|---|
| `navLinks` | Array of `{ id, title }` | `navbar.jsx` — renders nav `<a>` anchors |
| `cocktailLists` | Array of `{ name, country, detail, price }` | `Cocktails.jsx` — cocktail list |
| `mockTailLists` | Array of `{ name, country, detail, price }` | `Cocktails.jsx` — mocktail list |
| `profileLists` | Array of `{ imgPath }` | Available for profile avatar usage |
| `featureLists` | Array of strings | `Art.jsx` — right-side feature checklist |
| `goodLists` | Array of strings | `Art.jsx` — left-side quality checklist |
| `storeInfo` | Object with address, phone, email | Contact data |
| `openingHours` | Array of `{ day, time }` | `Contact.jsx` — hours grid |
| `socials` | Array of `{ name, icon, url }` | `Contact.jsx` — social icon links |
| `allCocktails` | Array of `{ id, name, image, title, description }` | `Menu.jsx` — slider content |

---

## Project Structure

```
NUVE/
├── index.html                      # Vite entry point
├── package.json                    # Dependencies: react 19, gsap, @gsap/react, react-responsive, tailwindcss 4, vite 7
├── package-lock.json               # Exact dependency lockfile
├── vite.config.js                  # Vite config — React plugin
├── eslint.config.js                # ESLint flat config
├── .gitignore                      # node_modules, dist excluded
├── constants/
│   └── index.js                    # All data: nav links, cocktail lists, opening hours, socials, allCocktails
├── public/
│   ├── fonts/
│   │   └── Modern Negra Demo.ttf   # Display typeface for hero title
│   ├── images/
│   │   ├── abt1.png … abt5.png     # About section image grid
│   │   ├── arrow.png               # Scroll-down arrow
│   │   ├── check.png               # Checklist bullet icon
│   │   ├── cocktail-left-leaf.png  # Parallax leaf for cocktails section
│   │   ├── cocktail-right-leaf.png
│   │   ├── cup-2.png               # Decorative cup asset
│   │   ├── drink1.png … drink4.png # Cocktail slider images (Classic Mojito, Raspberry Mojito, Violet Breeze, Curacao Mojito)
│   │   ├── fav.png                 # Favicon source
│   │   ├── fb.png / insta.png / x.png  # Social icons
│   │   ├── footer-drinks.png       # Footer decorative image
│   │   ├── footer-left-leaf.png    # Footer parallax leaves
│   │   ├── footer-right-leaf.png
│   │   ├── hero-left-leaf.png      # Hero section parallax leaves
│   │   ├── hero-right-leaf.png
│   │   ├── left-arrow.png          # Slider navigation arrows
│   │   ├── right-arrow.png
│   │   ├── logo.png                # Brand logo
│   │   ├── mask-img.png            # Mask overlay image for Art section
│   │   ├── noise.png               # CSS noise texture overlay
│   │   ├── profile1.png … profile4.png  # Customer profile avatars
│   │   ├── slider-left-leaf.png    # Slider section parallax leaves
│   │   ├── slider-right-leaf.png
│   │   └── under-img.jpg           # Art section masked reveal image
│   └── videos/
│       ├── input.mp4               # Source video (pre-processed)
│       └── output.mp4              # Processed video used for scroll-scrubbed playback
└── src/
    ├── main.jsx                    # React DOM root — mounts <App /> into #root
    ├── App.jsx                     # Root component — registers GSAP plugins, composes all sections
    ├── index.css                   # Global styles, CSS custom properties, section layouts, noisy texture class
    └── components/
        ├── navbar.jsx              # Sticky nav with transparent-to-blur scroll background transition
        ├── Hero.jsx                # SplitText character entrance, leaf parallax, scroll-scrubbed video pin
        ├── Cocktails.jsx           # Cocktail + mocktail list with leaf parallax
        ├── About.jsx               # Word-split heading, staggered image grid reveal
        ├── Art.jsx                 # Scroll-pinned mask zoom reveal with feature checklists
        ├── Menu.jsx                # Interactive cocktail slider — cyclic index, tab nav, GSAP entrance per slide
        └── Contact.jsx             # Footer with SplitText heading, hours list, social links, leaf parallax
```

---

## How to Run

1. Clone the repository
   ```bash
   git clone https://github.com/tripathipawan/NUVE.git
   ```

2. Move into the project directory
   ```bash
   cd NUVE
   ```

3. Install dependencies
   ```bash
   npm install
   ```

4. Start the development server
   ```bash
   npm run dev
   ```

5. Open `http://localhost:5173` in your browser

6. To build for production
   ```bash
   npm run build
   ```
   The compiled output goes into `dist/`, ready for Vercel, Netlify, or any static host.

---

## Repository

[https://github.com/tripathipawan/NUVE](https://github.com/tripathipawan/NUVE)
