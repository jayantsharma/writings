# Setting Up Arch Windows dual-boot on my X205TA

After a motherboard crash and the following factory-reset by the Asus center, I was left with a single *msftdata* 18 GB partition on my drive. Pathetic. The windows partition refuses to compress and yielded close to 5GB of space for my planned ext4 partition. Since I'm a self-assumed pro-geek, I vowed to take complete control of my machine by wiping out the Recovery partition on my netbook. 17 GBs for my arch install, hurray !

Installing Arch on X205TA is relatively straight-forward, thanks to the excellent Arch wiki (what else?). Here's the [link](https://wiki.archlinux.org/index.php/Asus_x205ta), in case you're looking for it. The problems began when I, feeling extra-ambitious, decided to restore my earlier system from a combination of backups of my local database and package list. Here's what I did : 
```
$ pacman -xjvf pacman_database.tar.bz2		# at /
$ cd /home/jayant
$ pacman -S $(< pkglist.txt)
```
For experienced users, I guess it's quite predictable to be faced with numerous conflicting package errors after this. This took the better part of an hour to resolve. I think the ideal step would have been to `pacman -U $(ls)` in my local package database and then run `pacman -Syu`. 

However, after sorting out all this muck, X wouldn't work. Some or the other component kept throwing *missing shared libraries* errors indefinitely. There's only so much `ldd` and `pacman` one can use to fix this. After getting X up, X froze on my screen. `pkill X` and logging into another VT failed. I recalled my last install on this machine where `pkill` had worked and the freezing had been sorted out by disabling GPU usage in my Intel graphics config. I concluded some modules related to recognizing input devices must be acting up. 

After being on the edge of madness I had had enough. I went for a fresh install and all was sorted out wo a hitch. Deleted the pkg database backup from my repo, so as to never get stuck on it until I learn to use it well. 

All that's left was to handle the freqeuent crashes that have racked my netbook since my first arch install. Turns out this is kernel bug that troubles all Baytrail netbooks : [kernel_bugzilla](https://bugzilla.kernel.org/show_bug.cgi?id=109051). The kernel parameter *intel_idle.max_cstate* is reported to be a good workaround for this bug, at the cost of battery-life. Read the documentation and got this in place. Thanks to arch, getting my hands dirty is not something I'm afraid of any longer.

I haven't experienced a single crash on my X205TA on kernel 4.3.3-3, with the above parameter in place. Curiously, enough the linux-lts kernel in arch repos doesn't last more than a couple of minutes.

So grub was set up nice and proper to load the windows bootloader, but here's what it spewed up : 
> Your PC needs to be repaired 
> Error code: 0xc0000225 

Turns out X205TA uses a WIMBoot install wherein, to save space, the boot files have just one copy stored in the guess what, Recovery Partition. So Windows is pretty much dead. Except that I can try to re-install from a bootable USB. And here's where it gets really funny. Keyboard and mouse input doesn't work on the install screen. Disabling XHCI in BIOS does nothing. USB input has been reported to work, but I don't own one. BIOS can be flashed, but a lot of X205TA users have reported brick-ing their laptops after attempting an upgrade. After two motherboard replacements in 10 months, I don't think Asus will be willing to repair my machine if I brick it this way. In any case, can't lose time on it too as this is my sole working machine (I know!).

So goodbye Windows. Wish I could say good riddance. But this implies, no sound, no media (sound doesn't work in X205TA arch, existing [bug](https://bugzilla.kernel.org/show_bug.cgi?id=95681)). And a crappy mobile to look up documentation, in case an upgrade crashes my system. Getting ready to dive into system backups now.

