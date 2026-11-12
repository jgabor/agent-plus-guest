# Agent+ guest kernels

Linux kernels built for the guest of Agent+, which runs coding agents in
isolated microVMs. `ap` will download a kernel from these releases on first
use, and refuse it unless its SHA-256 matches the digest compiled into `ap`.

Each release holds:

- `vmlinux`, the uncompressed kernel
- `config`, the options applied over `allnoconfig`
- `build.sh`, the script that builds it
- the kernel source archive, and `source.txt` with its kernel.org URL and hash

The files in this repository are copies from the newest release.

## Rebuilding a release

A kernel's digest depends on the compiler that built it, so `build.sh` pins
its toolchain: an `ubuntu:24.04` image named by digest, with packages from the
Ubuntu archive as it was at a fixed snapshot. It needs docker, and it builds
in a container with no network, at the same paths on every machine.

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
