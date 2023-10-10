# Termux-hermes
This repo contains How to build hermes on <b>termux</b> with pre-compiled binary <b><a href="https://github.com/AbhiTheModder/termux-hermes/releases">releases</a></b> also 

<b> NOTE:</b> <code> If you want Pre-compiled binares you can find them in releases section of this Repo</code>

## Building
Follow the Guide carefully else don't report me if you run into errors. <b>YOu can also watch this small video</b> on how to build it.

<iframe width="420" height="315"
src="https://youtu.be/loObsRvKi1s?si=5HrJmj2tX6uVPfdd">
</iframe>

- <b> Update Termux & packages: </b>
```
termux-setup-storage && apt -upgrade -y
```
- <b> Installing Dependencies: </b>
  ```
  pkg install cmake git ninja libicu python zip readline
  ```
  
- <b> Debug Build: </b>

Hermes will place its build files in the current directory by default.
You can also give explicit source and build directories, use `--help` on the build scripts to see how.

Create a base directory to work in, e.g. `~/workspace`, and cd into it.
(Tip: avoid naming it `hermes`, as `hermes` will be one of several subdirectories in the workspace).
After `cd`ing, follow the steps below to generate the Hermes build system:

<b> NOTE: If you want to build hermes for custom version bytecode download the archive(zip/tar.gz) files from <a href="https://github.com/facebook/hermes/tags">hermes tag</a> repo</b> and skip this process

    git clone https://github.com/facebook/hermes.git

- <b>Now simply run :</b>     `cmake -S hermes -B build -G Ninja`

- <b>The build system has now been generated in the `build` directory. To perform the build: </b>

    `cmake --build ./build`

- <b> Release Build: </b>

The above instructions create an unoptimized debug build. The `-DCMAKE_BUILD_TYPE=Release` flag will create a release build:
<b> NOTE: If you want to build hermes for custom version bytecode download the archive(zip/tar.gz) files from <a href="https://github.com/facebook/hermes/tags">hermes tag</a> repo</b> and skip this process

- <b>Now simply run :</b>     cmake -S hermes -B build_release -G Ninja -DCMAKE_BUILD_TYPE=Release
- 
- <b>The build system has now been generated in the `build_release` directory. To perform the build: </b>

    `cmake --build ./build`
  
## Running Hermes

The primary binary is the `hermes` tool, which will be found at `build/bin/hermes`. This tool compiles JavaScript to Hermes bytecode. It can also execute JavaScript, from source or bytecode or be used as a REPL.

### Executing JavaScript with Hermes

    hermes test.js

## Compiling and Executing JavaScript with Bytecode

    hermes -emit-binary -out test.hbc test.js
    hermes test.hbc

## Running Tests

To run the Hermes test suite:

    cmake --build ./build --target check-hermes

To run Hermes against the test262 suite, you need to have a Hermes binary built
already and a clone of the [test262 repo](https://github.com/tc39/test262/):

    hermes/utils/testsuite/run_testsuite.py -b <hermes_build> <test262>

E.g. if we configured at `~/hermes_build` (i.e. `~/hermes_build/bin/hermes` is
an executable) and cloned test262 at `~/test262`, then perform:

    hermes/utils/testsuite/run_testsuite.py -b ~/hermes_build ~/test262/test

Note that you can also only test against part of a test suite, e.g. to run the
Intl402 subset of the test262, you can specifiy a subdir:

    hermes/utils/testsuite/run_testsuite.py -b ~/hermes_build ~/test262/test/intl402

## Formatting Code

To automatically format all your changes, you will need `clang-format`, then
simply run:

    hermes/utils/format.sh

## AddressSanitizer (ASan) Build

 The `-HERMES_ENABLE_ADDRESS_SANITIZER=ON` flag will create a ASan build:

    git clone https://github.com/facebook/hermes.git
    cmake -S hermes -B asan_build -G Ninja -D HERMES_ENABLE_ADDRESS_SANITIZER=ON
    cmake --build ./asan_build

You can verify the build by looking for `asan` symbols in the `hermes` binary:

    nm asan_build/bin/hermes | grep asan

### Other Tools

In addition to `hermes`, the following tools will be built:

- `hdb`: JavaScript command line debugger
- `hbcdump`: Hermes bytecode disassembler
- `hermesc`: Standalone Hermes compiler. This can compile JavaScript to Hermes bytecode, but does not support executing it.
- `hvm`: Standalone Hermes VM. This can execute Hermes bytecode, but does not support compiling it.


- <b> Release Build </b>

The above instructions create an unoptimized debug build. The `-DCMAKE_BUILD_TYPE=Release` flag will create a release build:

    cmake -S hermes -B build_release -G Ninja -DCMAKE_BUILD_TYPE=Release
    cmake --build ./build_release

### License

Hermes is [MIT licensed](./LICENSE).
