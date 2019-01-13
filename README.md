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

## Licensing

Though the `PKGBUILD` files are licensed according to the `LICENSE` file, some
files included or the package being built might use a different license. Please
refer to the `license` entry in the `PKGBUILD`.
