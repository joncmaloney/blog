---
layout: post
title: Adding Screen Shots to Jekyll Posts.
summary: Simple guide to adding screen shots and making each blog post easier to follow.
author: jon_maloney
---

A screenshot or any image can be added to a jekyll page. If using markdown you add the following code. 

```markdown
![A screen shot of render images using liquid.]({{ site.baseurl | append: "/img/captures/jekyll-screen-shot.jpg" }})
```



![A screen shot of how to take screen shots in Jekyll.]({{ site.baseurl | append: "/img/captures/jekyll-screen-shot.jpg" | absolute_url }})

Notice the the source is created by appending the site.baseurl to the location of the image file. In this code the filter absolute_url has not been used either. 