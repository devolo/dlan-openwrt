## Description

Firmware and basic setup scripts for the PLC interface.

## Usage
You have to build a firmware image containing the dlan-fw packages since it requires
setsid which is not selected for busybox in stock OpenWrt/LEDE firmware. I.e if you
build the dLAN packages with the SDK they won't work due to missing setsid.

After selecting the profile for your dLAN device select the firmware package
'''dlan-fw-<device>''' that matches the device
(firmware and PIB files are device specific and won't work on any other device
type). Choosing a firmware package will autoselect dlan-plc and other
dependencies which contain the scripts and tools necessary to bring up the PLC
interface.

Device | Required PLC firmware package
---|---
dLAN pro 1200+ WiFi ac|dlan-fw-pro-1200-ac
dLAN pro 1200+ WiFi n|dlan-fw-pro-1200-n
dLAN pro 500 Wireless+|dlan-fw-pro-500-wp
dLAN Hotspot|dlan-fw-hotspot

After installing the new firmware use the init script ```/etc/init.d/plc``` to
start or stop the PLC interface. By default its enabled to be started automatically
at boot time. To disable it permanently call ```/etc/init.d/plc disable```,
reenable with ```/etc/init.d/plc enable```.

## PLC push button pairing
Push button pairing is started by pressing the Home/dLAN button for max 3 seconds.
Pressing it again before the pairing is finished stops the pairing process.
Pressing the button for more than 10 seconds randomizes the NMK to leave the AVLN.
Pressing between 3s and 10s has no effect.

## License

Firmware NVM/PIB files in qca/:
Copyright (c) Qualcomm Atheros, Inc.
See LICENSE in qca/*

All other files:
Copyright (c) 2023 devolo GmbH
