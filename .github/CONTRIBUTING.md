# Contributing

Thanks for your interest. The project is deliberately small — one HTML file — so contributions are easy to review.

## Bug reports

Open an issue with the browser and version, the `?lang=` value you used, what you expected and what happened. A screenshot helps.

## Translations

The most welcome contribution. To add or fix a language:

1. Add or edit the language object in the `T` dictionary inside `banner/index.html`. Every key must be present; copy the `en` object as a template.
2. Register the locale codes in `detectLang()` (see the "Adding a language" section of the README).
3. Add a row to the *Live demo* and *Language detection* tables in `README.md`, and a link on the demo page (the `.page` block in `banner/index.html`).
4. Bump the number in the `languages` badge.

Keep `langName` in the language itself (`Deutsch`, not `German`). British spelling for English.

## Code changes

- Plain HTML, CSS and ES5-compatible JavaScript; no build step, no dependencies. That constraint is the point of the project.
- Match the existing style: two-space indent, BEM-style `.cookie__*` classes, comments in English.
- Test at 320 px, 390 px, desktop and ≥1600 px, in at least one non-Latin language.
- Keep the file self-contained: no external fonts, scripts or images.

## Pull requests

One change per PR, with a short description of what and why. Screenshots for anything visual.
