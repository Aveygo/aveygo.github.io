---
title: Creating a laptop development environment with the power of a desktop PC
date: 2025-04-11
categories: [tutorial]
tags: [medium]
description: We will be setting up a remote VSCode environment that can be accessed anywhere with Tailscale.
---

# TLDR

I rock a 9 year old Acer Aspire S13 laptop, which means I get:
1. 4-thread 2.5 GHz Intel Core i7-6500U 
2. Integrated graphics
3. 6 hour battery life
4. Not much else

However, I still need that machine learning *ompf* that I get from my desktop pc during my university trips, and so my solution was to use Tailscale + VSCode remote explorer to hook them up.

# The Setup

To replicate my setup, all you'll need is:

1. The <font color="C97064"><b>Client</b></font> computer (the one that you'll be coding on, eg: a laptop)
2. The <font color="32965D"><b>Host</b></font> computer (the one that you wish you were coding on, eg: a desktop)

> The <font color="32965D"><b>Host</b></font> must have a ssh server running. If you can run ```ssh your-host-username@localhost``` on the <font color="32965D"><b>Host</b></font>, then you should be golden! See my [setting up ssh]({% link _posts/2025-04-12-setup_ssh_server.md %}) guide if you need more help.
{: .prompt-warning }

# Tailscale

Here we are just going to be installing and setting up Tailscale on both devices.
Repeat the following for **both** machines:
1. [Sign into tailscale](https://login.tailscale.com/admin).
2. [Install tailscale](https://tailscale.com/download)
3. Run ```sudo tailscale up --hostname=``` followed by either ```client``` or ```host``` depending on which one you are running it on. You should see something like:

> To authenticate, visit:
> https://login.tailscale.com/a/whatevercode

3. Click the url to add the device to tailscale under your account

And thats it! That should be the hardest part of the setup. What we've done is allowed our computers to find each other even if they are not on the same network. 

To test this connection, you can try ssh-ing into the <font color="32965D"><b>Host</b></font> from the <font color="C97064"><b>Client</b></font> with the command: ```ssh your-host-username@host```. If this fails, then you might need to go to the tailscale [dashboard](https://login.tailscale.com/admin) and check for any errors or change the device names.

You should up up with something that looks like:

![alt text](assets/media/remote_explorer/tailscale.png){: width="500"}


# VSCode

This will be the 'developer environment' that will be shared between the two computers. Make sure to [install VSCode](https://code.visualstudio.com/download) on both the <font color="C97064"><b>Client</b></font> **and** <font color="32965D"><b>Host</b></font>.

Then for both the <font color="C97064"><b>Client</b></font> and the <font color="32965D"><b>Host</b></font> (with vscode open):

1. ```ctrl + p```, and run the command: ```ext install ms-vscode-remote.remote-ssh```
2. ```ctrl + p``` again, but now run: ```ext install ms-vscode-remote.vscode-remote-extensionpack```

This will install the nessesary extensions that will allow us to use the <font color="32965D"><b>Host</b></font> as a development environment on our <font color="C97064"><b>Client</b></font>.

# Remote Explorer

Now we can actually start a connection between the <font color="32965D"><b>Host</b></font> and <font color="C97064"><b>Client</b></font>.

First (on the <font color="C97064"><b>Client</b></font> computer), you need to open the remote explorer tab, **select the ssh remote option** from the drop down menu, then ```New Remote```.

![alt text](assets/media/remote_explorer/open_remote.png){: width="300"}

Then, a new popup should appear, in which case you should type: ```ssh your-host-username@host``` to configure our extensions.

![alt text](assets/media/remote_explorer/enter_ssh.png){: width="500"}

Once you press enter, a new popup will appear. Simply select the default host file (just press enter).

![alt text](assets/media/remote_explorer/select_config.png){: width="500"}

Then start the connection to the <font color="32965D"><b>Host</b></font>.

![alt text](assets/media/remote_explorer/connect.png){: width="500"}

After entering your password, you can finally use your <font color="32965D"><b>Host's</b></font> instance of vscode as if it's running locally! Click ```Open Folder``` and navigate to your project directory.

# Limitations

The biggest limitation is that if you want to have more clients, you'll need to setup tailscale for each device. Otherwise, there's practically zero drawbacks!


# Tips & Tricks

 - Setup a ssh key so you don't have to use your password. Assuming you're using Linux, on the <font color="C97064"><b>Client</b></font>, type ```ssh-keygen``` (just press enter for all the options), then ```ssh-copy-id your-host-username@host``` and passwords-be-gone!

 - You can have multiple clients connected to one host, but I recommend renaming all your devices from client/host into something more useful (you'll need to go though the Remote Explorer setup again).

 - Tailscale is super versatile! I personally run it on my homelab so I can access my services at uni without needing to deal with port-forwarding (it also can act like a VPN).

 - A more mainstream approach might be with VSCode [tunnels](https://code.visualstudio.com/docs/remote/tunnels). The end result is almost exactly what we have here, but it unfortunately has a lot more limitations when compared to Tailscale.

# Conclusion

Hopefully this blog helps out with your portable programming adventures! 
