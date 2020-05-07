sd-chkhashboot
================

A mkinitcpio hook for encrypted installation of Arch Linux 
with systemd init. Checks if the machine was tampered with, before 
typing the passphrase of the root partition.

Inspired by: 
* https://github.com/grazzolini/mkinitcpio-chkcryptoboot
* https://gitlab.com/artefact2/sd-chkcryptoboot/

If you don't use full disk encryption, you don't need this.

**An encrypted boot partition with a different passphrase is strongly recommended! 
  If not, it's trivially easy to fake all the checks.**

Features
========

* Checks bootloader integrity.
* Checks ESP integrity.
* Checks USB and PCI devices (using `lsusb` and `lspci`).
* Checks block devices (using `lsblk`).
* Shows a customisable secret message before the password prompt, 
  to make sure you aren't booting someone else's boot partition.

Installation
============

Before tweaking with the boot process, **have a live USB image ready**
in case anything goes wrong and you end up with an unbootable system,
as unlikely as that is.
* Download package:
  ~~~
  git clone https://github.com/nod3man/sd-chkhashboot /tmp/sd-chkhashboot
  ~~~

* Build and install the package:
  ~~~
  cd /tmp/sd-chkhashboot
  makepkg -si
  ~~~

* Edit the configuration file:
  ~~~
  sudoedit /etc/default/sd-chkhashboot.conf
  ~~~

* Edit the prompt message with something unique you remember (like an ASCII map):
  ~~~
  sudoedit /etc/default/chkcryptoboot.motd
  ~~~

* Set the correct dependency in the systemd service:
  ~~~
  sudo systemctl edit chkcryptoboot.service

  # Use $(systemctl show /dev/disk/by-id/...) and see the Id= for the device name
  [Unit]
  After=dev-disk-by\x2did-usb\x2dCorsair_Survivor_3.0_22C2018251270274\x2d0:0.device
  Requires=dev-disk-by\x2did-usb\x2dCorsair_Survivor_3.0_22C2018251270274\x2d0:0.device
  ~~~

* Add `sd-chkhashboot` to your list of HOOKS in `mkinitcpio.conf`
  ~~~
  sudoedit /etc/mkinitcpio.conf

  HOOKS="base systemd ... sd-chkhashboot sd-encrypt ..."
  ~~~

* Rebuild the init image:
  ~~~
  sudo mkinitcpio -P
  ~~~

Threat model
============

**sd-chkhashboot can protect against *some* physical attacks, but
doesn't claim to catch all possible attacks. EFI backdoors, or clever
hardware keylogging is still possible, among others.**

## Ciphering the root partition

Pros:

* Data is safe against offline attacks (ie, laptop gets stolen).

Cons:

* Boot partition is not ciphered, can be tricked into running a
  compromised kernel (with a builtin keylogger, for instance) if an
  attacker can get physical access.

## Ciphering the boot and root partition

Pros:

* Same as above

Cons:

* You still can't guarantee the kernel hasn't been
  compromised. Someone can easily replace the encrypted boot
  partition with a fake password prompt that will just accept any
  passphrase.

## Ciphering the boot and root partition, and using sd-chkhashboot

Pros:

* Same as above

* **You are sure you are booting your boot partition and bootloader,
  not someone else's.** This is because of the (hopefully unique and
  rememberable) message shown before the password prompt of the root
  partition. An attacker cannot know your message, because it is
  stored as ciphertext. If an attacker replaces the boot partition
  with a fake one that accepts any passphrase, you won't see your
  message and know that you aren't booting the right kernel. You can
  now be sure that the checks made by sd-chkhashboot aren't fake
  messages and can also know that your devices, or bootloader, haven't
  been tampered with (at least not in a way that's easy to see).

Cons:

* There are still a ton of ways to compromise a machine without
  touching the kernel or bootloader. Backdoors in EFI drivers, or
  compromising the hardware in a way that doesn't get picked up by
  `lsusb`/`lspci`/`lsblk`. Or a compromised keyboard with the same
  device ID as your real one.

* Someone can look over your shoulder during the boot process and take
  a picture of your secret message, defeating its purpose. Using an
  ASCII map, not text, is recommended for this reason (it's harder for
  someone casually looking to remember it).

* You still need to remember, and type, two strong and different
  passphrases during the boot process.
