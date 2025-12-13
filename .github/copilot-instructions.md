## Quick orientation

This is a small, static personal portfolio website. There is no build system or server-side code in this repo — the site is a single-page HTML (`index.html`) with CSS (`styles.css`) and two profile images (`my-profile.jpg`, `my-profile2.jpg`). JavaScript lives inline in `index.html`.

Keep changes minimal and conservative: most edits are visual or content updates. If you modify behavior, update the in-file JS comments and keep network interactions stable.

## Big picture (what matters)
- index.html: single-page app. Contains the interactive logic (menu toggle, smooth scroll, modal form, image swap) and constants used for remote integrations.
- styles.css: full visual system (CSS variables, responsive breakpoints). Prefer editing variables at the top (`:root`) for color/spacing changes.
- Images and assets live at the repository root — update references in `index.html` when renaming.

## Key integration points (edit with care)
- APPS_SCRIPT_URL (in `index.html`): a Google Apps Script endpoint used for CV request counting and form submission. Search for the constant name `APPS_SCRIPT_URL` to find it. Changing this URL will alter where form submissions and the CV count fetch go.
- portfolioUrl (share buttons, in `index.html`): points to the source repo by default (`https://github.com/weeraboonkla/portfolio`). Verify this matches the public deployment URL used by GitHub Pages; if the site is hosted at the repository root (e.g. `portfolip.github.io`) or a Pages path, update accordingly.

## Important runtime/verification notes
- There is no build step. To preview locally, open `index.html` in a browser or serve the folder with a static server (example):

```bash
python -m http.server 8000
# then visit http://localhost:8000/
```

- Network behavior:
  - GET: `${APPS_SCRIPT_URL}?action=count` should return JSON of shape `{ "count": <number> }`.
  - POST: the CV request form posts FormData to `APPS_SCRIPT_URL`. The client accepts either valid JSON `{ "status": "success" }` or non-JSON text (code treats non-JSON OK responses as success). Tests should exercise both code paths.

## Project-specific patterns and conventions
- Inline JavaScript: Most interactive logic is inside `<script>` at the bottom of `index.html`. When adding features, prefer small, self-contained functions near related DOM markup.
- Mobile nav: toggles `.navbar.active` and swaps `#menu-icon` classes (`fa-bars` <-> `fa-times`). To change mobile behavior, update both the JS toggle and the `.navbar` CSS rules under `@media (max-width: 600px)`.
- CSS theming: uses CSS variables in `:root` (colors, spacing, image sizes). For global theme changes, edit variables at the top of `styles.css`.
- Accessibility/robustness: the code does light validation for email (`includes('@')`) and falls back on default counts (`'40+'`) if the Apps Script is unreachable. If you improve validation, keep the user-facing messages in the modal consistent with the existing `showMessage` helper.

## Quick edits examples (where to change things)
- Change the CV Apps Script endpoint: edit the `APPS_SCRIPT_URL` constant near the top of the inline script in `index.html`.
- Change the share URL: update the `portfolioUrl` assignment inside `setupShareButtons()`.
- Adjust hero text or CTA labels: edit the HTML inside the `.hero-content` section in `index.html`.

## Tests and debug tips for agents
- Use browser devtools (F12) to inspect console logs; the client logs network errors when fetch fails.
- To simulate a failing Apps Script response, run a tiny local server that responds with controlled JSON/text and point `APPS_SCRIPT_URL` to it.
- Verify responsive behavior by resizing the browser and checking `.navbar` slide-in, `.hero-image-container` layout and the swap-image interval.

## When to ask the repo owner
- If you need a canonical deployment URL (to correct `portfolioUrl`).
- If you need write access to the Google Apps Script or sample API responses for local testing.

If anything above is unclear or you need sample API responses or local mocks, ask and I will add them.
