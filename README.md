# Zyn-Fusion Build Scripts

These are the build scripts used to generate the Zyn-Fusion packages.

These build scripts (and only these build scripts) are licensed under the
WTFPL.

---

## Fetch repositories

You need to fetch this repo first.

```shell
git clone https://github.com/zynaddsubfx/zyn-fusion-build zyn-fusion-build
```
```shell
cd zyn-fusion-build
```

## How to build

Zyn-Fusion now uses Makefile. Each platform has a corresponding Makefile (`Makefile.<platform>.mk`):

| Makefile              | Target platform (Where Zyn runs) | Host platform (where compilers run) |
| --------------------- | -------------------------------- | ----------------------------------- |
| `Makefile.linux.mk`   | Linux (native build)             | Linux                               |
| `Makefile.windows.mk` | Windows                          | Linux **(cross compile)**           |
| `Makefile.mingw64.mk` | Windows                          | Msys2 Mingw-w64 64 bit              |

Since they are not in the default Makefile name, you need to explicitly specify them via parameter`-f` when invoking `make`.

Makefiles will automatically download ZynAddSubFX and Zest, as well as their dependencies. Then, automatically build them altogether.

**Built packages are put in directory `./build/` :**

- `./build/zyn-fusion`: Ready-to-use Zyn-Fusion files. You can directly use them as you wish, or copy this folder into your DAW's search-path.
- `./build/zyn-fusion/zyn-fusion-*.{tar.gz|zip}`:  Compressed package(s) ready for distribution.

### Building for Linux (native build)

#### Generic

```bash
# Install build dependencies
make -f Makefile.linux.mk install_deps

# Start building
make -f Makefile.linux.mk all

# Or, you can also build a specific component,
# then finally use `package` to get a package file
make -f Makefile.linux.mk zynaddsubfx
make -f Makefile.linux.mk zest
make -f Makefile.linux.mk package
```

> **NOTICE:** You need to run `install-linux.sh` within the built folder to install Zyn-Fusion properly, or it won't run, moreover you'll only see a black window in your host.

### Building for Windows (cross-compile on Linux)

```bash
# Install build dependencies
make -f Makefile.windows.mk install_deps

# Start building
make -f Makefile.windows.mk all

# Or, you can also build a specific component,
# then finally use `package` to get a package file
make -f Makefile.windows.mk zynaddsubfx
make -f Makefile.windows.mk zest
make -f Makefile.windows.mk package
```

### Building for Windows (native build via Msys2)

You must install [Msys2](https://www.msys2.org/) first, then **remember choosing `Mingw-w64 64 bit ` in MinTTY** (you can find it in Start Menu). 

The **default MSYS environment** is based on Cygwin, which **won't work**!

steps in order to build for windows:

-  Download msys2 then open the shell and type those commands respectively:

   -  run `update pacman -Syu`. 
   -  run `pacman -su`.

-  Open msys2-mingw64 shell from the start menu.
 
-  run `pacman -s`.
  
-  run `pacman --needed -s git mingw-w64-x86-gcc base-devel`.

-  run `pacman mingw-w64-x86-toolchain`.

-  Add path to bin directory for both msys64 and mingw64 to the system enviroment variables.

   For msys64: `..\msys64\mingw64\bin` 
   For mingw64 `...\msys64\usr\bin` 

Continue with the commands below:

```bash
# Install build dependencies
make -f Makefile.mingw64.mk install_deps

# Start building
make -f Makefile.mingw64.mk all

# Or, you can also build a specific component,
# then finally use `package` to get a package file
make -f Makefile.mingw64.mk zynaddsubfx
make -f Makefile.mingw64.mk zest
make -f Makefile.mingw64.mk package
```

### Moreover

- **Build types (modes):** You can choose either `demo` or `release`, and `demo` is the default. Demo build will automatically mute after 10 minutes.

  You can explicitly specify build type:
  
  ```bash
  make MODE=release -f Makefile.<platform>.mk <target>
  ```
  
- **Get help:** You can get a list of `make` targets by invoking:

  ```bash
  make -f Makefile.<platform>.mk help
  ```
