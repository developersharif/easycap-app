# RealBrain Design System — instructions for coding agents

This is the EasyCap marketing site (`easycap-app`, branch `web`): plain static HTML, no build step. It is built entirely on the RealBrain Design System. Read this before touching any markup or CSS.

## Source of truth

- In this repository: `assets/css/tokens.css` — a vendored copy of https://realbrain.cc/design-system/tokens.css. Never edit it by hand; refresh it with `curl -o assets/css/tokens.css https://realbrain.cc/design-system/tokens.css`.
- Site-specific composition lives in `assets/css/site.css` (page shells, hero, FAQ, footer, legal prose). New site CSS belongs there, built from tokens — never inline in a `<style>` block and never scoped into one page.
- Full guide: https://realbrain.cc/design-system/design-system.md
- Short reference for agents: https://realbrain.cc/design-system/llms.txt
- Tokens as JSON (DTCG): https://realbrain.cc/design-system/tokens.json

Read the guide's §2 (non-negotiables) and §6 (components) before writing or editing any CSS, component, or UI copy.

## Rules

1. **Tokens only.** Every colour, size, radius, shadow, duration, and easing in component code is a `var(--token)` from `tokens.css`. No raw hex, no raw px, no `rgba()`.
2. **One accent per screen.** Exactly one `.btn-primary` (or one accent-coloured action). Everything else is `.btn-secondary`, `.btn-ghost`, or neutral text.
3. **Weight ≤ 600.** Never `font-weight: 700` or `bold`. The font import does not even load 700.
4. **Flat.** No `linear-gradient`, `radial-gradient`, `backdrop-filter`, `text-shadow`, glow, or decorative `box-shadow`. `--shadow-*` is only for surfaces that are actually elevated (menus, modals, toasts).
5. **Never white on accent.** Text and icons on `--accent` use `--accent-fg`.
6. **Semantic colour is never the only signal.** Use `.status.ok|warn|err|info` (dot plus text) or an icon plus text.
7. **Copy:** sentence case everywhere. Buttons are verbs ("Sign in", "Create project"). "Sign in", never "Log in" or "Login". Errors say what happened and what to do next.
8. **One density per product.** `.density-comfortable` (default, 44px controls) or `.density-compact` (32px) on a container. Never both.
9. **Use the existing component classes** (`.btn`, `.field`, `.input`, `.select`, `.checkbox`, `.radio`, `.toggle`, `.card`, `dialog.modal`, `.toast`, `.tabs`, `.table`, `.status`, `.scaffold`, `.empty`, `.skeleton`). Do not re-style them locally. Do not build a div-based fake of a native control.
10. **Motion goes through `--dur-1/2/3` and `--ease`.** Nothing else. Reduced motion is handled globally by `tokens.css`.

## When the system doesn't have what you need

Do not invent a value. Build the piece from existing tokens if you can. If you genuinely need a new token or component, ship the work with a clearly marked proposal in your final message:

```
⚑ NEW TOKEN  --{category}-{name}: {value}
   why nothing existing fits: …
```

Do the same for drift you notice in existing code (`⚑ DRIFT  file:line  #3b82f6 → --accent`) rather than silently patching it.

## Self-check before you finish

Run these from the repository root (adjust `src` and the excluded path). Every command should print nothing:

```sh
# raw colours or px outside the token file
# Expected remaining hits, all matching the design system's own source:
#   - "1px solid var(--border)" hairlines (tokens.css uses the same idiom; there is no width token)
#   - <meta name="theme-color" content="#080808"> (browser chrome cannot read a CSS variable)
grep -rnE --include='*.css' --include='*.html' --exclude=tokens.css --exclude-dir=.git \
  '#[0-9a-fA-F]{3,8}\b|rgba?\(|\b[0-9]+px\b' .

# forbidden weight and effects — must print nothing
grep -rnE --include='*.css' --include='*.html' --exclude=tokens.css --exclude-dir=.git \
  'font-weight:\s*(700|800|900|bold)|linear-gradient|radial-gradient|backdrop-filter|text-shadow' .

# copy rules — must print nothing
grep -rniE --include='*.html' --include='*.css' --exclude-dir=.git 'log ?in|Sign In\b|Log In\b' .
```

Media-query breakpoints in `site.css` use `em`, not `px`, so they stay out of the first grep and respect the reader's font size (48em = 768px, 60em = 960px, 40em = 640px).

Then check by eye: one `.btn-primary` per screen, focus rings visible when tabbing, and every status or badge has text next to its colour.

## Quick reference

<!-- BEGIN GENERATED: quickref -->
```
--bg              #080808   light: #F7F7F9
--surface         #121216   light: #FFFFFF
--elevated        #1A1A20   light: #FFFFFF
--text-1          #F4F4F6   light: #16161C
--text-2          #A9A9B4   light: #4C4C58
--text-3          #83838F   light: #6E6E7A
--accent          #8FA8FF   light: #3450C8
--accent-fg       #080808   light: #FFFFFF
--accent-hover    #9EB5FF   light: #4C6BD2
--accent-pressed  #6173B1   light: #21358A
--success         #3ECF8E   light: #177A4E
--warning         #E8B23D   light: #8A5A00
--error           #F26D6D   light: #C2373C
--info            #62B0F5   light: #1D62A8
--border          #26262E   light: #E3E3E9
--border-strong   #3A3A46   light: #C6C6D0
--font-ui        'IBM Plex Sans', system-ui, sans-serif
--font-mono      'IBM Plex Mono', ui-monospace, monospace
type steps       display:60/64  8:40/48  7:30/38  6:24/32  5:20/28  4:17/26  3:15/24  2:13/20  1:12/16
weights          400 · 500 · 600 (never 700)
space            4px 8px 12px 16px 24px 32px 48px 64px 96px
radius           sm 6px · md 10px · lg 16px · full 999px
motion           120ms · 200ms · 360ms · ease cubic-bezier(.22,.61,.36,1)
layout           marketing 1200px · app 1440px · control-h 44px (compact 32px)
```
<!-- END GENERATED: quickref -->

Light theme: `document.documentElement.setAttribute('data-theme', 'light')`. Dark: remove the attribute.
