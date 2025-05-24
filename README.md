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

Here's a table of paths, expected files to be loaded, and what files actually get loaded.

path: /
expected: index.html
actual (wrangler): index.html ✅
actual (vite): index.html ✅

path: /xyz
expected: xyz.html
actual (wrangler): xyz.html ✅
actual (vite): index.html ❌

path: /xyz/asdasd
expected: index.html
actual (wrangler): index.html ✅
actual (vite): index.html ❌

path: /abc/
expected: abc/index.html
actual (wrangler): abc/index.html ✅
actual (vite): index.html ❌

path: /abc/def
expected: abc/def.html
actual (wrangler): abc/def.html ✅
actual (vite): index.html ❌

path: /abc/def/ghi
expected: abc/index.html
actual (wrangler): index.html ❌
actual (vite): index.html ❌
