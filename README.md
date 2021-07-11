[![marketplace](https://img.shields.io/badge/marketplace-python--stdeb-black?logo=github)](https://github.com/marketplace/actions/python-stdeb)
[![CI](https://github.com/lpenz/ghaction-python-stdeb/actions/workflows/ci.yml/badge.svg)](https://github.com/lpenz/ghaction-python-stdeb/actions/workflows/ci.yml)
[![github](https://img.shields.io/github/v/release/lpenz/ghaction-python-stdeb?include_prereleases&label=release&logo=github)](https://github.com/lpenz/ghaction-python-stdeb/releases)
[![docker](https://img.shields.io/docker/v/lpenz/ghaction-python-stdeb?label=release&logo=docker&sort=semver)](https://hub.docker.com/repository/docker/lpenz/ghaction-python-stdeb)

# ghaction-python-stdeb

Create a debian package from a python package using [stdeb].


## Usage

Example usage, copied in part from
[ftpsmartsync](https://github.com/lpenz/ftpsmartsync/blob/main/.github/workflows/ci.yml).

```{yml}
jobs:
  packagecloud:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v2
      - id: version
        uses: docker://lpenz/ghaction-version-gen:0.3
      - uses: docker://lpenz/ghaction-python-stdeb:0.1
        with:
          debian_revision: ${{ steps.version.outputs.distance }}
      - uses: docker://lpenz/ghaction-packagecloud:v0.3
        if: steps.version.outputs.version_commit != ''
        with:
          repository: debian/debian/buster
        env:
          PACKAGECLOUD_TOKEN: ${{ secrets.PACKAGECLOUD_TOKEN }}
```


## Inputs

### `working-directory`

Directory to go before running stdeb.

### `debian_revision`

Debian revision number to use.

The full package version generated is the python's package, as defined
in `setup.cfg`, with a `-` and this number appended.


[stdeb]: https://pypi.org/project/stdeb/
