# Changelog

## 1.0.1 — 2026-09-18

Audit and integration release on top of 1.0.0: host-page isolation and accessibility fixes, whole-pixel layout down to 320 px, a public API with events and script gating, a versioned consent record, a configuration object and neutral default texts in all nine languages. Two keyboard behaviours changed.

Breaking:

- The stored record changed shape and key: `localStorage['retroCookieBanner.consent']` = `{ schemaVersion: 2, policyVersion, updatedAt, expiresAt, categories: { necessary, personalization, statistics, marketing } }`. A 1.0.0 value under `cookieConsent` is not read, so visitors are asked once more after the upgrade.
- Paragraph links in the `T` dictionary are `[label](key)` instead of `<a href="#">…</a>`; their targets come from `urls` in the config. The `data-t-html` attribute is now `data-t-rich`. Texts are rendered with `textContent` and DOM-built links, never `innerHTML`.

New:

- `window.RetroCookieConfig` (optional, defined before the script): `storageKey`, `policyVersion`, `expiryDays` (default 182), `lang`, `urls` (`learnMore`, `privacy`, `terms`, `thirdParties`, `optOut`), `texts` (per-language overrides; keys missing from a language fall back to English, so a language can be added at runtime).
- `window.RetroCookie` API: `getConsent()`, `open(screen?)`, `close()`, `acceptAll()`, `rejectAll()`, `update({ … })`, `reset()`, `version`. The overlay stays in the DOM after a choice, so `open()` brings it back with the saved toggles and hands the focus back to the opener when it closes.
- Events on `document`: `cookieconsent:ready` (once, with the stored record or `null`) and `cookieconsent:change` (after every save, API call or reset, with `consent`, `categories` and `previous`).
- Script gating: `<script type="text/plain" data-cookie-category="…" data-src="…">` or inline code is activated once its category is allowed; `async`, `integrity`, `data-domain` and other attributes are copied, `data-type` sets the type. Each script runs once; scripts placed after the banner script are picked up on `DOMContentLoaded`.
- Stored consent is validated strictly: schema version, policy version, expiry date and a real boolean for every category. Anything else asks again.
- Storage falls back from `localStorage` to `sessionStorage`, then to memory for the current page (private mode, blocked storage, sandboxed frames); saving never throws.
- `Escape` closes the dialog: as *Deny All* on the first visit, without changes when it was reopened.
- *Manage Cookies* moves the focus to the first toggle instead of back to *Accept All*.
- Default texts are neutral in all nine languages: necessary cookies always, optional ones only with consent, no claims about IP addresses, network access, sign-in or anonymised data. The last paragraph link is now the "cookie settings page". Screenshots and the social preview are refreshed.
- `<noscript>` notice: visitors without JavaScript see a docked, non-blocking cookie notice in the same style (English).
- Dialog has `aria-describedby` pointing at the notice text, or at the settings intro on the settings screen.
- CSP: deployment note in the script; gated scripts hand their nonce to the live script, so nonce-based policies cover them. `touch-action: manipulation` on all controls. A second copy of the banner on one page no longer fights the first. Legacy `zh-CHT` / `zh-CHS` locales map to the Chinese dictionaries. A key missing from every language renders as the key instead of `undefined`.
- The banner declares its direction: `dir="ltr"` on the overlay and `direction: ltr` in the CSS, so a right-to-left host page no longer mirrors it.
- Demo page: a *Cookie settings* button that calls `RetroCookie.open()`.

Fixes and hardening from the audit of 1.0.0:

- CSS variables renamed to `--cookie-*` and scoped to the overlay; the global `* { box-sizing }` rule is gone from the banner block, so the banner no longer alters the host page.
- The banner sets its own font, size and resets on buttons, heading and labels; host `button`, `h2`, `p`, `label` styles cannot bleed in.
- Focus ring is black (8.5:1 on the teal) instead of white (2.3:1), with a `:focus` fallback for browsers without `:focus-visible`.
- Toggle knob travels symmetrically (1 px at both ends).
- `Enter` no longer accepts consent while the focus is in a host-page control; `Tab` from outside the dialog is pulled back in.
- Stored consent is validated: an unreadable or old-format value shows the banner again instead of counting as consent.
- Dialog opens scrolled to the top on small screens (moving focus to *Accept All* no longer scrolls it down to the buttons).
- Page scroll is locked only when the page overflows, with `scrollbar-gutter: stable` so nothing jumps sideways.
- Flex `gap` replaced by margins; `inset` and `clip-path` have fallbacks. Works from Safari 10.1 / iOS 10.3, Chrome 51, Firefox 52, Edge 79.
- Safe-area padding for notched phones, no sticky `:hover` on touch screens, no iOS tap highlight, print stylesheet hides the banner, buttons stack below 720 px so the widest (German) set never wraps.
- Title is an `<h2>`; decorative switch tracks are hidden from screen readers; text links and language buttons meet the 24 px target size.
- Every line-height inside the banner is a whole number of pixels, so all rows sit on the pixel grid and borders and toggle knobs render identically on 1x screens.
- Long words hyphenate on narrow screens and can never overflow the dialog (German compounds and Estonian at 320 px); each `[ language ]` unit in the switch stays on one line.

## 1.0.0 — 2026-09-17

First stable release.

- Single-file cookie consent banner: HTML, CSS and vanilla JavaScript, zero dependencies.
- Nine languages with automatic browser-language detection: English, German, Russian, Ukrainian, Belarusian, Finnish, Estonian, Traditional Chinese (HK), Simplified Chinese (SG).
- In-banner language switch to English and back.
- Settings screen with Necessary, Personalisation, Statistics and Marketing toggles.
- Consent stored as JSON in `localStorage`; banner shown once.
- Keyboard-accessible: default *Accept All* button, Enter shortcut, focus trap, visible focus rings, `prefers-reduced-motion`.
- Responsive from 320 px to TV screens.
- UTF-8 BOM for hosts that send a wrong charset header.
