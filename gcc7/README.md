# GCC 7.5.0 for Mac OS X Tiger (PowerPC)

## Packages

### Full Installation (~59MB each)
- `gcc7-7.5.0-tiger-ppc.tar.gz` - Full GCC 7 for Tiger
- `gcc7-7.5.0-tiger-ppc32.tar.gz` - 32-bit variant

### Standalone C++ Package (~15MB) - NEW!
- `gcc7-cpp-ppc.tar.gz` - Portable C++11/14/17 with wrapper scripts

## Full Installation

```bash
cd /usr/local
sudo tar xzf gcc7-7.5.0-tiger-ppc.tar.gz
```

After installation, binaries are located at:
- `/usr/local/gcc7/7.5.0/bin/gcc-7` - C compiler
- `/usr/local/gcc7/7.5.0/bin/g++-7` - C++ compiler

To use without full paths, add to your shell profile (`~/.bash_profile`):
```bash
export PATH="/usr/local/gcc7/7.5.0/bin:$PATH"
export DYLD_LIBRARY_PATH="/usr/local/gcc7/7.5.0/lib/gcc/7:$DYLD_LIBRARY_PATH"
```

## Fixing the libisl "Library not loaded" Error

If you see:
```
dyld: Library not loaded: /usr/local/lib/libisl.15.dylib
  Referenced from: .../cc1
  Reason: image not found
```

**Fix** — copy `libisl.15.dylib` to where `dyld` expects it:
```bash
sudo cp /usr/local/gcc7/7.5.0/lib/libisl.15.dylib /usr/local/lib/
```

Or symlink:
```bash
sudo ln -s /usr/local/gcc7/7.5.0/lib/libisl.15.dylib /usr/local/lib/libisl.15.dylib
```

This is a one-time step. `libisl` (Integer Set Library) is included in the tarball but `dyld` looks for it in `/usr/local/lib`.

## Standalone C++ Package

Self-contained with wrapper scripts that handle the libstdc++ path.

```bash
tar xzf gcc7-cpp-ppc.tar.gz
cd gcc7-cpp

./g++11 mycode.cpp -o myprogram   # C++11
./g++14 mycode.cpp -o myprogram   # C++14
./g++17 mycode.cpp -o myprogram   # C++17
./run-cpp ./myprogram              # Run with correct library path
```

**Why wrappers?** Tiger's stock libstdc++ is GCC 4.0 era. Wrappers set `DYLD_LIBRARY_PATH` to use the GCC 7 runtime.

## General Usage

```bash
# C with AltiVec
gcc-7 -mcpu=7450 -maltivec -O2 -o program program.c

# C++17
g++-7 -mcpu=7450 -maltivec -O2 -std=c++17 -o program program.cpp
```

## Tested C++ Features

- **C++11**: Lambdas, auto, range-for, shared_ptr, thread/mutex, move semantics
- **C++14**: Generic lambdas, make_unique, digit separators
- **C++17**: optional, variant, string_view, structured bindings, if-with-initializer

## Architecture
- **Target**: powerpc-apple-darwin8 (Tiger), darwin9 (Leopard)
- **Source**: Tigerbrew

Built with love for vintage Macs - Christmas 2025
