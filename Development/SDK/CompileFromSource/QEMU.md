# QEMU

Pebble's fork of QEMU is available at [@pebble/qemu](https://github.com/pebble/qemu). The `qemu_micro_flash.bin` and `qemu_spi_flash.bin` files are available at [@pebble/qemu-tintin-images](https://github.com/pebble/qemu-tintin-images).

```{note}
The latest Pebble SDK has version `v2.5.0-pebble3`, but this version has `v2.5.0-pebble4`.
```

```bash
sudo apt install libpixman-1-dev
git clone https://github.com/pebble/qemu.git --branch v2.5.0-pebble4
cd qemu
git submodule update --init dtc
./configure --disable-werror --enable-debug --target-list="arm-softmmu" \
    --extra-cflags="-DSTM32_UART_NO_BAUD_DELAY"
make
cp arm-softmmu/qemu-system-arm ~/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/bin/qemu-pebble
```

## Changes I made to help it build

### util/memfd.c

I replaced all whole-word occurences of `memfd_create` with `qemu_memfd_create`, because `memfd_create` is already declared by one of the ARM headers. This issue appears not to have popped up elsewhere, because it doesn't exist in the x86 headers.

### qga/commands-posix.c

I added `#include <sys/sysmacros.h>` underneath `#include <sys/types.h>`.

From (supposedly) the GLibC release notes:


>   The inclusion of `<sys/sysmacros.h>` by `<sys/types.h>` is deprecated.
    This means that in a future release, the macros `major`, `minor`, and
    `makedev` will only be available from `<sys/sysmacros.h>`.
>
>   These macros are not part of POSIX nor XSI, and their names frequently
    collide with user code; see for instance glibc bug 19239 and Red Hat
    bug 130601. `<stdlib.h>` includes `<sys/types.h>` under `_GNU_SOURCE`, and
    C++ code presently cannot avoid being compiled under `_GNU_SOURCE`,
    exacerbating the problem.


This issue was pointed out by @KKomarov on [a pull request to @pebble/qemu](https://github.com/pebble/qemu/pull/18#issuecomment-544914387).