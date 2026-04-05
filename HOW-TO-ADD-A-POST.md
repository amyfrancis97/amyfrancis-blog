# How to add a new blog post

## 1. Create a new file in the `_posts` folder

Name it: `YYYY-MM-DD-your-post-title.md`
Example: `2026-04-10-my-new-post.md`

## 2. Paste this at the top of the file (the "front matter")

```
---
layout: post
title: "Your Post Title Here"
description: "One or two sentences describing the post - shown on the homepage card."
standfirst: "The opening paragraph shown under the title on the post page."
category: opinion
category_label: "Opinion"
date: 2026-04-10
read_time: 8
---
```

## Categories
Change `category` and `category_label` to one of:

| category   | category_label        |
|------------|-----------------------|
| opinion    | Opinion               |
| industry   | Industry & Research   |
| technical  | Technical             |
| tutorial   | Tutorials & Guides    |

## 3. Write your post below the front matter

Use normal Markdown:
- `## Heading` for section headings
- `### Subheading` for subheadings
- `**bold**` for bold text
- `*italic*` for italic
- `> quote text` for blockquotes

## Special elements (copy and paste as needed)

**Divider line:**
```html
<div class="divider"></div>
```

**Callout box (green left border):**
```html
<div class="callout">
  <div class="callout-label">Worth knowing</div>
  <p>Your callout text here.</p>
</div>
```

**Source note:**
```html
<div class="source-note">Source: Your citation here.</div>
```

**Experience/case study box:**
```html
<div class="experience-box">
  <div class="exp-label">Event or context name</div>
  <div class="exp-title">Project title</div>
  <p class="exp-body">Description of the experience.</p>
</div>
```

## 4. Push to GitHub

Once your file is saved, push it to the GitHub repo and it will publish automatically.
