---
title: Kindle Jailbreaking with Winterbreak
date: 2025-01-24
categories: [kindle]
tags: [simple]
---

# Background

I used [Winterbreak](https://www.mobileread.com/forums/showthread.php?t=365372) to have some custom screensavers for my Paperwhite 4 10th gen.

I took the steps described in [this guide](https://kindlemodding.org/jailbreaking/WinterBreak/) to install the jailbreak + kual + mrpi + disableota, however, I had some issues I needed to solve.

# Issues

## xzdec

When trying to install the screensaver hack, I got:
```bash
./xzdec: not found
```
Within the mrpi logs.

I remember finding a forum post (can't find again!) to install [USBnetlite](https://github.com/notmarek/kindle-usbnetlite/releases) which solved this issue. Just move it to the ```extensions``` folder.

If it is still not working, then you might find some luck with
```bash
ln -s /lib/ld-linux-armhf.so.3 /lib/ld-linux.so.3
```
after ssh-ing into the kindle, which I found [here](https://www.mobileread.com/forums/showthread.php?t=365372&page=21).

## Screensaver hack

Very easy once the xzdev issue is solved, just follow the [guide](https://www.mobileread.com/forums/showthread.php?t=195474), or just install the [ScreenSavers Hack](https://www.mobileread.com/forums/showthread.php?t=225030) in to the ```extensions``` folder. Just **make sure** to read the headers and get the install for your kindle, eg: PW4 and not just the first link that crosses your eyes.

For some reason I got an error on install from mrpi but it ended up working anyways. I don't think python is needed unless you want the additional feature of downloaded book covers of whatever book you were reading.

## SSH

SSH-ing to the kindle was a pain. I placed the restriction to never disable airplane on my kindle to avoid even the potential of an automatic update, so I had to use USB.

First, you must go to KUAL -> usbnetlite -> toggle, to enable the ssh interface. Linux **should** stop recognising the kindle as a storage device at this point. 

I used the command:
```bash
ip addr
```
to find the network interface of the kindle (should be something like enp2s0...), then used [this](https://www.mobileread.com/forums/showthread.php?t=204942) guide:
```bash
sudo ip link set up dev INTERFACE_NAME
sudo ip address add 192.168.15.201 peer 192.168.15.244 dev INTERFACE_NAME
```
replacing INTERFACE_NAME with whatever was found in order to setup our usb network.

Then we can finally do:
```bash
ssh root@192.168.15.244
```

However, for some reason, I could not use [this root password finder](https://www.sven.de/kindle/) to authenticate. I eventually found some random forum post to find out that my password was simply ```kindle```.

## Useful link
I don't know who [notmarek](https://github.com/notmarek) is, but he has a bunch of usefull [extensions](https://fw.notmarek.com/khf/) for the kindle. 

# TLDR
Bit of a pain, but honestly that's par for the course. Thank you HackerDude for the jailbreak. 10/10 very well done, but would love these issues to be more easily categorised somehow.

# Screensavers

## General guide

For the PW4, make sure that they are vertically oriented, ie: width = 1072, height = 1448 (those numbers are engrained in my head btw), and in b/w. I used gimp because that is what I'm comfortable with.

## Personal

Here are some images that I created for my kindle (1072x1448):

![Fighter jet pilot holding a sign that says "will fly for food"](assets/media/kindle_screensaver/wfff.png){: width="400"}

![Space X starship"](assets/media/kindle_screensaver/starship.png){: width="400"}

![AI generated image of Kim Jong Un and Vladimir Putin through a door peephole"](assets/media/kindle_screensaver/kim_putin.png){: width="400"}

