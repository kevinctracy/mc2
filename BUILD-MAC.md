# Building on macOS

These notes cover the native SDL/OpenGL build path tested on Apple Silicon macOS.

## Prerequisites

Install the build tools and libraries with Homebrew:

```sh
brew install git-lfs cmake pkg-config sdl2 sdl2_mixer sdl2_ttf glew zlib
```

## Build the game and tools

Configure CMake with Homebrew in the prefix path, then build:

```sh
cmake -S . -B build-mac -DCMAKE_PREFIX_PATH=/opt/homebrew
cmake --build build-mac -j2
```

The main executable is written to `build-mac/mc2`. The asset tools are written under:

```text
build-mac/out/data_tools/
build-mac/out/text_tool/
```

## Build runtime data

Runtime data lives in a separate repository:

```sh
git clone https://github.com/alariq/mc2srcdata.git ../mc2srcdata
```

Copy the generated tools into the data build folder:

```sh
cp build-mac/out/data_tools/aseconv \
   build-mac/out/data_tools/makefst \
   build-mac/out/data_tools/makersp \
   build-mac/out/data_tools/pak \
   build-mac/out/text_tool/text_tool \
   ../mc2srcdata/build_scripts/
```

Build the data using the POSIX/Linux mode:

```sh
make -C ../mc2srcdata/build_scripts all BUILD_PLATFORM=linux
```

Copy the generated runtime files into `build-mac`:

```sh
cp -R ../mc2srcdata/build_scripts/data \
      ../mc2srcdata/build_scripts/assets \
      ../mc2srcdata/build_scripts/*.fst \
      ../mc2srcdata/build_scripts/*.cfg \
      ../mc2srcdata/build_scripts/testtxm.tga \
      build-mac/
cp -R shaders build-mac/
cp build-mac/out/res/libmc2res_64.dylib build-mac/
```

At that point `build-mac` should contain `mc2`, `data`, `assets`, shaders, config files, `.fst` manifests, and `libmc2res_64.dylib`.
