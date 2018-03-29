# PKGBUILDs

A repository of some custom PKGBUILDs for Arch Linux.

## Building

I prefer to build packages inside a chroot. This is not necessary for most
PKGBUILDs that just produce a binary but I would recommend doing so for any
package that builds a kernel module. Especially when building a new package
or trying out modifications this can be helpful to avoid polluting the host
with packages.

* Install devtools: `pacman -S devtools`
* Create a chroot, we'll name it `root`:
  ```
  mkdir $HOME/chroot
  mkarchchroot $HOME/chroot/root base-devel
  ```
* Update the chroot before any build:  `arch-nspawn $HOME/chroot/root pacman -Syu`
* Build a package: `cd <dir with PKGBUILD>; makechrootpkg -c -r $HOME/chroot`

## dell-smm-hwmon\*

In order to be able to use i8kmon to control the fans on some Dell laptops, a
patched version of `hwmon/dell-smm-hwmon` needs to be installed. Together with
`dell-smm-hwmon-cli` this will allow you to disable BIOS fan control and let
i8kmon do its job.

There are two packages that build a kernel module, `dell-smm-hwmon` and
`dell-smm-hwmon-lts`, the former targetting the `linux` kernel package and the
latter suited for the `linux-lts` kernel package. The `dell-smm-hwmon-cli`
package provides the `dell_smm` binary.

Because these modules are tied to the kernel version, they need to be rebuilt
for every kernel. You'll need to update the `pkgver` and `pkgrel` variables in
the appropriate `PKGBUILD` for the kernel you're targetting. Based on a
`uname -r` output of `4.14.30-1-lts` `pkgver` must be `4.14.30` and
`pkgrel` `1`. Adjust according to the kernel you're targetting. Note that if
you're targetting a different kernel than you've currently booted (for example
in preparation of a kernel upgrade) you will need to build the package inside
a chroot or ensure you have the new kernel and headers installed prior to
building the package.

Please note that loading a patched kernel module (for an in-tree module) will
result in the kernel being marked as tainted.

## Licensing

Though the `PKGBUILD` files are licensed according to the `LICENSE` file, some
files included or the package being built might use a different license. Please
refer to the `license` entry in the `PKGBUILD`.
