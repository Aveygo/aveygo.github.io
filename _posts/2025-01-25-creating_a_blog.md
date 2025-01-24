---
title: Creating a blog
date: 2025-01-24
categories: [webdev]
tags: [simple]
---

I used [Chirpy](https://chirpy.cotes.page/) to create this blog and ran into some very small issues.

# Favicons

The documentation gives a website to generate favicons, but I think there was an update and the generated favicons take on filenames that chirpy does not recognise. I was successful when I used [this one](https://www.favicon-generator.org/) instead

# Deployment

On my first attempt after trying to deploy, I got the response:
```html
--- layout: home # Index page --- 
```

This one is my fault, just read the documentation and go to repo settings -> Actions -> Source -> and select "Github Actions".
