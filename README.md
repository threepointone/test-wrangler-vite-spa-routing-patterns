inconsistencies with routing patterns with single page app mode

---

This is a an attempt to list some bugs with the way we choose which html file to load when using single page app mode. Assume we have an assets directory that looks like this:

```
public/
  index.html
  xyz.html
  abc/
    index.html
    def.html
```

with a configuration (as in wrangler.jsonc) that looks like this:

```jsonc
{
  // ...
  "assets": {
    "directory": "public",
    "not_found_handling": "single-page-application"
  }
}
```

(Use `npm run wdev` and `npm run vdev` to run `wrangler dev` and `vite dev` respectively.)

Here's a table of paths, expected files to be loaded, and what files actually get loaded.

| Path         | Expected       | Actual (wrangler) | Actual (vite) |
| ------------ | -------------- | ----------------- | ------------- |
| /            | index.html     | index.html ✅     | index.html ✅ |
| /xyz         | xyz.html       | xyz.html ✅       | index.html ❌ |
| /xyz/asdasd  | index.html     | index.html ✅     | index.html ❌ |
| /abc/        | abc/index.html | abc/index.html ✅ | index.html ❌ |
| /abc/def     | abc/def.html   | abc/def.html ✅   | index.html ❌ |
| /abc/def/ghi | abc/index.html | index.html ❌     | index.html ❌ |
