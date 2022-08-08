# Virtual Box Guest Additions

Installed `emulators/virtualbox-ose-additions`:

```sh
pkg install emulators/virtualbox-ose-additions
```

Edited `/etc/rc.conf` to add the following lines:

```sh
vboxguest_enable="YES"
vboxservice_enable="YES"
vboxservice_flags="--disable-timesync"
```

Created `/etc/X11/xorg.conf` to add the following lines:

```sh
Section "Device"
	Identifier "Card0"
	Driver "vboxvideo"
	VendorName "InnoTek Systemberatung GmbH"
	BoardName "VirtualBox Graphics Adapter"
EndSection

Section "InputDevice"
	Identifier "Mouse0"
	Driver "vboxmouse"
EndSection
```

Created `/usr/local/etc/hal/fdi/policy/90-vboxguest.fdi` to write onto it the following stuff:

```sh
<?xml version="1.0" encoding="utf-8"?>
<!--
# Sun VirtualBox
# Hal driver description for the vboxmouse driver
# $Id: chapter.xml,v 1.33 2012-03-17 04:53:52 eadler Exp $

	Copyright (C) 2008-2009 Sun Microsystems, Inc.

	This file is part of VirtualBox Open Source Edition (OSE, as
	available from http://www.virtualbox.org. This file is free software;
	you can redistribute it and/or modify it under the terms of the GNU
	General Public License (GPL) as published by the Free Software
	Foundation, in version 2 as it comes in the "COPYING" file of the
	VirtualBox OSE distribution. VirtualBox OSE is distributed in the
	hope that it will be useful, but WITHOUT ANY WARRANTY of any kind.

	Please contact Sun Microsystems, Inc., 4150 Network Circle, Santa
	Clara, CA 95054 USA or visit http://www.sun.com if you need
	additional information or have any questions.
-->
<deviceinfo version="0.2">
  <device>
    <match key="info.subsystem" string="pci">
      <match key="info.product" string="VirtualBox guest Service">
        <append key="info.capabilities" type="strlist">input</append>
	<append key="info.capabilities" type="strlist">input.mouse</append>
        <merge key="input.x11_driver" type="string">vboxmouse</merge>
	<merge key="input.device" type="string">/dev/vboxguest</merge>
      </match>
    </match>
  </device>
</deviceinfo>
```

**Skipped** the share folder configuration.

At reboot, `xorg` ended with errors.

I moved `/etc/X11/xorg.conf` to `/etc/X11/xorg1.conf1` in order to let not `xorg` loading it.
After reboot `xorg` started correctly.

I checked again the configuration and moved `/etc/X11/xorg.conf1` to its original position. `xorg` failed again, after reboot.

At present time (see commit data) I didn't check the configuration again and I will not fix it soon.

These steps that I followed can be found at the FreeBSD handbook chapter 22, section 4 ((here)[https://docs.freebsd.org/en/books/handbook/virtualization/#virtualization-guest-virtualbox]).

