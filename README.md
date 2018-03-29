Install Ubuntu 14.10 on Chromebook Pixel (2013)
===============================================

With this instructions you get a Ubuntu 15.04 on Chromebook Pixel, installed
on the SSD without removing Chrome OS. This means dual boot from development
mode prompt to either Chrome OS or Ubuntu. All relevant components work on
Ubuntu as you would expect it, including Bluetooth, touch and Wifi.

This guide is for the Chromebook Pixel from 2013. Most of the things here
will work for the Pixel 2015 as well. You will have issues with the Kernel as
it is lacking some drivers required for the newer Pixel. If anyone wants to
send me a Pixel 2015 get in touch by email (simon AT longsleep DOT org) and I
will see what I can do.

## Assumptions

 - Chromebook Pixel is already in developer mode.
 - You are willing to shrink Chrome OS (this will remove all data).

So just get this repository and extract it into ~/Downloads. You need to be able
to run the various scripts shipped in this repository.

This repository provides all the gear to install Ubuntu along side with Chrome
OS. During this process Chrome OS will be shrunk and will reset all its data.
You have been warned!

## Shrink Chrome OS

    sudo bash shrink-chromeos

## Install Ubuntu

  The Chrome Book Pixel hardware works out of the box with Ubuntu 15.04. No
  extra equipment is required.

    sudo bash install-minimal

## Kernel settings

  - Add line `i915.modeset=1` into /etc/modules.
  - Add `tpm_tis.force=1 tpm_tis.interrupts=0` to GRUB_CMDLINE_LINUX_DEFAULT in
    /etc/default/grub and run `sudo update-grub` afterwards.

## Disable bluetooth on startup

  Edit /etc/rc.local and add `rfkill block bluetooth` before the `exit 0`.

## Dim screen on startup

  The Pixel's screen is very bright and boots up at full intensity. So I find
  it a good idea to dim it on startup. Edit /etc/rc.local and add
  `echo 2800 > /sys/class/backlight/intel_backlight/brightness`.

## Hot keys

  - I use the CTRL key to rebind audio hotkeys in keyboard settings.
  - For keyboard backlight I use a custom trigger CTRL+TAB with my
    `pixel-keyboard-brightness` from my [bin-scripts](https://github.com/longsleep/bin-scripts) repository.
  - Display backlight keys can be bound to a custom action using `xdotool`.
    Install `xdotool` with apt-get and bind `xdotool key --clearmodifies XF86MonBrightnessUp`
    and `xdotool key --clearmodifies XF86MonBrightnessDown` to the designated
    keys within keyboard settings. I have a script for this in my bin-scripts
    repository as well.

## Known issues

  - Chromebooks forget about Dev mode when completely out of battery. This is
    very annoying. See [here](http://dev.chromium.org/chromium-os/developer-information-for-chrome-os-devices/workaround-for-battery-discharge-in-dev-mode) for
    details.

## Dual boot with Chrome OS

  So now we have Ubuntu. Chrome OS is still there and functional. You can boot
  it from the boot screen with CTRL+D.

## Install Chromebrew into Chrome OS

  [Chromebrew](http://skycocker.github.io/chromebrew/) is a package manager for
  Chrome OS which allows to easily install various tools into your Chrome OS
  environment. It installs directly into Chrome OS, no chroot what so ever.

    wget -q -O - https://raw.github.com/skycocker/chromebrew/master/install.sh | bash

  This gives you a decent start for working with Chrome OS on the command line,
  including Git support.

  Then try it out and install vim.

    crew install vim

  Yay!

## Install Chrome Dev editor (works on both Ubuntu and Chrome OS)

  [Chrome Dev Editor](https://chrome.google.com/webstore/detail/chrome-dev-editor-develop/pnoffddplpippgcfjdhbmhkofpnaalpg)
  is a Chrome App which works offline and provides you a Graphical Text editor.
  It supports Git directly (but do no use that) as it works great together with
  commandline Git. I use the Monokai color theme there as this is the theme I
  usually have in Sublime Text too.

## Use Kernel 4.1

  I recommend running the Pixel with Linux Kernel 4.1 as it brings lots of
  changes and improvements for the Intel Platform. I have a Kernel patches
  tree and configuration for the Chromebook Pixel in my [Kernel patches](https://github.com/longsleep/linux_patches/tree/pixel) repository.

## Extra packages

  I also have a [ppa for the Pixel on Ubuntu](https://launchpad.net/~longsleep/+archive/ubuntu/pixel-extras). This adds some extra functionality and tools i use of the
  time. Most of it requires Kernel 4.1 though.

## My result/ recommendation

  For me the Chromebook Pixel is working great with Linux. I am using it as
  main laptop for development when i am at home and travelling. Below you find
  a list of the best and worst items which are relevant to me.

### Pro's

  + Great QWERTY keyboard which has all be buttons at the right place and in
    the correct size to type quickly and fluidly.
  + Great, large touchpad.
  + Solid aluminium case, which maches the whole thing pretty much
    indestructable.
  + Linux support is good (using my custom Kernel).
  + Brilliant HiDPI 4:3 screen.
  + Fan is off when not doing very much / no noise!
  + Mini display port connector to attach external HiDPI display.
  + Good 720p UVC camera.
  + Reasonable CPU performance (14367.41 BogoMIPS).

### Con's

  - Gets rather hot (+loud fan) when CPU and GPU is loaded.
  - Moderate battery live (5 to 6 hours at best).
  - No secure boot / have to keep Chrome OS in dev mode.
  - Very small SSD, need to use extra SDCard to be useful.
  - Only has USB2 / no USB3.
  - Keyboard is missing some keys, requires extra Kernal patches to get
    POS1, END, PAGE-UP/DOWN.
  - Need to keep Chrome OS on the SSD to be able to recover from complete
    power loss. See [here](http://dev.chromium.org/chromium-os/developer-information-for-chrome-os-devices/workaround-for-battery-discharge-in-dev-mode) for
    reasons.

### Additional things

  - Check if we can improve battery life with Kernel 4.2 and
    by setting `pcie_aspm=force i915.enable_fbc=1 i915.enable_rc6=7`
    Kernel parameters. Also check `i915.enable_psr=1` parameter.


To sum this up, the Chromebook Pixel is the platform of choice and can only
be recommended if you are willing to work around the issues to get Linux on
it and all. Usually in other similar 13" Ultrabooks the list of problems is
a lot longer. The Pixel is almost flawless, now if it had a 512GB SSD and a
customizeable EFI BIOS .. well dreams.

--
Simon Eisenmann
