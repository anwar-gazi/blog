---
layout: post
title:  "Create a documentation site with github-pages, jekyll and just-the-docs theme"
date:   2021-07-14 10:00:00 +0600
categories: github-pages jekyll
---
Assuming you installed Ruby and Bundler first. Check the [prerequisite section here if you haven't already](https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll)
<br>
<br>
Go to the directory you want to keep the site in. 
For example, I will be putting in `docs`. 
Now create jekyll site with `jekyll new . `
This created a jekyll site in the current directory.

Open the Gemfile that jekyll created.
<br>Comment out the line that has `gem "jekyll"`. So that it looks like `#gem "jekyll"`
<br>Specify `just-the-docs` theme by adding this line in `Gemfile`: `gem "just-the-docs"`.
<br>Close the Gemfile.
<br>Open _config.yml and set theme in the line: `theme:minima` to `theme:just-the-docs`
<br>execute: `bundle` or `bundle update`
<br>Optionally, make other changes in the `_config.yml` file for values like `title, email, description, url, baseurl, twitter_username, github_username` etc.
<br>Close the `_config.yml` file.
<br>Test the site with `bundle exec jekyll serve`
<br>Visit `http://127.0.0.1:4000/docs/`

<br>
<br>
Acknowledgements:
<br>https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/creating-a-github-pages-site-with-jekyll
<br>https://docs.github.com/en/pages/setting-up-a-github-pages-site-with-jekyll/testing-your-github-pages-site-locally-with-jekyll
<br>https://github.com/pmarsceill/just-the-docs