---
title:  Rust Compiling for the Kindle
date: 2025-01-24
categories: []
tags: []
description: How to compile a rust binary that can run on the kindle natively 
---

# Background
I wanted to create a weather dashboard for my kindle. There were some [existing solutions](https://github.com/matopeto/kindle-weather-dashboard), but they tend to be super simple or extremely limited. 

Instead I wanted something that was:

1. Stable so I could set and forget
2. Information heavy with all my favorite stats
3. Customizable for future use

Naturally, wanting to explore a new language, I chose Rust to acheive these goals. However, I could not find any information on how to compile for it! 


# Solution
After some tinkering and exploring existing binaries, I found which target the kindle would support and combined it with static linking:

```bash
RUSTFLAGS="-C target-feature=+crt-static" cross build --target arm-unknown-linux-musleabi --release
```

I used [cross](https://github.com/cross-rs/cross) to avoid having to deal with a lot of additional pain.

# Final notes
You can find my finished project, [here](https://github.com/Aveygo/KindleDashboard).

I think it'll be interesting to see if there is a way to create a KUAL extension with this rust compiling in mind. Maybe that is a project for another day.