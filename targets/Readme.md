# Fuzzing targets for Radare2

This folder contains fuzzing targets for Radare2.

## Building the targets

The build process is designed to be able to use different fuzzing engines, such as AFL or libFuzzer. In order to build the fuzz targets the following environment variables must be set:

- LIB_FUZZING_ENGINE: Specifies the path to the fuzzing engine library. Example: `LIB_FUZZING_ENGINE=/usr/lib/libFuzzer.a`
- RADARE2_STATIC_BUILD: Specifies the path to the static Radare2 build that will be used to build the fuzzing targets. It is important to build Radare2 with sanitizing and coverage.

Below is an example on how to build the fuzzing targets with libFuzzer:

```
CXX=clang++ CXXFLAGS="-fsanitize=address -fsanitize-coverage=trace-pc-guard" LIB_FUZZING_ENGINE=/usr/lib/libFuzzer.a RADARE2_STATIC_BUILD=/r2-static/ make
```

And this one is an example on how to build Radare2 with Address Sanitizer (ASAN) and coverage enabled:

```
USERCC=clang CC=clang CFLAGS="-O0 -g -fsanitize=address -fsanitize-coverage=trace-pc-guard" LDFLAGS="-fsanitize=address" ./configure && make
```

## Running the fuzzers

In order to run the fuzzer some basic options should be taken into account, such as the following:

- workers=2: Determines how many processess will be run.
- jobs=100: Specifies the amount of jobs that will be run.
- close_fd_mask=3: Closes stdout and stderr.
- detect_leaks=0: Disable memory leak detection

Check other options in the [libFuzzer documentation](https://llvm.org/docs/LibFuzzer.html#options)

Example:

```
./ia_fuzz MY_CORPUS corpora/ia_fuzz -workers=4 -jobs=100 -close_fd_mask=3
```


## Minimizing the Corpus

In order to minimize the generated corpora just use the `-merge=1` option. Example:

```
./ia_fuzz MINIMIZED_CORPUS MY_CORPUS -merge=1
```

