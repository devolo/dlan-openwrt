# dlan-openwrt

This is the devolo dLAN package feed for OpenWrt.
It contains the firmware and tools for the Powerline (PLC) interface of devolo dLAN pro devices.

NOTE: Due to legal reasons we are not allowed to distribute PLC firmware and pib files.

## Usage

This repository is intended to be layered on-top of an OpenWrt buildroot. If you do not have an OpenWrt buildroot installed, see the documentation at: [OpenWrt Buildroot – Installation](http://wiki.openwrt.org/doc/howto/buildroot.exigence) on the OpenWrt support site.

To add it to the buildroot add the following line to feeds.conf.default (or feeds.conf if it exists):
```
src-git dlan https://github.com/devolo/dlan-openwrt.git
```

To install all its package definitions, run:
```
./scripts/feeds update dlan
./scripts/feeds install -a -p dlan
```

Then follow the instructions in README.md inside the dlan-fw package of this repository.

## License

See individual packages.
