<!-- .github/copilot-instructions.md - Project-specific guidance for AI coding agents -->

# Shahira (Yummy template) — Copilot instructions

Purpose: give an AI coding agent immediate, actionable context for working on this repository (static website template + simple PHP contact forms).

- **Big picture**: This is a mostly-static Bootstrap template (original: "Yummy" from BootstrapMade) served as plain HTML/CSS/JS. Dynamic behavior is limited to front-end widgets and two PHP form endpoints in `forms/` that delegate to a third-party helper (`assets/vendor/php-email-form/php-email-form.php`) if present.

- **Major entry points**:
  - `index.html` — single-page site, contains vendor `<link>` and `<script>` includes and the primary markup.
  - `assets/js/main.js` — main client-side behavior (AOS init, GLightbox, Swiper init, scrollspy, mobile nav).
  - `assets/css/main.css` — primary styling; vendor CSS under `assets/vendor/`.
  - `forms/contact.php` and `forms/book-a-table.php` — PHP endpoints used by the contact and booking forms.

- **Why it’s structured this way**: The template is intended for simple hosting without a build step. Assets are pre-bundled under `assets/vendor/`. JavaScript expects DOM-based config (see Swiper pattern below) and CSS is globally applied — avoid introducing module bundlers unless you add a clear build workflow.

- **Key patterns and gotchas (concrete examples)**
  - Swiper is configured in the DOM: an element with class `init-swiper` must include a child element `.swiper-config` containing JSON (see `assets/js/main.js` function `initSwiper`). When modifying sliders, update that JSON in the markup rather than editing `main.js`.
  - Lightbox: use anchor links with class `glightbox` (e.g., gallery and menu images) — GLightbox is initialized in `assets/js/main.js`.
  - PureCounter and AOS: initialized on window `load` in `main.js`; if adding elements that rely on them, ensure they exist before `load` or re-init after injecting DOM.
  - Mobile nav: toggles by adding `mobile-nav-active` to `body`; replicate that CSS hook if adding new nav behaviors.

- **Forms & server-side behavior**
  - `forms/contact.php` and `forms/book-a-table.php` expect `assets/vendor/php-email-form/php-email-form.php` to be present. If missing, the PHP endpoints `die()` with an explanatory message (see top of each file).
  - To test the PHP forms locally, run the built-in PHP server from the project root so the `forms/` paths resolve:

```bash
php -S 0.0.0.0:8000 -t .
```

  - Update the receiving address by editing the `$receiving_email_address` in the PHP files. SMTP credentials can be injected by uncommenting the `smtp` array in those PHP endpoints.

- **Development / debug workflows**
  - Quick static preview: open `index.html` in a browser or run `python -m http.server 8000` for a simple static server.
  - To exercise PHP forms use the PHP built-in server command above.
  - There is no `package.json` or automated build; prefer lightweight edits. If you introduce a build tool, add a top-level `README.md` or `Makefile` documenting commands.

- **Files to edit for common tasks**
  - Change logo: `assets/img/shahira_logo.png` (and markup in `index.html`).
  - Update styles: `assets/css/main.css` (override vendor CSS here).
  - Update client behavior: `assets/js/main.js`.
  - Add/replace vendor libs: `assets/vendor/` (keep versions and filenames consistent with `index.html` includes).

- **Integration points / external dependencies**
  - Vendor libraries included directly under `assets/vendor/`: Bootstrap, AOS, GLightbox, Swiper, PureCounter. Do not remove files without updating `index.html` script/style includes.
  - PHP email helper expected at `assets/vendor/php-email-form/php-email-form.php` (not included in this repo). See `forms/Readme.txt` for template author notes.

- **When changing functionality, check these locations**
  - If altering page load/order behavior: `index.html` includes order (vendor CSS → main CSS; vendor JS files are included before `assets/js/main.js`). Keep this load order unless intentionally changing dependency load timing.
  - When adding sliders: follow the DOM JSON approach used by existing `init-swiper` elements.

- **Example — locate and update the Swiper JSON in markup**
  - Search `index.html` for elements with `class="init-swiper"` and edit the nested `.swiper-config` innerHTML JSON to change autoplay, slidesPerView, etc.

If anything above is unclear or you'd like me to expand or add examples (e.g., a small test page for forms or a scripted local server entry in a `Makefile`), tell me which area to expand and I will iterate.
