# gotchas

Running list of underdocumented pitfalls.

### Safari 10 and 11 don't _actually_ support let and const

All of the support tables are lying: Safari 10 and 11 parse let and const, but if you try to rely on their block scoping, they break.

The fix: enable the `safari10` option in uglify-es.

### Firefox handles CSP `style-src` differently than Chrome

In Chrome, you can set style-src to just `somedomain.com` and it'll work. In Firefox, unless you set `unsafe-inline`, `style=""` attributes will break.

### Monospace is only as monospace as its character set

Let's say you have a monospace-formatted table, so all the `|` should align. If any value in any column includes a character that isn't supported by the monospace font, the table won't be aligned. Example:

```
| h |
| ⚰️ |
```
