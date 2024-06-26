## JPEG XL UTF-8 Patch for Windows

This [patch](https://github.com/JacobDev1/libjxl-utf8/blob/main/utf8_0.10.2.patch) enables UTF-8 support in `cjxl`, `djxl`, `cjpegli`, `djpegli`, `jxlinfo` on Windows.

- [Download patched pre-builds](https://github.com/JacobDev1/libjxl-utf8/releases)
- [Just the patch](#just-the-patch)
- [Full compilation guide](#full-compilation-guide)

## Just the Patch

```bash
cd libjxl/
wget https://raw.githubusercontent.com/JacobDev1/libjxl-utf8/main/utf8_0.10.2.patch
git apply utf8_0.10.2.patch
```

Compatible with `libjxl 0.10.2`

## Full Compilation Guide

Install [MSYS2](https://msys2.org) and launch **MSYS2 MINGW64**

Synchronize packages

```bash
pacman -Syu
```

MSYS2 will close, open it again, and install the packages

```bash
pacman -S --needed git base-devel mingw-w64-x86_64-toolchain mingw-w64-x86_64-cmake mingw-w64-x86_64-ninja mingw-w64-x86_64-gtest mingw-w64-x86_64-giflib mingw-w64-x86_64-libpng mingw-w64-x86_64-libjpeg-turbo
```

Clone and enter the repo

```bash
git clone --depth 1 -b v0.10.2 https://github.com/libjxl/libjxl
cd libjxl/
```

Download and apply the patch

```bash
wget https://raw.githubusercontent.com/JacobDev1/libjxl-utf8/main/utf8_0.10.2.patch
git apply utf8_0.10.2.patch
```

Download dependencies

```bash
./deps.sh
```

Generate a makefile

```bash
mkdir build
cd build
cmake -DCMAKE_BUILD_TYPE=Release -DBUILD_TESTING=OFF -DBUILD_SHARED_LIBS=OFF -DJPEGXL_ENABLE_BENCHMARK=OFF -DJPEGXL_ENABLE_PLUGINS=ON -DJPEGXL_ENABLE_MANPAGES=OFF -DJPEGXL_FORCE_SYSTEM_BROTLI=ON -DJPEGXL_FORCE_SYSTEM_GTEST=ON ..
```

Build

```bash
cmake --build .
```

You will find the tools in `libjxl/build/tools/`.

To run them outside of MSYS2, you need to include the necessary DLLs in the same folder.

### The Easy Way

1. Make a new folder anywhere, let's call it `libjxl-tools`
2. Copy the EXEs from `C:/msys64/home/user/libjxl/build/tools/` to `libjxl-tools`
3. Copy the DLLs mentioned below from `C:/msys64/mingw64/bin/` into `libjxl-tools`

- zlib1.dll
- libwinpthread-1.dll
- libstdc++-6.dll
- libpng16-16.dll
- libjpeg-8.dll
- libgif-7.dll
- libgcc_s_seh-1.dll
- libbrotlienc.dll
- libbrotlidec.dll
- libbrotlicommon.dll

4. Click on `cjxl.exe` in `libjxl-tools`. If you get DLL errors, also include those DLLs.

### The Better Way

Import `cjxl.exe` into a [dependency walker](https://github.com/lucasg/Dependencies). Any DLL not linked to `system32` needs to be copied over from `C:/msys64/mingw64/bin/`.

## See Also

- [How libavif added UTF-8 support](https://github.com/AOMediaCodec/libavif/commit/3ec01cefd1ddd266a622d5e114a0888581b68f4a)