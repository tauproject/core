# Tau Hypercomputing Facility
## Core container image

[![Release status: Pre-production][release-status]](#release-status)
[![Classification: UNRESTRICTED][classification]](#classification)
[![Apache 2.0-licensed][license]](#copyright)
[![Current Travis CI build status][travis]](https://travis-ci.org/tauproject/core)
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Ftauproject%2Fcore.svg?type=shield)](https://app.fossa.io/projects/git%2Bgithub.com%2Ftauproject%2Fcore?ref=badge_shield)

This project builds a [Docker container image](https://hub.docker.com/r/tauproject/core)
that is used as the base image for all other containers in this release of the
Tau Project.

### Release status

| Release track           | Tag/branch    |
|-------------------------|---------------|
| Long-term support (LTS) | Not available |
| Stable                  | Not available |
| Pre-production          | [`shimmer`](https://github.com/tauproject/core/tree/shimmer/master) |
| Legacy                  | Not available |

### Usage

Refer to the [Release status](#release-status) table above for information
on available releases.

These examples will use the pre-production release tag, `shimmer`.

#### Pull the appropriate release image

To pull the image, checking content trust:

```
$ docker pull --disable-content-trust=false tauproject/core:shimmer
```

Start a shell:

```
$ docker run -it tauproject/core:shimmer
root@20e84f276a99:/# ls /
bin   dev  home  lib64	mnt  proc  run	 srv  tmp  var
boot  etc  lib	 media	opt  root  sbin  sys  usr
root@20e84f276a99:/# exit
```

#### Referencing the image

In a `Dockerfile`, specify a `FROM` command as follows:

```
FROM tauproject/core:shimmer
```

### Development

#### Local builds

**Note**: This project can only be built on x86-64 (`amd64`) systems.

##### Prerequisites

The following software must be installed to build this project:

* [Docker](https://www.docker.com)
* [GNU Make](https://www.gnu.org/software/make/), or a BSD Make variant.

Additionally, to build from a Git clone, the following are required:

* [GNU Autoconf](https://www.gnu.org/software/autoconf/)
* [GNU Automake](https://www.gnu.org/software/automake/)

###### Debian GNU/Linux

Enable the Docker for Linux APT repository:

```
$ curl -fsSL https://download.docker.com/linux/debian/gpg | sudo apt-key add -
$ ( echo "deb [arch=amd64] https://download.docker.com/linux/debian $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list )
$ sudo apt update
```

Install the prerequisite packages:

```
$ sudo apt install -y docker-ce make autoconf automake
```

###### macOS

Download and install [Docker for Mac](https://hub.docker.com/editions/community/docker-ce-desktop-mac).

Install the Apple Developer command-line tools (which include GNU Make):

```
$ xcode-select --install
```

Finally, if required, install the packages required for building from a Git
clone using [Homebrew](https://brew.sh):

```
$ brew install autoconf automake
```

##### Prepare a cloned repository

If you have cloned the repository via Git, you must prepare it before it can
be configured:

```
$ cd /path/to/sources
$ autoreconf -fvi
autoreconf: Entering directory `.'
autoreconf: configure.ac: not using Gettext
autoreconf: running: aclocal --force -I buildtools/m4
autoreconf: configure.ac: tracing
autoreconf: configure.ac: not using Libtool
autoreconf: running: /usr/bin/autoconf --force
autoreconf: configure.ac: not using Autoheader
autoreconf: running: automake --add-missing --copy --force-missing
autoreconf: Leaving directory `.'
$
```

Note that the actual output from `autoreconf` may differ. You can omit the `-v`
option to suppress the verbose output.

You should re-run `autoreconf` (and follow the configuration steps below)
whenever you `git pull`, or if the `configure.ac` or any `Makefile.am`
files are modified.

##### Configure the repository

Run the `configure` script in the source directory:--

```
$ ./configure
```

The `configure` script accepts the [standard GNU Autoconf options](https://www.gnu.org/prep/standards/html_node/Configuration.html).

In this case, your build directory would be the same as your source
directory. If you want to build in a separate build directory, change
to it before invoking `configure`:

```
$ mkdir build
$ cd build
$ /path/to/source/configure
```

###### Configuration defaults

* The resulting Docker image will be tagged with `taubuild/core`
* The base image will be `debian/stable-slim`
* Docker Content Trust is enabled during the build

Official builds specify the `--enable-release` configuration option, which
causes the Docker image to be tagged with `tauproject/core` instead of
`taubuild/core`. This option may be useful when testing changes intended to
be contributed to the official releases.

Specify `--with-debian-release=RELEASE` to override the base Debian release
to use for the base image, defaulting to [the current stable release](https://www.debian.org/releases/).

##### Performing builds

To build, run `make` from the build directory to generate the new container
image (by default tagged `taubuild/core:latest`):

```
$ make
```

##### Automated tests

Use `make check` to invoke any automated tests:

```
$ make check
```

These tests are also performed by the Travis CI jobs used to generate the
release images.

##### Manual tests

Launch the container, invoking an interactive shell:

```
$ make shell
...
root@b0e593c86405:/# exit
```

#### Automated builds

##### Travis CI

[Travis CI](https://travis-ci.org/tauproject/core) performs a complete build
using `docker` at least once per day on each release branch and is configured
to push the resulting images to [Docker Hub](https://hub.docker.com/r/tauproject/core/).
These images constitute the official releases.

##### Docker Cloud

Docker Cloud is also configured to perform an automated build. This is to
ensure that the information presented on [Docker Hub](https://hub.docker.com/r/tauproject/core/)
is up to date. The resulting `cloudbuild` tag should be ignored.

### Copyright

Copyright © 2019.

Licensed under the Apache License, Version 2.0 (the "License"); you may not use
this file except in compliance with the License.

You may obtain a copy of the License at:

http://www.apache.org/licenses/LICENSE-2.0

Unless required by applicable law or agreed to in writing, software distributed
under the License is distributed on an **"as-is" basis, without warranties or
conditions of any kind**, either express or implied.  See the License for the 
specific language governing permissions and limitations under the License.

### Classification

This project has been classified **UNRESTRICTED**.

```
@(#) $Tau: core/README.md $
```

[license]: https://img.shields.io/badge/license-Apache%202.0-blue.svg?style=flat-square
[release-status]: https://img.shields.io/badge/release%20status-Pre--production-yellow.svg?style=flat-square
[classification]: https://img.shields.io/badge/classification-UNRESTRICTED-brightgreen.svg?style=flat-square
[travis]: https://img.shields.io/travis/tauproject/core.svg?style=flat-square


## License
[![FOSSA Status](https://app.fossa.io/api/projects/git%2Bgithub.com%2Ftauproject%2Fcore.svg?type=large)](https://app.fossa.io/projects/git%2Bgithub.com%2Ftauproject%2Fcore?ref=badge_large)