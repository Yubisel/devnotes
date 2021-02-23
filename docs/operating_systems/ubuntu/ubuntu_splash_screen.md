# Disable splash screen

How to disable the boot splash screen, and only show kernel and boot text instead?

Yes. Edit ```/etc/default/grub``` (using gksu ```gedit /etc/default/grub```), and remove the ```"quiet splash"``` from the Linux command line:

Here's what it looks like by default:

```bash
GRUB_CMDLINE_LINUX_DEFAULT="quiet splash"
```

Make it look like this:

```bash
GRUB_CMDLINE_LINUX_DEFAULT=""
```

After this run ```sudo update-grub2```

Also from the GRUB menu, if you want to do this temporarily, you can hit ```E``` on a line to edit it, then ```Ctrl+X``` to boot the kernel line.

Make sure you don't have ```plymouth-theme-ubuntu-text``` package installed.
