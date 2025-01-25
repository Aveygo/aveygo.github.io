---
title: Fixing rust cross "Permission denied (os error 13)" 
date: 2025-01-25
categories: []
tags: []
---

Today I spent 1 hour trying to solve this error:
```cargo
error: failed to open: /target/debug/.cargo-lock

Caused by:
  Permission denied (os error 13)
``` 
after trying to run ```cross build --target=x86_64-unknown-linux-musl```


Putting my detective hat on, we can guess that it has to do something with file permissions, so lets just 777 that bad boy and get it over and done with? No!

This problem happens because the user within the docker container that cross runs has the permissions uid=0, gid=0, and our folder is 1000:1000!

After digging into the cross [changelog](https://github.com/cross-rs/cross/blob/main/CHANGELOG.md), we can find a [pull request](https://github.com/cross-rs/cross/pull/543) that can help us out:

```bash
CROSS_CONTAINER_UID=0 CROSS_CONTAINER_GID=0 cross build --target=x86_64-unknown-linux-musl
```

or more simply:

```
sudo chown -R 0:0 target/
```

Sometimes it's good to know how file permissions work in linux. 