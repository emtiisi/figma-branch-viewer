# Copilot / AI agent instructions for this repo

Overview
- This is a tiny single-page static UI that queries the Figma REST API and lists branches per file. The entire app lives in a single HTML file: [index.html](index.html).
- No build system, no server-side code. Changes are made directly to `index.html`.

Big-picture architecture (what to know)
- Single-file client app: UI, styles, and all JS are inline in [index.html](index.html). Key behaviors are implemented in DOM-manipulation functions (fetch → render flow).
- Data flow: user provides a Figma token/team or project ID → `doFetch()` calls Figma endpoints (`/teams/:id/projects`, `/projects/:id/files`, `/files/:key`) → branches collected → cached in `localStorage` → rendered into DOM.
- Caching: the app uses `localStorage` to persist fetched results (search for `localStorage` in `index.html`).

Important files and examples
- Main file: [index.html](index.html) — read/modify here. Example runtime functions to reference when editing:
  - Fetch + orchestration: `doFetch()` (collects projects, files, branches)
  - Progress UI: `updateProgress()` (controls progress bar and messages)
  - Rendering: `render()` and `filterCards()` (DOM creation for file cards and branches)

Project-specific conventions & patterns
- Everything is in one HTML file: prefer small, incremental changes rather than massive refactors.
- Visual tokens live in CSS variables at `:root` (colors, fonts). Preserve these when editing styles to keep the theme consistent.
- Strings/UI are in Turkish; keep new UI text consistent with the existing language unless asked otherwise.
- Network errors surface via `showError(msg)` and the `#status` element. Use that pattern for user-facing errors.

Running & debugging locally (how to preview)
- There is no build step — open `index.html` in a browser, or serve it locally to avoid CORS restrictions:

```bash
# from repo root
python3 -m http.server 8000
# then open http://localhost:8000 in a browser
```

Debug tips
- Use the browser DevTools console to inspect `allData`, `isFetching`, and `localStorage` keys.
- To simulate API responses, either enter a valid Figma token in the UI or stub network calls in DevTools (or modify `doFetch()` locally to return mocked data).

Security / token handling
- The UI expects the user to paste a Figma token at runtime (it is sent as `X-Figma-Token`). Do NOT commit tokens or secrets. If adding developer helpers, read tokens from environment only in local developer scripts — do not add hard-coded tokens to `index.html`.

When adding features
- Keep logic synchronous/linear as current code does (explicit async/await phases with progress updates). Follow existing function boundaries: fetch → updateProgress → render.
- If extracting JS to separate files, keep changes minimal and preserve the same global DOM IDs and CSS variables to avoid breaking selectors.

Searchable anchors for quick edits
- Search for `doFetch`, `updateProgress`, `render`, `localStorage` and `showError` when locating main behaviors.

If something is unclear
- Tell me which part you'd like expanded (UI flows, caching details, or adding tests). I can iterate on this file with targeted examples or propose a small refactor into `app.js` + `styles.css` and accompanying run instructions.
