# Heltec V4 "Low Power"

Trying to use newer ESP32 libraries/tools/platforms/whatever
to enable ESP32 power saving features

## Pioarduino

Looks like 
> "PlatformIO won't update esp-idf and dependencies from that old 2024 version? Hold our beer!"

[https://github.com/pioarduino/platform-espressif32]

Community(?) driven replacement for the PlatformIO-provided `espressif32`
platform (which uses very old versions of espressif's libraries/toolchain)

Allows local compilation (and thus configuration) of esp32 libraries ("Hybrid
Compilation") instead of downloading a provided libary with fixed configuration.

### Changing configuration

This is either not correctlly integrated or just messy. After changes, always
delete your `~/.platformio` and local `./.pio` directory, as well as the "Hybrid
Compilation"-produced directories `managed_components`, `.dummy` and files like
`CMakeLists.txt` and `sdkconfig.*` in the top level project directory.

## Changes/Fixes/issues

* libpax does not compile. -> disabled with `MESHTASTIC_EXCLUDE_PAXCOUNTER`
* ADC libary code seems to have changed, old version is still available but
"deprecated". -> deprecated headers included in `variant.h`
* `Syslog` class conflicts with a class with the same name in some dependency.
  -> Relocated in `meshtastic::` namespace.

