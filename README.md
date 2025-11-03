# lao

<img src="img/logo.png" alt="Lao's logo" width="200"/>

A simple static site for managing URL redirects. It's a manual way to create shortened URLs, like `lao.bz/alias`. I don't rely on existing services such as TinyURL because I have specific needs:

- A free, centralized place to keep track of all my redirects
- Full control over redirect aliases without the risk of them being already taken
- The ability to delete my redirects whenever I want
- Optional descriptions associated with each redirect
- A single page displaying all redirects
- Tags and lists

**Note:** These are HTML-based redirects, not real HTTP redirects. This is sufficient for my purposes.

---

## Fork this if you need it

This project was primarily created for personal use, but feel free to fork and adapt it to your own needs.

Each redirect is stored in the `_redirects` folder as a `.md` file. The filename serves as the *alias* that maps to the target URL. Since the site is served from `lao.bz`, the filename becomes `lao.bz/filename`, which is short enough.

There are two redirect types, each implemented through a different Jekyll `layout`:

1. `redirect`: maps an alias to a target URL. The filename (without the extension) is the alias.

Files use the following structure:

```yaml
---
layout: redirect
url_target: <url/to/redirect/to>
description: <optional description>
tags: <optional list of tags>
---
```

For example (`gl.md` into `_redirects`):

```yaml
---
layout: redirect
url_target: https://google.com
description: A famous search engine
tags: [search]
---
```

creates a redirect to `https://google.com` from:

```
yourusername.github.io/lao/gl
```

2. `list`: a page that displays multiple existing redirects. The filename (without the extension) becomes the alias to this list page.

Files use the following structure:

```yaml
---
layout: list
redirects: [list or existing redirects]
description: <optional description>
tags: <optional list of tags>
---
```

For example (`socials.md` into `_redirects`):

```yaml
---
layout: list
redirects: [lk, fb, x]
description: My social pages
tags: [social]
---
```

creates a redirect to `yourusername.github.io/lao/socials` to a page that shows these redirects, if existing:

```
yourusername.github.io/lao/lk
yourusername.github.io/lao/fb
yourusername.github.io/lao/x
```

You can also include lists inside other lists as expected. Example:

```yaml
---
layout: list
redirects: [another-redirect, socials]
description: My social pages
tags: [social]
---
```

### Show all redirects

`lao` provides a [`/all`](https://lao.bz/all) page that lists all the available redirects.

This page supports a few basic filtering capabilities:

- filtering by list, by clicking any list button
- filtering by tag, by clicking a tag
- a query parameter `?tag=<tag-name>` for sharing the filtered page
- a search bar for free-text search

**Important**: Do not create a redirect named `all.md`, or the build will trigger a warning and this feature will not work.

### Build and run locally

You can build and serve the website locally using Docker and Python:

```shell
git clone https://github.com/ilpropheta/lao.git
cd lao
docker run --rm -v %cd%:/srv/jekyll jekyll/jekyll jekyll build
python -m http.server --directory _site 4000
```

Then visit `http://localhost:4000/alias` to see that in action. The list of all redirects will be at `http://localhost:4000/all`.

# Who is Lao?

[Lao Tzu](https://en.wikipedia.org/wiki/Laozi) (or *Laozi*) is generally considered the founder of *Taoism*, an ancient philosophy that values simplicity of living and harmony with nature.