# today i learned

Running list of little interesting things I learn. Uninteresting things not included. Not guaranteed to be interesting for all / any audiences.

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

### git is case sensitive, but if you're on a Mac, your file system isn't

`git status` will show a moved file on a case-sensitive system, but not here.

```
~/tmp/example〉git init .
Initialized empty Git repository in /Users/tmcw/tmp/example/.git/
~/tmp/example〉touch Hi
~/tmp/example〉git add Hi
~/tmp/example〉git commit -m "First"
[master (root-commit) c09565f] First
 1 file changed, 0 insertions(+), 0 deletions(-)
 create mode 100644 Hi
~/tmp/example〉mv Hi hi
~/tmp/example〉git status
On branch master
nothing to commit, working tree clean
```

### rgba and hsla isn't actually necessary

In CSS Color Module 4, [rgba and hsla](https://drafts.csswg.org/css-color/#rgb-functions) are officially 'legacy' - you can add an alpha component without adding the `a` to either - just use `rgb()` and `hsl()` always.

### Browser search is not literal and equates certain characters

If you're in _extreme pedant mode_ and want to find any place where you've typed an errant `'` instead of a curly `’`, and you use Google Chrome's built-in search, you'll be surprised to see that - it matches both, regardless of which character you type. Apparently the search aliases certain common characters, because there are more people who would be upset about some quote-containing search to _not_ return results based on typographical pedantry than people who would want to search for non-curly quotes only.

### The default google analytics tracking snippet is sort of wrong

```html
<!-- Google Analytics -->
<script>
window.ga=window.ga||function(){(ga.q=ga.q||[]).push(arguments)};ga.l=+new Date;
ga('create', 'UA-XXXXX-Y', 'auto');
ga('send', 'pageview');
</script>
<script async src='https://www.google-analytics.com/analytics.js'></script>
<!-- End Google Analytics -->
```

Spot the weirdness. Well: if you have some script that's sync or that's async but loads _before_ google analytics, and that script changes the page location, then the first page that's tracked is not the page URL that's loaded. We throw in a

```js
ga("set", "page", location.pathname + location.search);
```

To make sure the first-page-location-loaded is tracked.

### console.log ain't sync

Okay, I knew this one before, but it's always good to remember: console.log isn't sync. If you console.log something and then immediately modify the thing, console.log is likely to show the modified version, not the version at the time of the `console.log` call. Beware.

### smartypants isn't always right

There are some tough cases in English punctuation: see [this issue in the marked repository](https://github.com/markedjs/marked/issues/166):

> I won't do it 'cause I don't want to.

This should have:

* won’t
* 'cause
* don’t

The commonplace smartypants option isn't smart enough to parse that, but implementations like [markdown-it](https://github.com/markdown-it/markdown-it) and Medium's online editor can do very advanced quote formatting.

### content-type is a weird attack vector

> An attacker could upload a specially crafted JPEG file that contained script content, and then send a link to the file to unsuspecting victims. When the victims visited the server, the malicious file would be downloaded, the script would be detected, and it would run in the context of the picture-sharing site.

[Here's one blog post](https://blogs.msdn.microsoft.com/ie/2008/07/02/ie8-security-part-v-comprehensive-protection/). It's weird, because before even if you declared a content-type, IE would still try to auto-detect a content type, and might re-interpret the file as something different. So you serve a file that contains JavaScript-looking content, but with an `image/jpg` mime-type, IE would treat it as a script. So they:

- stopped sniffing image/ content
- let you turn sniffing off

### along with the set-cookie http header, there's a historical set-cookie2 header

https://developer.mozilla.org/en-US/docs/Web/HTTP/Headers/Set-Cookie2

It's just weird, that's all.
