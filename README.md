# Command to Fix DNS under WSLv1 when running a VPN connection

This is a quick script to fix DNS when running WSLv1 from a VPN connection. I had a need for it when using WSLv1 with a corporate GlobalProtect VPN client. I hope it helps someone else.

## Install Script

```
$ git clone https://github.com/mcgarrah/wsl-vpn-fixdns
$ cd wsl-vpn-fixdns
$ sudo cp fixdns /usr/local/bin
$ sudo chmod +x /usr/local/bin/fixdns
```

## Execute automatically at startup

Add a sudoers entry to run without password prompt
```
$ sudo visudo
...
# FixDNS for GlobalProtect VPN
%gpfixdns ALL=NOPASSWD: /usr/local/bin/fixdns
```

Add the script to the Bash startup
```
$ vi ~/.bashrc
...
# Added for GlobalProtect VPN DNS issue
/usr/local/bin/fixdns
```

## Running Script Manually

```
$ fixdns
[sudo] password: *********
Current resolv.conf
-------------------
# This file was automatically generated by WSL. To stop automatic generation of this file, remove this line.
nameserver 192.168.1.254
nameserver 10.10.1.5
nameserver 10.10.2.5
search attlocal.net

Creating new resolv.conf
------------------------
# This file was automatically generated by WSL. To stop automatic generation of this file, remove this line.
nameserver 10.10.1.5
nameserver 10.10.2.5
nameserver 192.168.1.254
nameserver 192.168.86.1
nameserver 192.168.1.254
search example.net
+ sudo cp -i /tmp/tmp.I3f660fyr0 /etc/resolv.conf
```

## Shell Code

I borrowed the code almost whole cloth from the comments found at https://github.com/microsoft/WSL/issues/1350#issuecomment-427924074 by https://github.com/nfekete for a WSLv1 issue. I did add a check for running as root and force sudo execution and minor fix for duplicate nameservers. There were other solutions on that issue but this one worked for me.

The code depends on PowerShell. It will not likely port to WSLv2.

## Errata

Nothing so far.
