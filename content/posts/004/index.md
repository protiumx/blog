---
title: Automate an articles section in your github.io page
date: "2022-05-13"
slug: automate-an-articles-section-in-your-github-io-page
toc: true
tags: 
  - ci, 
  - github actions
  - github pages
  - automation
  - profile
cover_image: ./cover.png
published: true
---

Today I wanted to update my [github page](https://protiumx.github.io/) to show a list of my
last Medium [articles](https://medium.com/@protiumx). I ended up automating it.

## First approach

My first attempt was fetching the RSS feed using the URL `medium.com/feed/@username` and parse
the `xml` document. But I was hit with a charming `CORS` error.
Now what?

I remembered that I'm using a github action to update a similar section on my [github profile](https://github.com/protiumx).
So, what if I tell this action that the README file is called `index.html`?

## blog-post-workflow

The [github action](https://github.com/gautamkrishnar/blog-post-workflow) supports a `readme_path` parameter. After 
a quick dive in its source code I noticed that this file could be anything, not necessarily a markdown file. Problem solved!

Let's add the articles section into the html:
```html
<section class="content__articles">
  <p>&gt; last published articles:</p>

  <ul>
    <!-- BLOG-POST-LIST:START -->
    <!-- BLOG-POST-LIST:END -->
  </ul>
</section>
```

Since the action is intended for markdown files, the default template for the posts links is `[title](url)` but we need html code.
Luckily the github action also provides a `template` param. So let's change the template to generate list items:
```html
<li><a href="$url" target="_blank">$title</a></li>$newline
```

That's it!

Our workflow configuration should be:
```yml
name: Update Medium articles
on:
  schedule:
    - cron: '0 0 * * 0' # Runs once a week
  workflow_dispatch:

jobs:
  update-articles-section:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v3
      - uses: gautamkrishnar/blog-post-workflow@master
        env:
          GITHUB_TOKEN: ${{ secrets.GITHUB_TOKEN }}
        with:
          feed_list: "https://medium.com/feed/@protiumx"
          readme_path: "./index.html"
          template: '<li><a href="$url" target="_blank">$title</a></li>$newline'
```
Check the file [here](https://github.com/protiumx/protiumx.github.io/blob/main/.github/workflows/medium-articles.yml).
NOTE: I added the `workflow_dispatch` trigger so I can trigger the action from the github UI.

That's all. I few lines of code and we added an automated section for our github page (or any page).

Related articles:
- [Publish your blog articles everywhere with this github action]({{< ref "/posts/001/index.md" >}})

👽
