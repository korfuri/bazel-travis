This repository is an illustration of how to use
[Bazel](http://bazel.io) with [Travis-CI](https://travis-ci.org/).

[![Build Status](https://travis-ci.org/korfuri/bazel-travis.svg?branch=master)](https://travis-ci.org/korfuri/bazel-travis)

Caveat: currently (as of 2015/07/24), Bazel doesn't provide binary
releases. The `.travis.yml` configuration does a git clone/build from
source for each run, which is quite slow and wasteful. Worse, due to
the way Bazel scripts are set up for self-hosting, this Travis
configuration requires root access (to use `update-alternative`). This
will all get better once Bazel provides binary releases (maybe even a
PPA, that would make things much easier).
