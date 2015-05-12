README for build-latest-mpd-for-debian
======================================

The `make-debian-mpd-chroot` bash script creates a debian chroot,
which is started as a virtual machine and automagically builds the
latest mpd packaged for Debian. The resulting package is copied back
from the virtual machine to the directory where the script was started.

It is intended to be run on a non-debian system, like arch. Users of
debian and ubuntu may be better of using
[`git-buildpackage`](https://wiki.debian.org/PackagingWithGit) for
this purpose.


Dependencies
------------

This script was designed to have a minimal amount of external
dependencies on the host that will be used as the build
environment. Besides
[`systemd-nspawn`](http://www.freedesktop.org/software/systemd/man/systemd-nspawn.html),
which is installed by default in any modern linux distribution, it
only depends on `debootstrap`.

Arch users may install that from the [Arch User
Repository](https://aur.archlinux.org/packages/debootstrap/), while
Fedora users can use their [`debootstrap`
package](https://admin.fedoraproject.org/pkgdb/package/debootstrap/).


Usage
-----

Download both the scripts and run `make-debian-mpd-chroot` as root (or using sudo):

```bash
export scriptname="make-debian-mpd-chroot"
export url="https://raw.githubusercontent.com/ronalde/build-latest-mpd-for-debian/master/${scriptname}"
wget -q "${url}" || curl -s -k -o "${scriptname}" "${url}"
sudo bash ${scriptname}
```

Design
------

After building the chroot (using `debootstrap`), the main script
(`make-debian-mpd-chroot`) downloads another helper script
(`build-latest-debian-mpd-package`) inside the chroot and adds it to
the root users `~/.profile`. Furthermore, it modifies the
chroot to automatically logon the root user. This way, after the
script starts the virtual machine with the chroot as its environment,
the helper script is automatically run.

The helper script makes optimal usage of the debian package building
environment and the work done by the package maintainer. Using
`uscan`, `debian/watch`, `uupdate` and the [official git repository
for package
maintenance](http://anonscm.debian.org/cgit/pkg-mpd/pkg-mpd.git), the
latest mpd source packages are downloaded and build using `debbuild`.

When `debbuild` is successful, the helper scripts shuts the virtual
machine down, and the main script copies the generated binary mpd
package to it's local directory. After this, the chroot serves no real
purpose and may be destroyed by the user.

