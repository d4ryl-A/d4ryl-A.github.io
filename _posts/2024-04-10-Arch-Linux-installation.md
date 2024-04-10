---
title: 'Arch Linux installation'
categories: [Proxmox]
tags: [arch,proxmox,linux]
render_with_liquid: false
---

## Before we begin

By going to https://archlinux.org/download/, we can download the ISO file and if you are doing a baremetal, you can use rufus to create a bootable USB.

On this demonstration I will be using Proxmox.

## Installation

Once we boot to the ISO, we will be greeted by a CLI (Command Line Interface)

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/cb1b4fce-e6bd-4962-9e3f-641a216048fa)

Wired ethernet connection is the advised way but if you want to connect via Wi-Fi, you need to do some extra steps.

### Guide to connect to Wi-Fi

If you look at the screen, there is a portion in there for the command to authenticate to wireless network.

Command:

`iwctl`

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/04fcbf49-08ed-4a59-9537-b956a1c812dc)

To verify if our Wi-Fi scan is running or not.

Command:

`station wlan0 show`

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/28cb1b7e-5bd6-495e-8782-127ae4ccfdb6)

If it shows **no** on scanning, to turn it on we need to issue another command.

`station wlan0 scan`

And that should change the Scanning to **yes**.

Now to connect to your Wi-Fi, issue this command

`station wlan0 connect 'your Wi-Fi SSID/name'`

Then it will ask for the password.

Then hit enter, on the show command, it should give the Wi-Fi connection info and to verify, you can do `ping 8.8.8.8`.

Lastly, type `exit` to go back to the root shell.

### Arch installation

Once you are connected to Wi-Fi, to install Arch you need to issue this command.

`archinstall`

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/1d08611f-6d50-4db4-82fe-eb124e72642c)

Then just set or modify the options that suites you.

This is what I setup on mine

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/1df56315-81b3-43ff-8c59-93073b020474)

Once it is all setup, then choose `install`

Instalaltion will take time so just sit back and relax.

Once it is done, I usually do a reboot. 

(Note: If you click Yes on chroot, you can just type `exit`, then you can `reboot`)

And there we go, we have now a running Arch Linux.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/91297980-dac8-4edb-9f36-4a389479162d)

### Connecting to Wi-Fi after the installation

To connect to Wi-Fi once Arch is running, you can issue this command.

`nmcli device wifi connect 'Your SSID' password 'Your Wi-Fi password'`

Then hit enter, you should see a confirmation, then you can do `ping 8.8.8.8` again to verify.
