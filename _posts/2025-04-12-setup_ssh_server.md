---
title: Setting up a SSH server
date: 2025-04-11
categories: [tutorial]
tags: [easy]
description: Yet another guide on installing OpenSSH (Windows & Linux)
---

# What is this post about?

This is just a quick post to supplement [another]({% link _posts/2025-04-11-remote_desktop.md %}). Sorry, but there's not really anything groundbreaking or new here.

# Windows

I'm mainly just rehashing the official [guide](https://learn.microsoft.com/en-us/windows-server/administration/openssh/openssh_install_firstuse?tabs=gui&pivots=windows-server-2025), which contains the instructions:

1. Open powershell
2. Install the ssh client
``` powershell
Add-WindowsCapability -Online -Name OpenSSH.Client~~~~0.0.1.0
```
3. Install the ssh server
``` powershell
Add-WindowsCapability -Online -Name OpenSSH.Server~~~~0.0.1.0
```
4. Start the server
``` powershell
Start-Service sshd
```
5. Then enable it
``` powershell
Set-Service -Name sshd -StartupType 'Automatic'
```
1. And optionally, open the firewall for ssh. This is not needed for tailscale if you are here from my [remote desktop guide]({% link _posts/2025-04-11-remote_desktop.md %})
``` powershell
New-NetFirewallRule -Name sshd -DisplayName 'OpenSSH Server (sshd)' `
  -Enabled True -Direction Inbound -Protocol TCP -Action Allow -LocalPort 22
```

# Linux

This will mainly depend on your distribution, but 9/10 times, OpenSSH can be installed via your package manager.

In the case of Ubuntu, this is simply:
``` bash
sudo apt install openssh-server
```

Then, you only need to enable & start the service:
``` bash
sudo systemctl enable ssh --now
``` 

And you're done!
