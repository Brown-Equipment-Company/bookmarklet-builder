# Bookmarklet Builder

A single static page that turns a URL into a bookmarklet. Clicking the bookmarklet
opens that URL with a fresh random query parameter each time, so the browser (and
anything caching in between) has to fetch the page rather than serve a stored copy.

Useful for dashboards, status pages, and internal tools that cache harder than you
would like.

## Use it

**<https://brown-equipment-company.github.io/bookmarklet-builder/>**

No build step, no dependencies, nothing leaves the browser — `index.html` works
just as well opened straight from disk.

1. Paste the URL.
2. Drag the generated button onto your bookmarks bar, or copy the code and paste it
   as the address of a hand-made bookmark.

## Prefilled links

The form reads its own query string, so you can send someone a link that arrives
ready to drag instead of asking them to fill anything in:

| Parameter | Meaning | Default |
| --- | --- | --- |
| `url` | The target URL | — |
| `name` | Text on the bookmarklet button, which becomes the bookmark's name | the host name |
| `param` | Cache-busting parameter name | `cb` |

```
https://brown-equipment-company.github.io/bookmarklet-builder/?url=https%3A%2F%2Fexample.com%2Freport%3Fteam%3Dops&name=Ops%20Report
```

Each value must be URL-encoded, since the target URL usually has its own `?`, `&`
and `#` in it. You do not have to do that by hand — fill the form in once and the
page shows the matching prefilled link with a **Copy** button.

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

GitHub Pages serves `main` from `/`, so anything pushed to `main` is live within
a minute or so. It is one self-contained file, so any other static host works too.
