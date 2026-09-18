# retro-cookie-banner

<p>
  <a href="LICENSE" target="_blank" rel="noopener"><img src="https://img.shields.io/github/license/AbbeyLubber/retro-cookie-banner" alt="License: MIT"></a>
  <a href="https://github.com/AbbeyLubber/retro-cookie-banner/commits/main" target="_blank" rel="noopener"><img src="https://img.shields.io/github/last-commit/AbbeyLubber/retro-cookie-banner" alt="Last commit"></a>
  <a href="https://abbeylubber.github.io/retro-cookie-banner/" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/demo-GitHub%20Pages-1fb5bd" alt="Live demo"></a>
  <a href="https://abbeylubber.github.io/retro-cookie-banner/" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/languages-9-f5f542" alt="9 languages"></a>
  <a href="https://github.com/AbbeyLubber/retro-cookie-banner/" target="_blank" rel="noopener"><img src="https://img.shields.io/badge/dependencies-0-brightgreen" alt="Zero dependencies"></a>
  <a href="https://github.com/AbbeyLubber/retro-cookie-banner/" target="_blank" rel="noopener"><img src="https://img.shields.io/github/languages/top/AbbeyLubber/retro-cookie-banner" alt="Language: HTML"></a>
</p>

**retro-cookie-banner** is a free, open-source cookie consent banner (cookie popup / GDPR cookie notice) written in plain HTML, CSS and vanilla JavaScript. It ships as a single self-contained HTML file with zero dependencies — no framework, no build step, no external requests — and drops into any website, static site or CMS.

The banner detects the visitor's browser language and shows itself in one of nine languages (English, German, Russian, Ukrainian, Belarusian, Finnish, Estonian, Traditional Chinese for Hong Kong, Simplified Chinese for Singapore). Visitors can accept all cookies, deny all, or pick categories (necessary, personalisation, statistics, marketing) on a settings screen with toggle switches. The choice is stored as a versioned JSON record in `localStorage` (falling back to `sessionStorage`, then memory), the banner appears only once until the record expires or your policy version changes, and the whole dialog is keyboard-accessible with a focus trap, visible focus rings and `prefers-reduced-motion` support. A small `RetroCookie` API and two DOM events let the site reopen the dialog, read the choice and load analytics or ads only after consent; scripts marked `type="text/plain"` with a category are activated automatically. The look is retro/brutalist: hard shadows, thick black borders, monospace type.

Call it what you like — cookie banner, cookie popup, cookie notice, cookie bar, consent dialog, GDPR / ePrivacy consent modal — it is the same thing: a small, self-hosted alternative to cookie-consent SaaS scripts and to heavier JavaScript libraries.

**The banner is one file: [`banner/index.html`](banner/index.html).** Everything else in the repository is documentation, screenshots and the GitHub Pages demo around it.

## Live demo

The banner picks the language from the browser. Click a parameter to open the demo in that language:

| Flag | Parameter | Language |
|:---:|:---:|:---:|
| :gb: | [`?lang=en-GB`](https://abbeylubber.github.io/retro-cookie-banner/?lang=en-GB) | English (English) — default |
| :hong_kong: | [`?lang=zh-HK`](https://abbeylubber.github.io/retro-cookie-banner/?lang=zh-HK) | 繁體中文（香港）(Traditional Chinese, Hong Kong) |
| :singapore: | [`?lang=zh-SG`](https://abbeylubber.github.io/retro-cookie-banner/?lang=zh-SG) | 简体中文（新加坡）(Simplified Chinese, Singapore) |
| :ru: | [`?lang=ru-RU`](https://abbeylubber.github.io/retro-cookie-banner/?lang=ru-RU) | Русский (Russian) |
| :ukraine: | [`?lang=uk-UA`](https://abbeylubber.github.io/retro-cookie-banner/?lang=uk-UA) | Українська (Ukrainian) |
| :finland: | [`?lang=fi-FI`](https://abbeylubber.github.io/retro-cookie-banner/?lang=fi-FI) | Suomi (Finnish) |
| :de: | [`?lang=de-DE`](https://abbeylubber.github.io/retro-cookie-banner/?lang=de-DE) | Deutsch (German) |
| :belarus: | [`?lang=be-BY`](https://abbeylubber.github.io/retro-cookie-banner/?lang=be-BY) | Беларуская (Belarusian) |
| :estonia: | [`?lang=et-EE`](https://abbeylubber.github.io/retro-cookie-banner/?lang=et-EE) | Eesti (Estonian) |

## Language detection

`navigator.languages` is checked in order; the first entry that matches wins, everything else falls back to English.

| Browser locale | Banner language |
|:---:|:---:|
| `en`, `en-GB`, `en-US`, `en-*` | English (British spelling) |
| `zh-HK`, `zh-MO`, `zh-TW`, `zh-Hant`, `zh-Hant-*` | Traditional Chinese (Hong Kong) |
| `zh-SG`, `zh-CN`, `zh`, `zh-Hans`, `zh-Hans-*` | Simplified Chinese (Singapore) |
| `ru`, `ru-*` | Russian |
| `uk`, `uk-*` | Ukrainian |
| `fi`, `fi-*` | Finnish |
| `de`, `de-*` | German |
| `be`, `be-*` | Belarusian |
| `et`, `et-*` | Estonian |
| anything else | English |

The `lang` attribute is set on the dialog only (`en-GB`, `zh-HK`, `zh-SG`, `ru`, `uk`, `fi`, `de`, `be`, `et`), so the host page's own language is untouched. All nine languages are written left to right, and the banner says so explicitly: `dir="ltr"` on the overlay plus `direction: ltr` in its CSS, so it keeps its layout on a right-to-left host page. There is no right-to-left layout yet.

## Features

- **One HTML file.** No build step, no framework, no external requests. Copy and paste.
- **Nine languages, picked automatically** from `navigator.languages`: British English, German, Russian, Ukrainian, Belarusian, Finnish, Estonian, Traditional Chinese (Hong Kong) and Simplified Chinese (Singapore). Anything else falls back to English.
- **In-banner language switch** — `Language: [ENGLISH] [繁體中文]` — shown only when the browser language is not English. Positions are fixed; the active language is lowercase and underlined, the other one is uppercase and clickable.
- **Two screens:** a short notice with *Accept All / Deny All / Manage Cookies*, and a settings screen with *Necessary* (locked), *Personalisation*, *Statistics* and *Marketing* toggles, each with a one-line description and an ON/OFF label inside the switch.
- **Shown once.** The decision is saved as a versioned JSON record under `localStorage['retroCookieBanner.consent']`; on the next visit the banner stays hidden until the record expires (182 days by default) or you bump the policy version. Reopen it any time with `RetroCookie.open()`.
- **Public API and events.** `RetroCookie.getConsent()`, `open()`, `close()`, `acceptAll()`, `rejectAll()`, `update()`, `reset()`; `cookieconsent:ready` and `cookieconsent:change` fire on `document`, the latter with the previous record so withdrawals can be handled.
- **Script gating.** `<script type="text/plain" data-cookie-category="statistics" data-src="…">` runs only once its category is allowed: on page load for returning visitors, or right after the choice.
- **Configurable without editing the file.** Link targets, storage key, expiry, policy version, forced language and any text through `window.RetroCookieConfig`.
- **Keyboard first.** *Accept All* is the default button and receives focus; `Enter` anywhere inside the dialog triggers it unless the focus is on another button, link or toggle. `Escape` on the first visit closes the dialog as *Deny All*; on a reopened dialog it just closes. *Manage Cookies* moves the focus to the first toggle. `Tab` is trapped inside the dialog while it is open.
- **Accessible.** `role="dialog"`, `aria-modal`, `role="switch"` on toggles, visible `:focus-visible` rings, `prefers-reduced-motion` respected, state readable without colour (black border + ON/OFF text).
- **Responsive** from 320 px (iPhone 5) to TV screens: buttons stack on phones, the whole dialog scales up on ≥1600 px displays.

## Screenshots

### English (browser language is English)

No language switch is shown — the visitor sees only the banner.

| Desktop | Mobile (390 px) |
|:---:|:---:|
| ![Main screen, desktop](docs/screenshots/main-en.png) | ![Main screen, mobile](docs/screenshots/mobile-main-en.png) |
| ![Settings screen, desktop](docs/screenshots/settings-en.png) | ![Settings screen, mobile](docs/screenshots/mobile-settings-en.png) |

### Local language (browser language is not English)

The banner opens in the visitor's language and adds a `Language: [ENGLISH] [native]` switch above the buttons. Traditional Chinese (Hong Kong) shown as an example.

| Desktop | Mobile (390 px) |
|:---:|:---:|
| ![Main screen, desktop, zh-HK](docs/screenshots/main-zh-hk.png) | ![Main screen, mobile, zh-HK](docs/screenshots/mobile-main-zh-hk.png) |
| ![Settings screen, desktop, zh-HK](docs/screenshots/settings-zh-hk.png) | ![Settings screen, mobile, zh-HK](docs/screenshots/mobile-settings-zh-hk.png) |

## Usage

`banner/index.html` is the banner plus a small demo page around it. To add the banner to your own site, copy three blocks from it:

1. In `<style>`, everything between the `retro-cookie-banner` and `END retro-cookie-banner` comment markers (the rules above the first marker belong to the demo page).
2. The `<noscript>` notice and the `<div class="cookie-overlay" id="cookieOverlay" hidden>…</div>` markup that follows it — put them right before `</body>`. The notice is what visitors without JavaScript see (English only; translate it if you like).
3. The `<script>` block that follows it (not the small demo script above the markup).

That's it. The banner shows itself on the first visit and stays hidden afterwards. It brings its own fonts, sizes and resets and scopes its CSS variables to the overlay, so it neither changes the host page's styles nor picks up the host's `button`, `h2`, `p` or `label` rules.

### Configuration

Optional. Define the object *before* the banner script; every key has a default:

```html
<script>
window.RetroCookieConfig = {
  urls: { learnMore: '/cookies', privacy: '/privacy', terms: '/terms', thirdParties: '/cookies#third-parties', optOut: '/cookies#settings' },
  policyVersion: '2026-09-01', // bump it when your cookie policy changes: every visitor is asked again
  expiryDays: 182,             // ask again after this many days (default 182)
  storageKey: 'retroCookieBanner.consent',
  lang: null,                  // force a language ('de', 'zh-HK', …) instead of detecting it
  texts: { en: { statisticsDesc: 'Usage statistics collected with Plausible; no cookies, no IP address stored.' } }
};
</script>
```

`urls` holds the five links of the two paragraphs (`[Learn more](learnMore)` and so on in the dictionary): one entry covers all nine languages. `texts` overrides single keys per language and leaves the rest of the dictionary alone; a key missing from a language falls back to English. Regulators expect the category descriptions to say what actually runs on your site (providers, cookie names, lifetimes): put that into `personalizationDesc`, `statisticsDesc` and `marketingDesc` through `texts`.

### Reading the consent

```js
var consent = RetroCookie.getConsent();
// → null before a choice, otherwise
// { schemaVersion: 2, policyVersion: null,
//   updatedAt: '2026-09-18T07:35:00.000Z', expiresAt: '2027-03-19T07:35:00.000Z',
//   categories: { necessary: true, personalization: false, statistics: true, marketing: false } }
if (consent && consent.categories.statistics) {
  // load your analytics
}
```

The same record is the JSON under `localStorage['retroCookieBanner.consent']`, so server-side code or a script that runs before the banner can read it directly. A record with another `schemaVersion` or `policyVersion`, an `expiresAt` in the past, or a category that is not a plain boolean is ignored and the banner asks again.

`Accept All` stores every category as `true`, `Deny All` (and `Escape` on the first visit) stores everything except `necessary` as `false`, `Accept Selected` stores the toggle states.

### Events

Instead of polling storage, listen on `document`:

```js
document.addEventListener('cookieconsent:ready', onConsent);  // once, when the banner script has run
document.addEventListener('cookieconsent:change', onConsent); // after every Accept / Deny / Save, API call or reset

function onConsent(e) {
  var now = e.detail.categories;                               // null while there is no consent
  var was = e.detail.previous && e.detail.previous.categories; // only on change
  if (now && now.statistics) loadAnalytics();
  if (was && was.statistics && !(now && now.statistics)) stopAnalytics(); // consent withdrawn
}
```

`detail` carries `consent` (the new record or `null`), `categories` (its categories or `null`) and, on `change`, `previous` (the record before the change or `null`). Withdrawal is the site's job: a script that already runs cannot be unloaded, so compare `previous` with `categories`, stop sending events and delete your own cookies.

### API

```js
RetroCookie.getConsent();                 // record or null
RetroCookie.open();                       // reopen: settings screen with the saved toggles, or the notice if nothing is saved yet
RetroCookie.open('main');                 // force a screen: 'main' or 'settings'
RetroCookie.close();                      // hide without saving
RetroCookie.acceptAll();                  // same as the buttons
RetroCookie.rejectAll();
RetroCookie.update({ statistics: true }); // change some categories, keep the rest
RetroCookie.reset();                      // forget the choice; the banner shows again on the next page load (or call open())
RetroCookie.version;                      // '1.0.1'
```

A "Cookie settings" link in the footer is one line; the dialog returns the focus to it when it closes:

```html
<button type="button" onclick="RetroCookie.open()">Cookie settings</button>
```

### Loading scripts only with consent

Mark a script with `type="text/plain"` so the browser ignores it, and name the category. The banner turns it into a live script as soon as that category is allowed: on page load for returning visitors, or right after the choice.

```html
<!-- external -->
<script type="text/plain" data-cookie-category="statistics" data-src="https://plausible.io/js/script.js" data-domain="example.com"></script>

<!-- inline -->
<script type="text/plain" data-cookie-category="marketing">
  initMarketing();
</script>
```

Every other attribute (`async`, `crossorigin`, `integrity`, `data-domain`, …) is copied to the live script and `data-type="module"` sets its `type`; dynamically inserted scripts always load asynchronously, so `defer` has no effect. Each script runs once. Scripts placed after the banner script are picked up on `DOMContentLoaded`. Categories are `necessary`, `personalization`, `statistics` and `marketing`, one per script.

### Customising

- **Texts** live in the `T` dictionary in the script, one object per language; override single keys without touching the file through `texts` in the [config](#configuration). Every key maps to an element with the matching `data-t` (plain text) or `data-t-rich` (paragraphs with links) attribute. Texts are rendered with `textContent` and DOM-built links, never `innerHTML`, so a translation supplied through config or a CMS cannot inject markup. The default wording is deliberately generic: necessary cookies are always set, optional ones only with consent, and nothing is claimed about the data your site processes. Check it against your privacy notice and override whatever does not fit.
- **Links** in the two paragraphs are written as `[label](key)` in the dictionary; the five keys (`learnMore`, `privacy`, `terms`, `thirdParties`, `optOut`) get their `href` from `urls` in the config, `#` until you set them.
- **Colours and fonts** are CSS custom properties on `.cookie-overlay`: `--cookie-bg` (teal), `--cookie-ink` (black), `--cookie-paper` (white), `--cookie-accent` (yellow primary button), `--cookie-font` and `--cookie-font-cjk`. Only *Accept All* uses the accent; set `--cookie-accent` to the value of `--cookie-paper` if you want the three buttons to look identical (some regulators read a highlighted accept button as emphasis). For a dark theme, redefine the variables under your own `@media (prefers-color-scheme: dark)` rule; the banner deliberately ships one fixed look.
- **Storage key, expiry and policy version** are `storageKey`, `expiryDays` and `policyVersion` in the config; the defaults sit in `DEFAULTS` at the top of the script.

### Adding a language

Two edits, both inside the `<script>` block. Example for Polish:

```js
// 1. Add a dictionary entry. Copy the `en` object and translate every value.
pl: {
  langName: 'Polski', langLabel: 'Język:',
  title: 'Ustawienia cookie', settingsTitle: 'Zarządzaj cookie',
  p1: '…', p2: '…',
  accept: 'Zaakceptuj wszystkie', deny: 'Odrzuć wszystkie', manage: 'Zarządzaj cookie', save: 'Zaakceptuj wybrane',
  settingsIntro: 'Wybierz, których cookie możemy używać', back: '← Wstecz', alwaysOn: '(zawsze włączone)',
  necessary: 'Niezbędne', necessaryDesc: '…',
  personalization: 'Personalizacja', personalizationDesc: '…',
  statistics: 'Statystyka', statisticsDesc: '…',
  marketing: 'Marketing', marketingDesc: '…'
},

// 2. Teach detectLang() the locale codes.
if (l === 'pl' || l.indexOf('pl-') === 0) return 'pl';
```

`langName` is what appears in the switcher; keep it in the language itself (`Polski`, not `Polish`). Links inside `p1` and `p2` are written as `[label](key)` with one of the five link keys. Non-Latin scripts that need a different font can be targeted with `.cookie:lang(xx)` in CSS, as the Chinese entries are.

A language can also be supplied at runtime through `texts` in the config (`texts: { pl: { … } }` together with `lang: 'pl'` or `?lang=pl`); keys missing from it fall back to English.

### Testing

Force a language with the `?lang=` parameter — see the [Live demo](#live-demo) table for the values. To see the banner again after making a choice, run `RetroCookie.reset()` in the console and reload, or `RetroCookie.open()` to reopen it right away with the saved toggles.

## Comparison with other cookie consent tools

| | retro-cookie-banner | [cookieconsent](https://github.com/orestbida/cookieconsent) | [Klaro!](https://github.com/klaro-org/klaro-js) | [tarteaucitron.js](https://github.com/AmauriC/tarteaucitron.js) | Cookiebot, CookieYes, OneTrust (SaaS) |
|:---:|:---:|:---:|:---:|:---:|:---:|
| Delivery | one HTML file | JS + CSS files | JS + CSS files | JS + CSS + language files | script loaded from the vendor |
| Dependencies | none | none | none | none | vendor service |
| External requests at runtime | none | none | none | none | yes, every page view |
| Languages built in | 9, auto-detected | you supply translations in config | you supply translations in config | many bundled language files | vendor-managed |
| Category toggles | yes | yes | yes | per-service | yes |
| Consent storage | `localStorage` JSON, versioned, with expiry; `sessionStorage` / memory fallback | cookie / localStorage | cookie / localStorage | cookie | vendor cookie |
| Script gating | `type="text/plain"` + category | `type="text/plain"` + category | per-service | per-service | per-service |
| Events / API | 2 DOM events, 7 methods | callbacks + API | callbacks + API | callbacks | vendor API |
| Configuration | optional config object; texts in one dictionary | JS config object | JS config object | JS config + service list | web dashboard |
| Licence | MIT | MIT | BSD-3-Clause | MIT | commercial, free tier with limits |
| Best for | small sites that want one file and no build | sites that need a flexible config API | sites with many third-party services to gate | French-market sites with many services | organisations that need audit logs and consent records |

retro-cookie-banner shows a consent dialog, remembers the answer, activates `type="text/plain"` scripts by category and tells the page through events. It has no service catalogue, no consent log and no dashboard; if you need those, the tools to the right are the better fit.

## FAQ

**Is this GDPR compliant?**
It implements the mechanics the GDPR and the ePrivacy Directive ask for: no non-essential cookies before consent, an equally prominent *Deny All*, granular categories and a way to change the choice later. Compliance also depends on your texts, your privacy notice and on actually gating your scripts: mark them `type="text/plain"` with a category, or listen to the events. What the banner cannot do is stop a script that already ran when a visitor withdraws consent; handle that in the `cookieconsent:change` event.

**Does it work without a framework?**
Yes. It is plain HTML, CSS and ES5-compatible JavaScript. It works on static sites, WordPress, Shopify themes, Jekyll, Hugo, Astro — anything that lets you paste HTML.

**How big is it?**
About 53 KB unminified (roughly 18 KB gzipped) with all nine languages, API and script gating, in a single file, no external requests.

**How do I read the visitor's choice?**
`RetroCookie.getConsent()` returns the stored record (its `categories` holds the four booleans) or `null` if no valid choice exists. To react to changes, listen to `cookieconsent:change` on `document`; to run a script only with consent, give it `type="text/plain"` and a `data-cookie-category`.

**How do I show the banner again?**
`RetroCookie.open()` reopens the dialog with the saved toggles: put it behind a "Cookie settings" link in your footer. `RetroCookie.reset()` forgets the choice so the banner comes back on the next page load.

**Can I add a language?**
Yes — add one object to the `T` dictionary and one line to `detectLang()`. See [Adding a language](#adding-a-language).

**Can I change the colours or the font?**
Colours are four CSS variables on `.cookie-overlay` (`--cookie-bg`, `--cookie-ink`, `--cookie-paper`, `--cookie-accent`); the font stacks are `--cookie-font` and `--cookie-font-cjk`. Nothing else needs to change.

**Does it work on old browsers?**
Yes, from Safari 10.1 / Chrome 51 / Firefox 52 upwards — see [Browser support](#browser-support). The script is ES5 and uses only DOM APIs from that era, so it needs no transpiling.

**Why localStorage and not a cookie?**
It is the simplest client-side way to remember a preference: the record is not sent to the server with every request, there are no cookie attributes to get wrong, and your own scripts read it with one call. The difference is architectural, not legal: the ePrivacy rules cover `localStorage` just like cookies (storing the consent choice itself is strictly necessary and needs no consent). If you need the value server-side, mirror it into a cookie from the `cookieconsent:change` event.

**Upgrading from 1.0.0?**
The stored record changed shape and key, so visitors are asked once more; `localStorage.cookieConsent` from 1.0.0 is not read. Paragraph links moved from `<a href="#">` in the dictionary to `[label](key)` plus `urls` in the config, and `data-t-html` became `data-t-rich`. See [CHANGELOG.md](CHANGELOG.md).

## Contributing

Translations are the most useful contribution. See [CONTRIBUTING.md](.github/CONTRIBUTING.md); security issues go through [SECURITY.md](.github/SECURITY.md). Release history is in [CHANGELOG.md](CHANGELOG.md).

## Encoding

`banner/index.html` is UTF-8 and starts with a UTF-8 byte-order mark. The BOM takes precedence over the server's `Content-Type` charset, so the nine languages render correctly even on hosts that send `charset=windows-1251` or `iso-8859-1` by mistake.

If you copy the three blocks into your own page instead of using the file as is, that page must be served as UTF-8 (`<meta charset="UTF-8">` first in `<head>` and no conflicting HTTP header) — otherwise the non-Latin strings in the `T` dictionary will be garbled.

## Content Security Policy

The banner is inline CSS and JavaScript. A policy without `'unsafe-inline'` must allow the two blocks by nonce: put the same `nonce="…"` attribute, generated per response by your server, on the `<style>` and `<script>` tags you copied. Gated `<script type="text/plain">` tags carry their own nonce, and the banner copies it to the live script it creates, so `script-src 'nonce-…'` covers those too; `'strict-dynamic'` works as well. There is no external file to allow-list and no `eval`.

## Browser support

Chrome 51+, Firefox 52+, Safari 10.1+ (iOS 10.3+), Edge 79+, Samsung Internet 5+ — in practice everything since 2017. Newer CSS (`:focus-visible`, `overscroll-behavior`, `scrollbar-gutter`, safe-area `env()`, `zoom` on very large screens) is applied as progressive enhancement with fallbacks, so older browsers get the same dialog with slightly less polish. Internet Explorer is not supported.

## License

[MIT](LICENSE)
