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

The files in this repository are copies from the newest release. To rebuild a
kernel, run a release's `build.sh` with its `config` beside it, and compare the
SHA-256 it prints. The build goes in `../../target/guest-kernel`, relative to
the script.

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
