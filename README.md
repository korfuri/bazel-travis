This repository is an illustration of how to use
[Bazel](http://bazel.io) with [Travis-CI](https://travis-ci.org/).

[![Build Status](https://travis-ci.org/korfuri/bazel-travis.svg?branch=master)](https://travis-ci.org/korfuri/bazel-travis)

# How it works

There's a couple of tricks in here to make things work.

Basically, `.travis.yml` lists a couple of apt packages that need to
be installed to compile Bazel, and in `before_install` it clones the
latest version of Bazel and compiles it.

To do so it provides its own `bazelrc` file, which limits the RAM
allocated to Bazel. It also adds more verbosity to the build
process. Note that this `bazelrc` is not used to compile your code:
it's used during the self-hosting process of Bazel.

# How do I use this?

You will need:

  * Everything that's in `.travis.yml`. Note that you can just merge
    that into your existing `.travis.yml` just fine - it doesn't
    specify any exclusive option, except that it requires Linux
    (either container-based or legacy).
  * The `.bazelrc`.
  * Everything under `tools/`, that includes the sha256 fingerprint for Bazel's
    installer.
  * A (possibly empty) `WORKSPACE` file. See below.
  * Your own `script` section in the `.travis.yml` config file.

## Where to put the WORKSPACE file

If you don't know what a WORKSPACE file is, please refer to the
relevant Bazel documentation.

In this example, I chose to put WORKSPACE at the root of the
repository. This has the advantage that I can follow the convention of
having my code in /src/ at the root of the repository and have that be
//src/ for Bazel.

Be careful about using `//...`. This will include paths that are in
Bazel's base workspace, including build utilities for many languages
and environments (Rust, D, Android, etc.). This may make Bazel pull
large blobs from the network.

## Using Bazel in your Travis config

The bazel binary is in ~/bin/. You can do stuff like:

```yaml
script:
  - bin/bazel test //src/...
```

but see the caveat in the "Where to put the WORKSPACE file" section
above if you plan to use `//...`.
