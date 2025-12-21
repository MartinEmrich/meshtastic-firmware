# Heltec V4 "Low Power"

(... and Heltec V3 in `../heltec_v3_lp`)

Trying to use newer ESP32 libraries/tools/platforms/whatever
to enable ESP32 power saving features.

/!\ For whatever reason, after `pio upload` the device does not boot right away.
    Connecting to serial or pushing reset is needed.

## Pioarduino

Looks like 
> "PlatformIO won't update esp-idf and dependencies from that old 2024 version? Hold our beer!"

[https://github.com/pioarduino/platform-espressif32]

Community(?) driven replacement for the PlatformIO-provided `espressif32`
platform (which uses very old versions of espressif's libraries/toolchain).
The story is polical, messy and on Github for your entertainment...

It allows local compilation (and thus configuration) of esp32 libraries ("Hybrid
Compilation") instead of downloading a provided libary with fixed configuration.

### Changing configuration

This is either not correctly integrated or just messy. After changes, always
delete your `~/.platformio` and local `./.pio` directory, as well as the "Hybrid
Compilation"-produced directories `managed_components`, `.dummy` and files like
`CMakeLists.txt` and `sdkconfig.*` in the top level project directory.

## Changes/Fixes/issues

### Worked around

* Lots of `CONFIG_BT...` options defined both in sdkconfig and `build_flags`,
  breaks at `-Werror`. -> Removed from `build_flags`, sadly `build_unflags` did
 not work, thus looks messy.

* libpax does not compile. -> disabled with `MESHTASTIC_EXCLUDE_PAXCOUNTER` and
  patched out.

* ADC libary code seems to have changed, old version is still available but
  "deprecated". -> deprecated headers included in `variant.h`, any warnings
  deactivated via CONFIG_ options. _Unclear whether the result works at all_

* `Syslog` class conflicts with a class with the same name in some dependency.
  -> Relocated in `meshtastic::` namespace.

* BLE seems to no longer work after https://github.com/meshtastic/firmware/commit/40f1f91c0d6b7ff859fbbf4d67511000548d74ee
  (Upgrade to Nimble 2.x). Device appears, but phone cannot connect.
  -> Rebased on the last commit before restored BLE functionality.

### Open/Looming

* Lot's of "redefined" warnings around Nimble Bluetooth config options. I
  suspect: Both the (newer) pioarduino tree and meshtastic pull in/provide 
  (conflicting) Nimble Bluetooth instances.
* Had to put a `default_16MB.csv` partition table here. Not clear why and which,
  pulled one from esp-idf.
