---
layout: post
title: Successfully dual-booting Windows 8 and Linux Mint 15 on UEFI
---

Today's CPLUG [Free Your Machine][] brought new challenges in the form of
UEFI laptops with Windows 8. (On the plus side, though, GPT means no more
worries about already having 4 OEM-created partitions).

First, all Windows 8 systems needed [fast boot][] disabled. Then we needed to
disable Secure Boot. Some firmware setup utilities were more cooperative than
others, and in several cases we had to just recommend installing Linux in a VM
(since [Wubi][] doesn't support UEFI).

Finally, on two laptops (an HP Envy and a Toshiba Satellite), Linux Mint and
GRUB would install correctly but the machine would still boot directly to
Windows, and there was no firmware setup option to specify the path to the
`grubx64.efi` file. I ended up getting these to boot to GRUB, then to either
Linux Mint or Windows 8, as follows.

*Disclaimer: I'm reconstructing these commands from memory a few minutes after
the fact, so no guarantees they're exactly correct, much less that they will
work on any other system.*

Boot to a Linux LiveCD/USB again and mount the EFI partition, then move the
Windows `bootmgfw.efi` aside and replace it with a copy of GRUB's
`grubx64.efi`:

    # mkdir efi
    # mount /dev/sda2 efi
    # cd efi/EFI/Microsoft/Boot/
    # mv bootmgfw{,-real}.efi
    # cp -a ../../linuxmint/grubx64.efi bootmgfw.efi

Now on boot, GRUB should run and start Linux Mint. From the installed system, 
add an entry to GRUB that chainloads Windows via the moved-aside file by
adding the following to `/etc/grub.d/40_custom` (adapted from
[here][askubuntu]):

    menuentry "Windows 8" --class windows --class os {
        insmod part_gpt
        insmod chain
        set root=(hd0,gpt2)
        chainload /EFI/Microsoft/Boot/bootmgfw-real.efi
    }

then run `update-grub` to regenerate the configuration. With any luck, GRUB
will now offer a choice between Linux Mint and Windows 8, and both will work!

[Free Your Machine]: http://cplug.org/2013/09/29/fym-fall-2013/
[fast boot]: http://www.typicaltips.com/2013/02/disable-fast-startup-in-windows-8.html
[askubuntu]: http://askubuntu.com/a/216254
[Wubi]: http://www.ubuntu.com/download/desktop/windows-installer
