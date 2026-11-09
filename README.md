# Agent+ guest kernels

Linux kernels built for the guest of Agent+, which runs coding agents in
isolated microVMs. `ap` will download a kernel from these releases on first
use, and refuse it unless its SHA-256 matches the digest compiled into `ap`.

Each release holds:

- `vmlinux`, the uncompressed kernel
- `config`, the options applied over `allnoconfig`
- `build.sh`, the script that builds it
- the kernel source archive, and `source.txt` with its kernel.org URL and hash

The files in this repository are copies from the newest release. To rebuild a
kernel, run a release's `build.sh` with its `config` beside it, and compare the
SHA-256 it prints. The build goes in `../../target/guest-kernel`, relative to
the script.

Linux is licensed under GPL-2.0.
