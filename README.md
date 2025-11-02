# lao

A simple static site for managing (long) URL redirects. It's a manual way to create shortened URLs, like `lao.bz/alias`. I don't rely on existing services such as TinyURL because I have specific needs:

- A free, centralized place to keep track of all my redirects
- Full control over redirect aliases without the risk of them being already taken
- The ability to delete my redirects whenever I want
- Optional descriptions associated with each redirect
- A single page displaying all redirects
- Possibly in the future: support for tags or categories to group redirects

**Note:** These are HTML-based redirects, not real HTTP redirects. This is sufficient for my purposes.

---

## Fork this if you need it

This project was primarily created for personal use, but feel free to fork and adapt it to your own needs.

All redirects are stored as `.md` files with `layout: redirect`. A redirect file looks like this:

```yaml
---
layout: redirect
url_target: <url/to/redirect/to>
description: <optional description>
---
```

Each file represents a redirect. The filename (without the extension) becomes your alias. For example:

```
yourusername.github.io/lao/filename
```

Since I serve this website from `lao.bz`, the URL becomes `lao.bz/filename`, which is convenient.

### Show all redirects

This website also renders a page listing all available redirects at `/all`.

**Important**: Do not create a file named `all.md`, or the build will trigger a warning and this feature will not work.

In the future, I may add support for categories or tags to group related redirects, possibly allowing URLs like `/all/tag`. This is not supported yet.

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

[Lao Tzu](https://en.wikipedia.org/wiki/Laozi) (or *Laozi*) is generally considered the founder of *Taoism*, an ancient philosophy that values simplicity of living.