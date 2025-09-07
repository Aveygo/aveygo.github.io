---
title:  Rust Compiling for the Kindle
date: 2025-01-24
categories: [kindle, rust]
tags: [simple]
description: How to compile a rust binary that can run on the kindle natively 
---

# Background
I wanted to create a weather dashboard for my kindle. There were some [existing solutions](https://github.com/matopeto/kindle-weather-dashboard), but they tend to be super simple or extremely limited. 

Instead I wanted something that was:

1. Stable so I could set and forget
2. Information heavy with all my favorite stats
3. Customizable for future use

Naturally, wanting to explore a new language, I chose Rust to acheive these goals. However, I could not find any information on how to compile for it on the kindle! 


# Solution

After some tinkering and exploring existing binaries, I found which target the Kindle would support and combined it with static linking:

```bash
RUSTFLAGS="-C target-feature=+crt-static -C opt-level=s -C strip=symbols" cross build --target arm-unknown-linux-musleabi --release
```

### Command Breakdown
- `RUSTFLAGS=`: Sets compiler flags via environment variable.
  - `-C target-feature=+crt-static`: Enables static linking to include all dependencies in the binary (no external libraries needed).
  - `-C opt-level=s`: Optimizes code for smaller file size.
  - `-C strip=symbols`: Removes debug symbols to make the binary even smaller.
- `cross build`: Runs the build using the `cross` tool for easy cross-compilation.
- `--target arm-unknown-linux-musleabi`: Specifies the build target (ARM architecture, Linux OS, MUSL libc – matches Kindle's setup).
- `--release`: Builds in release mode for optimized performance and size.

I used [cross](https://github.com/cross-rs/cross) to avoid having to deal with a lot of additional pain that arrises from targeting `arm-unknown-linux-musleabi`. If you know what you're doing you can simply replace it with cargo.

# Final notes
You can find my finished project, [here](https://github.com/Aveygo/KindleDashboard).

I think it'll be interesting to see if there is a way to create a KUAL extension with this rust compiling in mind. ~~Maybe that is a project for another day.~~ *Update! Check out my new blog post [here]({{ site.baseurl }}/posts/kual_hello_world/)
