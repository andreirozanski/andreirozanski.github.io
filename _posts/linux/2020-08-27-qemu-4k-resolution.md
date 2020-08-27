---
layout: post
title:  "QEMU - 4k resolution"
tag: Linux
---

For all that I have done here, I have followed the directions provided by "Staf Wagemakers" in his post: "High screen resolution on a KVM virtual machine with QXL"

To get 4k working in the guest one need to fix the amount of video RAM in the host:

```
$ virsh

Welcome to virsh, the virtualization interactive terminal.

Type:  'help' for help with commands
       'quit' to quit



virsh # list --all
 Id   Name       State
--------------------------
 6    debian10   shut-off

# Now, edit debian10 domain to set proper video RAM

virsh # edit --domain debian10


``` 

This will open your default text editor and there I have changed **vgamem** value to 65536:

```
     <video>
       <model type='qxl' ram='65536' vram='65536' vgamem='65536' heads='1' primary='yes'/>
       <address type='pci' domain='0x0000' bus='0x00' slot='0x01' function='0x0'/>
     </video>

```

Then, after booting the VM, I have checked the mode line:

```
cvt 3840 2160

# 3840x2160 59.98 Hz (CVT 8.29M9) hsync: 134.18 kHz; pclk: 712.75 MHz
Modeline "3840x2160_60.00"  712.75  3840 4160 4576 5312  2160 2163 2168 2237 -hsync +vsync

```

Then, to create a permanent configuration:

```
sudo vim /usr/share/X11/xorg.conf.d/10-monitor.conf 

# and add

section "Monitor"
    Identifier "Virtual-0 "
    Modeline "3840x2160_60.00"  712.75  3840 4160 4576 5312  2160 2163 2168 2237 -hsync +vsync
    Option "PreferredMode" "3840x2160_60.00"
EndSection

```

Then, restart the VM and should be good to go.

---
## References

[High screen resolution on a KVM virtual machine with QXL](https://stafwag.github.io/blog/blog/2018/04/22/high-screen-resolution-on-a-kvm-virtual-machine-with-qxl/)
