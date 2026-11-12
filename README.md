# Agent+ guest kernels and OS layers

Linux kernels and OS layers built for the guest of Agent+, which runs coding
agents in isolated microVMs. `ap` will download them from these releases on
first use, and refuse one unless its SHA-256 matches the digest compiled into
`ap`.

## Kernels

A release tagged `kernel-<release>` holds:

- `vmlinux`, the uncompressed kernel
- `config`, the options applied over `allnoconfig`
- `build.sh`, the script that builds it
- the kernel source archive, and `source.txt` with its kernel.org URL and hash

The files in this repository are copies from the newest release.

## Rebuilding a release

A kernel's digest depends on the compiler that built it, so `build.sh` pins
its toolchain: an `ubuntu:24.04` image named by digest, with packages from the
Ubuntu archive as it was at a fixed snapshot. It needs docker, and an x86-64
host or emulation. It builds in a container with no network, at the same
paths on every machine.

To rebuild a release, put its `build.sh` and `config` in a directory named
`guest/kernel`, and run the script. It downloads the source archive, checks
its hash, builds in `../../target/guest-kernel` relative to itself, and prints
the SHA-256 of `vmlinux`. That digest matches the release's.

`7.3.3-ap.2` and earlier releases attach a `build.sh` that used the machine's
own compiler. They were built with GCC 13.3.0-6ubuntu2~24.04.1 and binutils
2.42-4ubuntu2.10, the toolchain that `build.sh` now pins. To rebuild
`7.3.3-ap.2`, use the `build.sh` in this repository with the release's
`config`.

Linux is licensed under GPL-2.0.

## OS layers

A release tagged `os-minimal-<version>` holds the `OS minimal` layer:

- `minimal.erofs`, the layer, an EROFS image of Debian 13's essential packages
  and git
- `build.sh` and `debian.sh`, the scripts that build it
- `packages.txt`, each Debian package in the layer and its version
- `source.txt`, the Agent+ commit whose EROFS writer made the layer

`build.sh` assembles the root from snapshot.debian.org at one timestamp, and
checks the signed release file of each suite against its pinned SHA-256. Each
package's source is in the same snapshot. The release notes give the layer's
SHA-256.

The files in `os/` are copies from the newest release. To rebuild a layer, you
need an x86-64 Linux host with git, docker, and a Rust toolchain. Another
architecture builds another root, and another digest. Check out Agent+ at the
commit in the release's `source.txt`, copy the release's `build.sh` and
`debian.sh` into its `guest/os`, and run `guest/os/build.sh` from the checkout.
It writes the layer to `target/guest-os/minimal.erofs`, and prints its
SHA-256.

The packages are under their own licenses, which each package's copyright
file in the layer gives.
