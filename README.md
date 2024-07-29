<!--
SPDX-FileCopyrightText: 2024 Ledger SAS

SPDX-License-Identifier: Apache-2.0
-->

# Meson cross files

This repository hold `meson` build system cross files for various targets, that can be shared between users
and for the CI/CD tools.

These cross-files are made in order to work on the Outpost Docker images.

## Meson cross-file naming

<arch-triplet>-<toolchainname>.ini

e.g.
`arm-noeabi-gcc`, that hold the following:

```
[constants]
cross_compile = '/opt/arm-none-eabi/bin/arm-none-eabi-'

[host_machine]
system = 'baremetal'
cpu_family = 'arm'
cpu = 'cortex-m4'
endian = 'little'
exe_wrapper = 'qemu-arm-static'

[binaries]
c = cross_compile + 'gcc'
cpp = cross_compile + 'g++'
ar = cross_compile + 'gcc-ar'
ranlib = cross_compile + 'gcc-ranlib'
strip = cross_compile + 'strip'
objcopy = cross_compile + 'objcopy'
```

## Guildelines on adding new host targets

Adding new targets is made with new dedicated cross-files.

Some requirements are set to avoid any confusion in the usage of crossfiles:

REQ-1. *cross-toolchains path must be absolute*

When executing `meson` on a system where multiple version of a given toolchain
are deployed, the build step would depends on the current environment setting if
the toolchain path is deducated from environment variables such as PATH, and thus
be non-reproductible. The toolchain path must be strictly hardcoded. This should
not be a problem when using Docker images for building Outpost OS.

REQ-2. *If an exe-wrapper exists, it should be added*

`meson` support execution wrapping on build host, allowing to execute non-native
binaries through such exe-wrapper, to directly test the host-compiled content.

