# Compile Toolchain From Source

This assumes that `~/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf` contains the Pebble SDK build tools, excluding the build toolchain and the SDK itself.

```{note}
Using the Pebble repository won't work on ARM, and might not work on your system. I will upload a better version to GitHub soon.
```

```bash
git clone https://github.com/pebble/arm-eabi-toolchain
cd arm-eabi-toolchain
# Install dependencies
sudo apt install curl flex bison libgmp3-dev libmpfr-dev texinfo \
      libelf-dev autoconf build-essential libncurses5-dev libmpc-dev
# Build and install the toolchain
# Set BUILDSYS to your target triple. You can usually get it by using `gcc -dumpmachine`. On Raspberry Pi OS (32-bit), it's `arm-linux-gnueabihf`.
BUILDSYS=arm-linux-gnueabihf PREFIX=$HOME/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-cs-tools make install-cross
```

If `make` mysteriously exits without a description of the error, after compiling GCC, run the following (remember to change `BUILDSYS` and `PREFIX`):

```bash
BUILDSYS=arm-linux-gnueabihf PREFIX=$HOME/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-cs-tools make install-cross --keep-going
```

## Notes

```
----------------------------------------------------------------------
Libraries have been installed in:
   /home/pi/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-eabi-toolchain//libs/lib

If you ever happen to want to link against installed libraries
in a given directory, LIBDIR, you must either use libtool, and
specify the full pathname of the library, or use the `-LLIBDIR'
flag during linking and do at least one of the following:
   - add LIBDIR to the `LD_LIBRARY_PATH' environment variable
     during execution
   - add LIBDIR to the `LD_RUN_PATH' environment variable
     during linking
   - use the `-Wl,-rpath -Wl,LIBDIR' linker flag
   - have your system administrator add LIBDIR to `/etc/ld.so.conf'

See any operating system documentation about shared libraries for
more information, such as the ld(1) and ld.so(8) manual pages.
```

```
+-------------------------------------------------------------+
| CAUTION:                                                    |
|                                                             |
| If you have not already run "make check", then we strongly  |
| recommend you do so.                                        |
|                                                             |
| GMP has been carefully tested by its authors, but compilers |
| are all too often released with serious bugs.  GMP tends to |
| explore interesting corners in compilers and has hit bugs   |
| on quite a few occasions.                                   |
|                                                             |
+-------------------------------------------------------------+

make[5]: Leaving directory '/home/pi/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-eabi-toolchain/build/gmp'
```

```
This script, last modified 2011-06-03, has failed to recognize
the operating system you are using. It is advised that you
download the most up to date version of the config scripts from

  http://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.guess;hb=HEAD
and
  http://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.sub;hb=HEAD

If the version you run (../../gcc-4.7-2013.05/config.guess) is already up to date, please
send the following data and any information you think might be
pertinent to <config-patches@gnu.org> in order to provide the needed
information to handle your system.
```

```bash
rm gcc-4.7-2013.05/config.guess gcc-4.7-2013.05/config.sub
wget "http://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.guess;hb=HEAD" -O gcc-4.7-2013.05/config.guess
wget "http://git.savannah.gnu.org/gitweb/?p=config.git;a=blob_plain;f=config.sub;hb=HEAD" -O gcc-4.7-2013.05/config.sub
```

ARM patch: <https://lists.freebsd.org/pipermail/freebsd-arm/2015-August/012055.html>

```
configure:5454: gcc -o conftest -g -O2 -I/home/pi/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-eabi-toolchain/libs/include -I/home/pi/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-eabi-toolchain/libs/include -I/home/pi/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-eabi-toolchain/libs/include    conftest.c  -L/home/pi/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-eabi-toolchain/libs/lib -L/home/pi/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-eabi-toolchain/libs/lib -L/home/pi/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-eabi-toolchain/libs/lib -lmpc -lmpfr -lgmp >&5
/usr/bin/ld: /home/pi/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-eabi-toolchain/libs/lib/libgmp.a(divrem_1.o): in function `__gmpn_divrem_1':
divrem_1.c:(.text+0xb0): undefined reference to `__gmpn_invert_limb'
/usr/bin/ld: divrem_1.c:(.text+0x248): undefined reference to `__gmpn_invert_limb'
/usr/bin/ld: /home/pi/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-eabi-toolchain/libs/lib/libgmp.a(divrem_2.o): in function `__gmpn_divrem_2':
divrem_2.c:(.text+0x64): undefined reference to `__gmpn_invert_limb'
/usr/bin/ld: /home/pi/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-eabi-toolchain/libs/lib/libgmp.a(div_q.o): in function `__gmpn_div_q':
div_q.c:(.text+0x184): undefined reference to `__gmpn_invert_limb'
/usr/bin/ld: div_q.c:(.text+0x4b8): undefined reference to `__gmpn_invert_limb'
/usr/bin/ld: /home/pi/pebble-dev/pebble-sdk-4.6-rc2-linux-armv7hf/arm-eabi-toolchain/libs/lib/libgmp.a(div_q.o):div_q.c:(.text+0x5bc): more undefined references to `__gmpn_invert_limb' follow
collect2: error: ld returned 1 exit status
configure:5454: $? = 1
configure: failed program was:
| /* confdefs.h */
| #define PACKAGE_NAME ""
| #define PACKAGE_TARNAME ""
| #define PACKAGE_VERSION ""
| #define PACKAGE_STRING ""
| #define PACKAGE_BUGREPORT ""
| #define PACKAGE_URL ""
| #define LT_OBJDIR ".libs/"
| /* end confdefs.h.  */
| #include <mpc.h>
| int
| main ()
| {
| 
|     mpfr_t n;
|     mpfr_t x;
|     mpc_t c;
|     int t;
|     mpfr_init (n);
|     mpfr_init (x);
|     mpfr_atan2 (n, n, x, GMP_RNDN);
|     mpfr_erfc (n, x, GMP_RNDN);
|     mpfr_subnormalize (x, t, GMP_RNDN);
|     mpfr_clear(n);
|     mpfr_clear(x);
|     mpc_init2 (c, 53);
|     mpc_set_ui_ui (c, 1, 1, MPC_RNDNN);
|     mpc_cosh (c, c, MPC_RNDNN);
|     mpc_pow (c, c, c, MPC_RNDNN);
|     mpc_acosh (c, c, MPC_RNDNN);
|     mpc_clear (c);
| 
|   ;
|   return 0;
| }
configure:5458: result: no
configure:5479: error: Building GCC requires GMP 4.2+, MPFR 2.3.1+ and MPC 0.8.0+.
Try the --with-gmp, --with-mpfr and/or --with-mpc options to specify
their locations.  Source code for these libraries can be found at
their respective hosting sites as well as at
ftp://gcc.gnu.org/pub/gcc/infrastructure/.  See also
http://gcc.gnu.org/install/prerequisites.html for additional info.  If
you obtained GMP, MPFR and/or MPC from a vendor distribution package,
make sure that you have installed both the libraries and the header
files.  They may be located in separate packages.
```

```
/usr/bin/ld: libsim.a(maverick.o):(.bss+0x0): multiple definition of `DSPregs'; libsim.a(wrapper.o):(.bss+0x8): first defined here
/usr/bin/ld: libsim.a(maverick.o):(.bss+0x80): multiple definition of `DSPacc'; libsim.a(wrapper.o):(.bss+0x88): first defined here
/usr/bin/ld: libsim.a(maverick.o):(.bss+0xa0): multiple definition of `DSPsc'; libsim.a(wrapper.o):(.bss+0xa8): first defined here
/usr/bin/ld: libsim.a(armemu32.o):(.bss+0x0): multiple definition of `isize'; libsim.a(armemu26.o):(.bss+0x0): first defined here
collect2: error: ld returned 1 exit status
```