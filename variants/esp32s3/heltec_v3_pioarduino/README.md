# Heltev V3 with Pioarduino

## Rebuilding SDK

Before building, and after every change to `sdkconfig`, the following
files/folders need to be removed for a clean rebuild including the platform
library ("Hybrid Compile"):

* in `firmware`:

```
rm -rf .pio/ managed_components/ .dummy/ CMakeLists.txt sdkconfig.*
```

* in your `${HOME}/.platformio/`:
```
rm -rf idf-env.json packages/framework-arduinoespressif32-libs platforms/espressif32
```

```
pio run -e heltec-v3-pioarduino -t clean
```

## SDK config

After cleaning like above:

```
pio run -e heltec-v3-pioarduino -t menuconfig
```

After saving and exitting, copy the resulting `sdkconfig.<environment>` file to
the path referenced in the variant's configuration file.
