---
title: 'blogpub@v0.4.1: host media on github for your blog posts'
slug: host-media-on-github-for-your-blog-posts
date: "2022-02-21"
toc: true
tags: 
  - automation
  - github actions
  - ci
  - typescript
  - blogging
  - blogpub
_build:
 list: false
 render: false
published: true
---
Hey everyone, this is just a small update on the [blogpub](https://github.com/marketplace/actions/blogpub) action.

## Relative paths

In release [v0.4.1](https://github.com/protiumx/blogpub/releases/tag/v0.4.1) I added support for relative 
paths which means you can now use **github** to host the media you use in your blog posts.
How does it work? Just use relative paths (to the markdown file) and you are ready to go.
E.g.
![magic](./magic.gif)

The action replaces all relative paths with the **raw content** github urls.
In the example `./magic.gif` is replaced with `https://raw.githubusercontent.com/protiumx/blog/articles/blog-002/magic.gif`.
Check out the code [here](https://github.com/protiumx/blogpub/blob/main/src/parser.ts#L31).

That's all!

What other feature would you find useful for this action?
Let me know in the comments.

Related articles:
- [Publish your blog articles everywhere with this github action]({{<ref "/posts/001/index.md" >}})
