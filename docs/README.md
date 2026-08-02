# Tailwind build (replaces the CDN script)

This folder builds `../css/tailwind.css`, which `index.html` now links to
directly instead of loading `https://cdn.tailwindcss.com` at runtime.

## Folder layout expected

```
public/
  index.html
  css/
    tailwind.css       <- generated, committed to the repo
  build-tools/
    package.json
    input.css
    node_modules/      <- NOT committed (see .gitignore below)
```

## One-time setup

```powershell
cd public/build-tools
npm install
```

## Whenever you add/change Tailwind classes in index.html

```powershell
npm run build:css
```

This re-scans `../index.html` for class names and regenerates
`../css/tailwind.css`. Commit the updated `tailwind.css` along with your
HTML changes — that compiled file is what actually ships to users.

## Optional: auto-rebuild while editing

```powershell
npm run watch:css
```

Leave this running in a terminal while you work; it rebuilds on every save.

## Add this to your repo's .gitignore

```
public/build-tools/node_modules/
```

`node_modules` is large and fully reproducible from `package.json` +
`package-lock.json`, so it shouldn't be committed.

## Why this exists

The previous setup loaded Tailwind from `https://cdn.tailwindcss.com` at
runtime. Tailwind's own docs say this script "should not be used in
production" — it ships the full framework and compiles on the fly in the
browser instead of shipping only the CSS you actually use. This build step
generates a small, static, versioned CSS file instead, pinned to Tailwind
`4.3.3` so an upstream update can't silently change your site's styling.
