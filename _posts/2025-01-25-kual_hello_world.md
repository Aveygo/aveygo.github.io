---
title: KUAL Hello World in Rust
date: 2025-01-24
categories: [tutorial, rust, kindle]
tags: [simple]
description: An introduction into a basic KUAL App with rust binaries
---

# Background

This is a tutorial to create a simple script that will show a "Hello World" message. This will be an extension within KUAL, so it'll need some additional configuration.

# Configuration

Here we are creating some boilerplate for KUAL to recognise our app

config.xml
```xml
<extension>
    <information>
        <name>KindleApp</name>
		<version>0.1.0</version>
		<author>Me</author>
		<id>KindleApp</id>
    </information>
    <menus>
        <menu type="json" dynamic="true">menu.json</menu>
    </menus>
</extension>
```

menu.json
```json
{
    "items": [
        {
            "name": "Hello World",
            "priority": -999,
            "action": "kindleapp/target/arm-unknown-linux-musleabi/release/kindleapp",
            "exitmenu": true
        }
    ]
}
```

# Rust Binary

Now lets create our rust binary that we want to run on the kindle.

```bash
cargo new kindleapp && cd kindleapp
```

src/main.rs
```rust
use std::process::Command;

fn main() {
    Command::new("eips").arg("1").arg("1").arg("\"Hello World\"").output().ok();
}
```

> Make sure you have docker installed, running, and your account is added to the docker group, and then install [Cross](https://github.com/cross-rs/cross)
{: .prompt-warning }

Add the arm target to rust

```bash
rustup target add arm-unknown-linux-musleabi
```

<br />

Finally compile for the kindle

```bash
RUSTFLAGS="-C target-feature=+crt-static -C opt-level=s -C strip=symbols" cross --target arm-unknown-linux-musleabi --release
```

# Final check
```
.
├── kindleapp/
│   ├── src/
│   │   └── main.rs
│   ├── target/
│   ├── .gitignore
│   ├── Cargo.lock
│   └── cargo.toml
├── config.xml
└── menu.json
```

# Moving to the kindle

For the sake of simplicity, we are going to do a very bad thing and move everything to the kindle within a single folder to ```extensions```.

> This is the programming equivalent of taking a jet to the kitchen. When you eventually commit to creating a kindle app, I strongly recommend you to single out the rust binary ```kindleapp/target/arm-unknown-linux-musleabi/release/kindleapp``` outside of the rust folder, something like:
> ```
> .
> ├── kindleapp
> ├── config.xml
> └── menu.json
> ```
> to avoid having to transfer literal gigabytes in dependencies; **make sure to change the path in menu.json to just kindleapp!** For such a small project this doesnt matter as it currently should be ~10mb, but it will later bite you in the butt if you are not careful.
{: .prompt-tip }

At the end, it should look like this on the kindle:
```
/
├── ...
└── extensions/
    └── kindleapp/
        ├── config.xml
        ├── menu.json
        └── kindleapp/
            └── ...
```

# Starting

Eject, then on the kindle go to KUAL -> "Hello World" and you should see a message show in the top left corner if successful.

# Closing Notes

**Make sure to remove the rust dependencies (see note above) when you start writing more complex scripts.**
