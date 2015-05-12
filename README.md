README for build-latest-mpd-for-debian
======================================

The `make-debian-mpd-chroot` bash script creates a debian chroot,
which is started as a virtual machine and automagically build the
latest mpd packaged for Debian. The resulting package is copied back
from the virtual machine to the directory where the script was started
on the real host.

It is intended to be run on a non-debian system, like arch. Users of
debian or ubuntu may be better of using git-buildpackage for this
purpose.

It was designed to have a minimal amount of external
dependencies. Therefore it only depends on fairly recent versions of
`debootstrap` and
[`systemd-nspawn`](http://www.freedesktop.org/software/systemd/man/systemd-nspawn.html),
of which the latter is iinstalled by default in any modern linux
distribution. debootstrap is available for arch as a package in the
[Arch User
Repository](https://aur.archlinux.org/packages/debootstrap/).

