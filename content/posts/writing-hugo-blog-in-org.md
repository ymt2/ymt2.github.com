+++
title = "Writing Hugo blog in Org"
author = ["ymt2"]
lastmod = 2021-01-03T05:58:36+09:00
tags = ["hugo", "org"]
categories = ["emacs"]
draft = true
weight = 2001
+++

## First heading within in the post {#first-heading-within-in-the-post}

-   This post will be exported as
    `content/posts/writing-hugo-blog-in-org.md`.
-   Its title will be "Writing Hugo blog in Org".
-   It will have _hugo_ and _org_ tags and _emacs_ as category.
-   The menu item _weight_ and post _weight_ are auto-calculated.
-   The menu item _identifier_ is auto-set.
-   The _lastmod_ property in the front-matter is set automatically to
    the time of export.


### A sub-heading under that heading {#a-sub-heading-under-that-heading}

-   It's draft state will be marked as `true` as the subtree has the
    todo state set to _TODO_.

With the point <span class="underline">anywhere</span> in this _Writing Hugo blog in Org_ post
subtree, do `C-c C-e H H` to export just this post.

The exported Markdown has a little comment footer as set in the _Local
Variables_ section below.

[Ox-Hugo](https://github.com/kaushalmodi/ox-hugo) を利用して出力した。
