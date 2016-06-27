---
layout: post
title: Installing Void Linux on a Raspberry Pi
date: 2016-06-26 16:12:50
categories: void.linux linux raspberry.pi
---

I wanted to try out [Void Linux](http://www.voidlinux.eu/) so I decided to throw one of my raspberry pi's at it.

1. Follow the [void linux wiki](https://wiki.voidlinux.eu/Raspberry_Pi) to install rootfs on the sd card. ssh was working out of the box so I was able to plug in the pi and connect via ssh using root with voidlinux as the password.
2. Configure the package repo to point to:

   ```bash
   echo "repository=http://repo3.voidlinux.eu/current/musl/" >> /etc/xbps.d/my-remote-repo.conf
   ```

3. Through normal means:
   * Set the root password
   * Create a new user
   * Create a password for the new user
   * Add the new user to the suders file
   * Log out of root and log back in as the new user
4. Update the system:

   ```bash
   sudo xbps-install -Suv
   ```

5. Hack away
