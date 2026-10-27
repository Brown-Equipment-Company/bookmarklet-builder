# Bookmarklet Builder

A single static page that turns a URL into a bookmarklet. Clicking the bookmarklet
opens that URL with a fresh random query parameter each time, so the browser (and
anything caching in between) has to fetch the page rather than serve a stored copy.

Useful for dashboards, status pages, and internal tools that cache harder than you
would like.

## Use it

Open `index.html` — no build step, no dependencies, nothing leaves the browser.

1. Paste the URL.
2. Drag the generated button onto your bookmarks bar, or copy the code and paste it
   as the address of a hand-made bookmark.

## What gets generated

For `https://example.com/report?team=ops`:

```js
javascript:(function(){location.href="https://example.com/report?team=ops&cb="+Date.now().toString(36)+Math.random().toString(36).slice(2,8)})();
```

- The parameter name defaults to `cb` and can be changed.
- An existing parameter of the same name is replaced, so repeated clicks do not
  stack up `?cb=...&cb=...`.
- A `#fragment` is preserved and stays at the end of the URL.
- Only `http:` and `https:` URLs are accepted.

## Hosting

It is one self-contained file, so any static host works. To serve it from GitHub
Pages, enable Pages for this repository with `main` as the branch and `/` as the
folder.
