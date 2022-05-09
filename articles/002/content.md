---
tags: automation, github, ci, typescript, blogging
published: true
---

# blogpub@v0.4.1: host media on github for your blog posts

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
