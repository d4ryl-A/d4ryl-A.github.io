---
title: 'EndeavourOS setup on Proxmox'
categories: [Proxmox]
tags: [arch,proxmox,linux]
render_with_liquid: false
---

## Before we begin

EndeavourOS is an Arch-based distro that provides an Arch experience without the hassle of installing it manually for both x86_64 and ARM systems. After installation, you’re provided with a lightweight and almost bare-bones environment ready to be explored with your terminal, along with our home-built Welcome App as a powerful guide to help you along.

### ISO Download

By going to https://endeavouros.com/ and look at the list of Download Mirror list that matches the country that you are in for better download speed.

Mine will be North-America, for some reason the Canada link does not work but US will also work for me.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/2a142040-e62b-4a9c-abb1-4a5440c17ab2)

Once the download is done, we will then open our Proxmox server.

### VM setup

Once we are logged in to our proxmox.

First we need to upload the ISO file that we downloaded

We need to go to **local (Node name)**
**ISO Images**
Then **Upload**
A box will popup and it will ask to select a file then locate the downloaded iso and click **Upload**

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/1dd58eaf-40f1-4f2b-9c32-4f59ea7a57fa)

Once the upload is done, we will then create a VM.

Click on ![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/a8edad96-525e-4d53-a9d3-3b9d7ebb806c)

and fill out the box that popped out.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/9227fdc9-7389-431e-9cc2-bbb9d36808ef)

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/a8719c8d-c4bf-4b8f-bf49-8a14c594d872)

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/73dbbe34-f42a-4524-a828-35682e2f835e)

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/a198392e-46d1-4342-a209-23ac94f4601e)

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/d6f7382d-810c-4b40-9d32-e08cc6877227)

Once you are done click **Finish**

Note: you can copy what I set on storage, CPU  numbers, and Memory as long as your computer can support it or you can set it more resource or less resource.

### OS Installation

Once our VM is setup, we will then right click the VM that we create and **Start**

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/65a5910c-b458-4a08-9951-5b5d68972353)

Double click on the VM that we created to open it to another browser.

Once it boots up to desktop and a Welcome screen will popup and we want to choose **Start the Installer**

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/b86ac963-c108-4bcd-825d-041274fd550f)

Then it will ask for Online or Offline installation, because we have internet running on this VM, we will do the Online installation method.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/dd70d656-e5fb-41e3-8a50-f8c7c862f8b7)

It will ask you multiple setup question, just choose the correct language, location, keyboard layout, etc. that you want.

Once it is all done, we want to do a restart.

![image](https://github.com/d4ryl-A/d4ryl-A.github.io/assets/129752764/cfd3bc07-1562-4759-b8f2-080409455dad)

And that is it!

Once it boots up to desktop, there is another popup for "After Install" which is basically updates that you can also do or change wallpaper, resolution, and etc.

This will be a journey for me as I will modify this EndeavorOS heavily when it comes to design and other aspect.

I will create a guide blog with the changes that I will be doing in the future.
