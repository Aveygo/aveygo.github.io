---
title: Why it takes 9GB to detect music genres
date: 2025-09-10
categories: [Automated Decisions]
tags: [AI, Python]
description: Why dependency management is important, and why it sometimes doesn't even matter.
---

![alt](assets/media/dependencies/small.webp){:style="display:block; margin-left:auto; margin-right:auto; width: 300px;"}

# Background

I have a music collection of over 25,000 songs. However, because most of them were downloaded from youtube, they don't have genre information within their metadata. You would think that surely there would be at least one existing project or library that can do this job, but after many hours of searching online it became apparent that it was going to be easier to just make it myself in good old Python.

However, after a weekend of tinkering and optimizing I ended up with a 120 line script that requires a staggering 9GB of storage to run. This then raises a very simple question: how did we get here?

# Dependencies, Dependencies, & Dependencies

We've all experienced it; you have a dream to make something cool and novel, but when you start pushing characters through the good 'ol IO you realize just how much of your idea relies on other people's code.

At first you keep it minimal; 5 megabytes won't hurt, I need to parse JSON. 100 for numpy because why not. Another 150 for pandas, yes please.

But then, you start considering the heavy hitters; OpenCV at 200mb, PyTorch at 5GB, Tensorflow + Keras at **8GB**.

And of course to keep it all organized you keep your packages in a virtual environment like conda; an additional 1.5GB.

Then before you know it your simple hotdog detector takes the storage equivalent of 100,000 Apollo computers, which would take the volume of roughly 250 Olympic sized swimming pools.

# How did we get here

The simple fact is that programmers are lazy; I would even call it a law: 

> 'The easier it is to download dependencies, the more bloat the average application will have'.
{: .prompt-info }

As a Python dev, I mainly concern myself with pip packages, but my fellow web developers are probably thinking about the monstrosity that is npm, or Rust crustaceans with cargo crates.

While all these ecosystems solve very different problems, the convenience of ```whatever install this``` is too tempting for 99% of us. However, this comes with some significant drawbacks.

# Dependency Hell

We all know how painful it is to get someone else's python project to run. The worst part is that it's most often not the actual code that's at fault, but the dependencies themselves and the myriad of ways that they like to break:

 - Wrong version of python
 - Unsupported platform (macos vs windows vs linux)
 - Version incompatibility
 - Incorrect versions
 - Incorrect feature selection
 - Dependency conflicts
 - Deprecated features
 - Build failures (i'm looking at you setuptools)
 - Unsupported packages that are no longer on pip

Every python dev has more than likely encountered each of these problems at least once. Even if take the nuclear option and wrap your script in a docker container, you can't guarantee that pip would still be hosting the versions you need, or that docker itself will remain compatible 10 years from now.

# Why it takes 9gb to detect music genres

The 'meat' of [pyTuneTag](https://github.com/Aveygo/pyTuneTag) (my finished script) is actually very slim and only requires 120 lines of Python code to load and process data for [mtrpp](https://github.com/seungheondoh/music-text-representation-pp). However, to actually have the code do something useful, we need the following things:

1. Python: Because the project is written in Python.
2. PyTorch: Loading & inferencing the primary model.
3. Tensorflow: Because mtrpp utilizes a LLM that is only officially supported in TF.
4. pydub + mutagen: For mp3 audio/meta manipulation.
5. Numpy: For array manipulation.
6. mtrpp + its dependencies & models: To convert audio/text data into embeddings.

This means that my measly 120 lines 'expands' by **2 million times more** than the original required storage.

If we were smart, we would instead use a more efficient language such as C/C++ (maybe even Go) and write a custom PCA algorithm that would perform nearest-neighbor classification. It would be much smaller and likely more accurate, but even with that option available I would still prefer my Python solution. 

# Why this is fine

Sometimes there’s no other option but to accept the bloat and the storage requirements. Ideally, all our projects would be <10mb, fast, and secure, but then they wouldn’t be as useful. At the end of the day, [Joe Barnard](https://www.youtube.com/watch?v=4jgTCayWlwc) from BPS.Space put it best:

> The hardest part of any personal project, is to finish the f*****g project

In my case, I just needed a way to finish organizing my music collection. I simply don't need to optimize it for embedded devices or even on the CPU. It needs to run once and only once for the project to do it's job. Even if it takes 100GB of LLMs it'll still be finished and most importantly; useable.

# My message to you

I can almost guarantee that whoever is reading this has a project or two that is still unfinished to this day; even if you know it can be finished and it is something that you need finished.

But the hardest part often isn't making it performant or efficient. Sometimes, just getting the darn thing to work is hard enough as it is. Who knows, maybe you'll come back to it and make it even better sometime in the future.
