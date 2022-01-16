---
title: Publish your blog articles everywhere with this github action
tags: ci, github, typescript, blogging
published: true
---
Long ago I made this [comment](https://dev.to/protium/comment/clno) in this [article](https://dev.to/maxime1992/manage-your-dev-to-blog-posts-from-a-git-repo-and-use-continuous-deployment-to-auto-publish-update-them-143j):

```
This is a really good idea with great potential. 
Imagine a standarized API for different blogs. 
You can automatize publishing and editing, multiple collaborators.
And use a git provider as unique source of content.

And you can also make your git repo as some sort of blog.
I'll use it for my next posts for sure.

Thanks!
```

It was an idea that I had left on the back of my head and I didn't come back to it because I wasn't writing articles actively. But this has changed in the last months and I have published 6 articles during December. So I decided to revisit the idea and finally develop it.

## blogpub

[blogpub](https://github.com/marketplace/actions/blogpub) is a **github action** developed with `typescript` with the purpose of allowing you to use a `github` repository as **source of truth** for your blog posts. The action supports:
- Automatically publishing to `Medium` and `DEV.to`
- Templating
- Metadata/config section (published, tags, title, etc)

In fact, this article was automatically published by `blogpub` and you can see it's source in this [folder](https://github.com/protiumx/blog/tree/main/articles).

## Template Support

Sometimes we want to add different content to our blog posts depending on the platform, for instance for `dev.to` I want to use `liquid tags`, which `medium` doesn't support.
For this purpose I have used [handlebars](https://handlebarsjs.com/) to make use of conditionals. Example:
```md
\{{#if medium}}
This is only for Medium
\{{/if}}
\{{#if devto}}
This is only for DEV.to
\{{/if}}
```

For the first release of the action, the template context contains:
```ts
{
  medium: boolean;
  devto: boolean;
}
```

## Article metadata

I have added support for a **metadata** section (similar to dev.to) where we can specify the following attributes:

- `title`: `[string]` The title of the article. If not specified, the **first** H1 heading will be used.
- `description`: `[string]` Description for `dev.to` API.
- `tags`: `[string]` Comma separated tags. Note: Medium allows up to 5 tags whereas Dev.to only 4.
- `license`: `[string]` Medium license type. Refer to [Medium API Docs](https://github.com/Medium/medium-api-docs#33-posts). **Default**: `public-domain`
- `published`: `[boolean]`. **Default**: `true`

Example:
```md
---
title: First blogpub test
tags: test, ci
---
# I'm using `blogpub`
```

## Adopting CI/CD practices for blogging

Imagine the following scenario:
- Write an article
- Create PR
- CI checks the **spelling**
- Your peers review the article and propose changes or approve it
- Merge the PR to `main` and it gets published everywhere
- Use [send tweet action](https://github.com/marketplace/actions/send-tweet-action) to promote your new article.

I can imagine this workflow in a company where different colleagues write articles together. I like the idea that we could also automatize tweets, this is why `blogpub` outputs the URLs of the article after publishing it.

## Usage
Ideally you want to run this action for every push to your `main` branch. This setup should be enough.
```yml
name: 'publish'

on:
  push:
    branches:
      - 'main'
    paths:
      - 'articles/*'
jobs:
  publish:
    name: publish new article
    runs-on: ubuntu-latest    
    steps:
      - name: Checkout
        uses: actions/checkout@v2
      - name: blogpub
        uses: protiumx/blogpub@v0.2.3
        with:
          articles_folder: articles
          devto_api_key: ${{ secrets.DEVTO_API_KEY }}
          gh_token: ${{ secrets.GH_TOKEN }}
          medium_token: ${{ secrets.MEDIUM_TOKEN }}
          medium_user_id: <user_id>
```
Few things to take into account:
- We only want to run the action for files inside a folder.
- You need to setup repository secrets with your tokens and api keys.

Check this [blog source](https://github.com/protiumx/blog) to see this in action.

## What is missing?

For the first release I just wanted to be able to publish the articles in Medium and DEV.to. But there are a few features that would be handy:

- **Uploading images:** Imagine also versioning images and then uploading those images automatically
- **Support for updates:** If you want to make a correction, it should be possible to update the source and it should get reflected in all the platform the article was published to.
- **Platform specific configuration**: maybe we want different tags per platform
- **Support multiple articles:** at the moment `blogpub` publishes only 1 article per run.

What else would you like to have? Let me know in the comments!


That's it!
As usual, any help is well received and I have a [TODO](https://github.com/protiumx/blogpub#todo) list if you would like to collaborate with this project.

{{#if devto}}
{% github protiumx/blogpub %}
{{else}}
[blogpub repo](https://github.com/protiumx/blogpub)
{{/if}}

:robot:
