# Security

The banner runs entirely in the browser, makes no network requests and stores only the visitor's consent choice in `localStorage`. It reads the `?lang=` URL parameter but never injects it into the page — the value is only compared against a fixed list of locale codes.

If you find a security issue, please open a [GitHub issue](https://github.com/AbbeyLubber/retro-cookie-banner/issues) describing the problem and the steps to reproduce it. There is no bug bounty, but reports are read and fixed promptly.
