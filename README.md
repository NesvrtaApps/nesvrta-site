# Nesvrta LLC — nesvrta.com

Static marketing site. Three pages, no build step, no frameworks,
no analytics, no cookies, no JavaScript.

## Files

    index.html      home page
    privacy.html    Privacy Notice (accurate to this site's behavior)
    terms.html      Terms of Use (accurate to this site's behavior)
    styles.css      all styling, brand tokens at the top
    robots.txt      allows indexing
    sitemap.xml     three pages
    CNAME           custom-domain file for GitHub Pages (nesvrta.com)
    assets/         brand SVGs + og-image.png + favicon

## Brand rules baked in

- The wordmark and mark are inlined SVGs — never re-typeset the name.
- Palette: Ink #16130F · Paper #F4F0E7 · Clay #C4552E · Stone #8C8578.
  Tokens live at the top of styles.css. Dark mode swaps Ink/Paper
  automatically via prefers-color-scheme.
- Wordmark renders ≥120px wide (min is 90px). Footer mark 26px (min 24px).

## Editing

Open any .html file and edit text directly — it's plain HTML.
To change colors or spacing, edit the :root tokens in styles.css.

## Local preview

Open index.html in a browser. That's it.
