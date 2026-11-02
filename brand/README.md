# Artifact Keeper brand assets

This directory holds the canonical Artifact Keeper logo files. The mark is the
**C1 "stacked artifact" shield**: a shield outline with a stacked box (an artifact)
inside it.

<p align="center">
  <img alt="Artifact Keeper symbol" src="symbol-primary-256.png" width="128">
</p>

## Palette

| Token | Hex | Used for |
|---|---|---|
| navy | `#06264B` | shield outline, "Artifact" text, dark tile |
| blue | `#087CF4` | left box face, "Keeper" text (light lockup), blue tile |
| cyan | `#06D6E9` | right box face, "Keeper" text (dark lockup) |
| white | `#FFFFFF` | box top, shield chevron, light tile |

## Which file to use

| Use | File |
|---|---|
| UI marks (navbars, login screens, docs headers), READMEs, any "mark on a background" | `symbol-primary.svg`, `symbol-primary-{16,24,32,48,64,96,128,256,512,1024}.png` |
| Monochrome contexts | `symbol-{navy,blue,cyan,black}.svg`, `-256.png`, `-512.png` |
| Avatars (GitHub org, Docker Hub, social accounts) | `symbol-primary-512.png` (transparent mark; `app-icon-blue-512.png` if a service needs a solid background) |
| iOS app icon (opaque, full-bleed, square corners) | `app-icon-1024.png` |
| macOS app icon (rounded, transparent corners) | `app-icon-1024-rounded.png` |
| Other app tiles | `app-icon-{blue,light,dark}.svg`, `-192.png`, `-512.png` |
| Browser tabs | `favicon.svg`, `favicon.ico` (16 to 256), `favicon-32.png` |
| Marks rendered below about 40 px | `symbol-small.svg` (same geometry, thicker white ring; the 16/24/32 PNGs are made from it) |
| iOS home-screen bookmark | `apple-touch-icon.png` (180x180) |
| Horizontal lockup (shield + one-line wordmark) | `logo-horizontal-light.{svg,png}` (light backgrounds), `logo-horizontal-dark.{svg,png}` (dark backgrounds); 1232x304 |
| Stacked lockup (shield + two-line wordmark) | `logo-stacked-wordmark-light.{svg,png}`, `logo-stacked-wordmark-dark.{svg,png}`; 716x304 |
| Geometry master and reusable paths | `source/canonical.svg`, `source/brand-components.json`, `source/wordmark-components.json`, `source/lockup-layouts.json` |

"light" and "dark" name the background the file is meant for, not the colour of
the lettering. The lockups carry 32 px of clear space on every side at native size;
keep it, do not trim.

### Banners

All banners are the same artwork laid out for each canvas (RGB, no alpha). Each
one ships as a PNG and as an editable SVG with outlined text.

| File | Size | Target |
|---|---|---|
| `banners/github-readme_1600x600.png` | 1600x600 | README headers and the org profile |
| `banners/github-social-preview_1280x640.png` | 1280x640 | GitHub repo social preview (Settings, Social preview) |
| `banners/website-open-graph_1200x630.png` | 1200x630 | Open Graph / Twitter card image |
| `banners/promotional-header_1500x500.png` | 1500x500 | promotional headers (X, LinkedIn, decks) |
| `banners/website-hero_1920x600.png` | 1920x600 | website hero |

## Linking

Raw URL pattern:

```
https://raw.githubusercontent.com/artifact-keeper/.github/main/brand/<file>
```

- READMEs may hotlink these URLs.
- Product UIs (website, web app, iOS, Android, Docker extension and so on) **vendor
  their own copies** and never hotlink raw.githubusercontent.com.

## Notes

- Every SVG uses outlined paths only; nothing depends on an installed font.
  The wordmark is a Noto Sans Bold reconstruction.
- The monochrome symbols (`symbol-{black,white,navy,blue,cyan}.*`) are single-ink
  outlines with a transparent interior. `symbol-white` is the one to use on dark
  surfaces.
- The full-colour symbol keeps an opaque navy field inside the shield so the white
  box faces stay visible on both light and dark backgrounds.

## README header snippets

Option A, the banner:

```html
<p align="center">
  <img alt="Artifact Keeper" src="https://raw.githubusercontent.com/artifact-keeper/.github/main/brand/banners/github-readme_1600x600.png" width="100%">
</p>
```

Option B, the lockup (theme-aware):

```html
<p align="center">
  <picture>
    <source media="(prefers-color-scheme: dark)" srcset="https://raw.githubusercontent.com/artifact-keeper/.github/main/brand/logo-horizontal-dark.png">
    <img alt="Artifact Keeper" src="https://raw.githubusercontent.com/artifact-keeper/.github/main/brand/logo-horizontal-light.png" width="360">
  </picture>
</p>
```
