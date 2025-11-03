# lao

<img src="img/logo.png" alt="Lao's logo" width="200"/>

A simple static site for managing URL redirects. It's a manual way to create shortened URLs, like `lao.bz/alias`. I don't rely on existing services such as TinyURL because I have specific needs:

- a free, centralized place to keep track of all my redirects
- full control over redirect aliases without the risk of them being already taken
- the ability to delete my redirects whenever I want
- optional descriptions associated with each redirect
- a single page displaying all redirects
- tags
- link and redirect lists

**Note:** These are HTML-based redirects, not real HTTP redirects. This is sufficient for my purposes.

---

## Fork this if you need it

This project was primarily created for personal use, but feel free to fork and adapt it to your own needs.

Each redirect is stored in the `_redirects` folder as a `.md` file. The filename serves as the *alias* that maps to the target URL. Since the site is served from `lao.bz`, the filename becomes `lao.bz/filename`, which is short enough.

There are two redirect types, each implemented through a different Jekyll `layout`, explained here below.

### Redirect

The `layout: redirect` maps an alias to a target URL. The filename (without the extension) is the *alias*.

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

### Link and redirect list

`layout: list` creates a redirect that maps to a page on this website displaying a list of links defined in the file. The filename (without the extension) is the *alias*.

Files use the following structure:

```yaml
---
layout: list
description: <optional description>
tags: <optional list of tags>
links: 
  - https://google.com # <- this is just a link
  - gl # <- this is intended as an existing redirect
  - /gl # <- this is intended as an existing redirect that points to itself
  - title: github # <- this is just a link with some information
    url_target: https://github.com
    description: this is github's homepage  
---
```

`links` is not just a list of links but it's also working with existing `redirect`s stored in `_redirects`.
Thus, `links` support:
- external links (with optional metadata `title` and `description`)
- existing redirects on this website (metadata is taken from the corresponding redirect file and cannot be overridden)

Note the difference between `existing-redirect` and `/existing-redirect`: for both, `lao` displays the same title and description that are read from the corresponding redirect file. However, the resulting link will have to two different targets:

- `existing-redirect`, points to the mapped url of that redirect (e.g. `gl -> https://google.com`)
- `/existing-redirect`, points to the redirect itself (e.g. `gl -> /gl`)

This allows you to include frequently updated redirects in multiple lists without updating each list individually.

For example (`socials.md` into `_redirects`) - suppose we have `lk` and `fb` redirects in-place:

```yaml
---
layout: list
description: My social pages
tags: [social]
links:
  - title: x
    url_target: https://x.com/ilpropheta
  - lk  
  - /fb
---
```

creates a redirect to `yourusername.github.io/lao/socials` to a page that shows these links. The rendering should visualize something like:

```
- [x](https://x.com/ilpropheta), with an empty description
- [lk](https://www.linkedin.com/in/marcoarena), with the description found in the redirect `lk` (if any)
- [fb](/fb), with the description found in the redirect `fb` (if any)
```

You can also include lists inside other lists as expected. Example:

```yaml
---
layout: list
description: My social pages
tags: [social]
links:
  - title: x
    url_target: https://x.com/ilpropheta
  - lk  
  - /fb
  - suppose-this-is-another-list
---
```

Bear in mind that if an links entry is not an existing redirect then `lao` assumes is a general link, not performing any existance check. For example, if you mispell `lj` instead of `lk`, the resulting link will be `/lj`.

### Show all redirects

`lao` provides a [`/all`](https://lao.bz/all) page that lists all the available redirects. This is not intended to be shared with people but it's just for your reference (well, this is publicly accessible so can be shared but it's probably not useful).

This page supports a few basic filtering capabilities:

- filtering by list, by clicking any list button
- filtering by tag, by clicking a tag
- a query parameter `?tag=<tag-name>` for sharing the filtered page
- a search bar for free-text search

Such features are not available in link lists but might be in the future if someone thinks it can be useful.

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