# Fuzz targets for Racket CS

The harness that [OSS-Fuzz](https://github.com/google/oss-fuzz/tree/master/projects/racket)
uses to fuzz Racket CS, as the `racket-fuzz` package.

- `safe-eval.rkt`: the `fuzz-*` entry points. Each takes an input as a
  byte string, runs it in a sandbox, and checks one property.
- `libs.rkt`: the libraries that fuzzed programs may `require`.
- `driver.c`: the libFuzzer driver, which boots Racket CS and calls one
  entry point on each input.
- `embed-modules.rkt` and `embed-data.S`: embed the boot files and the
  compiled harness in a fuzz target, so that it needs no files at run time.
- `make-corpus.rkt`: makes seed corpora from Racket's core tests.
- `*.dict`: libFuzzer dictionaries.

Install the package into the Racket CS that the targets link against,
then make the module bundle and the seeds with

    raco pkg install --scope installation --copy <this directory>
    racket -l- racket-fuzz/embed-modules fuzz-modules.bin
    racket -l- racket-fuzz/make-corpus <racket>/pkgs/racket-test-core/tests/racket corpus

See `build.sh` of the OSS-Fuzz project for the whole build.

Licensed under either the [Apache License, Version 2.0](LICENSE-APACHE.txt)
or the [MIT license](LICENSE-MIT.txt), at your option.
