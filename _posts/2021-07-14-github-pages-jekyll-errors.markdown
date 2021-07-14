---
layout: post 
title:  "Jekyll based github pages common errors and troubleshoot"
date:   2021-07-14 08:00:00 +0600 
categories: github-pages jekyll
---
We installed jekyll with `just-the-docs` theme for our [FreightForward Doc Software][ff_soft_code]
documentation [site][ff_doc_ghpages].

Upon github-pages deployment, we found the [site][ff_doc_ghpages] to be blank. So it was obvious that github-pages build
failed with jekyll errors.

Unfortunately the github-pages jekyll error debug section doc didn't help much, if not at all!
So we had to recheck our code and jekyll config along with googling for users with similar issues.

Upon our inspection on our code and config, we noted some issues.

## _config.yml values

Check the keys value in _config.yml file.

### theme, remote_theme

`just-the-docs` is not officially available for github pages. So along with the `theme` key in _config.yml, we
need `remote_theme` key also.

```yaml
remote_theme: pmarsceill/just-the-docs
```

### base_url, url

in local testing of jekyll(`jekyll serve`), `base_url` is empty mostly.
<br>For github pages, value to be in this format: https://<repo_owner_username>.github.io,
e.g., https://freightforward.github.io.
<br>`url` is the repo name, e.g., `docs`.

```yaml
baseurl: "/docs" # the subpath of your site, e.g. /blog
url: "https://freightforward.github.io" # the base hostname & protocol for your site, e.g. http://example.com
```

So the full url of the github-pages site is https://freightforward.github.io/docs

_repo_owner_username: Your github account username for most cases. When the repo owned an organization in your github
account, username will be that organization username, not your account username!_

## Jekyll errors

Check for any errors or warnings thrown when you run your site locally, i.e., `jekyll serve`.
<br>There could be warnings and the site would still work locally but fail in github-pages build.

## Directory Structure

Our site had pages grouped by a directory structure.
<br>There was a top level directory named `docs` under which there were some pages markdown files.
<br>|...
<br>|--- docs/ (index.md, page1.md, page2.md etc)
<br>|--- _config.yml
<br>|--- Gemfile
<br>|...

<br>In local testing, `base_url` was empty, but for github-pages its value set to be `/docs`.
<br>So the local site url was like http://127.0.0.1:4000/docs, remember the trailing /docs in that url comes from our
directory structure.
<br>So for github pages build `docs` comes two times in the url, one is from the repo name, another is from our site
structure directory.
<br>A potential clash was impending. (We are not 100% sure though)
<br>So we changed the directory structure just to be on safe side(As we didn't have debug time at that moment)

## Ide autoformatting

We used a jetbrains ide with markdown support, and we used to [reformat code][reformat_url] a lot.
<br>In fact, hitting `Ctrl`+`Alt`+`L` was very frequent.
<br>This worked well mostly, mostly!
<br>It messed the formatting yaml [front matters][frontmatter_doc], which in turn makes the page un-renderable for
jekyll. Beware!

[ff_soft_code]: https://github.com/minhajme/freightforward

[ff_soft_ghpages]: https://freightforward.github.io

[ff_doc_ghpages]: https://freightforward.github.io/docs

[justthedocs_gh]: https://github.com/pmarsceill/just-the-docs

[reformat_docs]: https://www.jetbrains.com/help/idea/reformat-and-rearrange-code.html

[frontmatter_doc]: https://jekyllrb.com/docs/front-matter/