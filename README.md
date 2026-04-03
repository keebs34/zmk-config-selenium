# zmk-config-selenium

This is a [ZMK](https://zmk.dev/) implementation of the [Selenium keymap](https://onedeadkey.github.io/selenium/).

This repository also contains Selenium firmware builds for these keyboards:
- [Corne](https://github.com/foostan/crkbd)
- [Ferris](https://github.com/pierrechevalier83/ferris)
- [Glove80](https://www.moergo.com/)
- [Quacken](https://github.com/Nuclear-Squid/Quacken)
- [Sweep](https://github.com/davidphilipbarr/Sweep)
- [Totem](https://github.com/GEIGEIGEIST/TOTEM)

# Usage

## Using `zmk-config-selenium` with an existing ZMK project

If you already have a ZMK configuration for your keyboard in GitHub which you
use to build firmwares for your keyboard, you can use `zmk-config-selenium` as
an external Zephyr module.

1. Modify `west.yml`

In your ZMK project, edit `config/west.yml` and add to the `manifest/remotes` section:
```
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    - name: OneDeadKey
      url-base: https://github.com/OneDeadKey
```

Add to the `manifest/projects` section:
```
    - name: zmk-config-selenium
      remote: OneDeadKey
      revision: main
```

2. Modify your keymap

Your keymap should be defined in `config/$keyboard.keymap`.
Replace its whole content with something like:
```
#define KB_LAYOUT_ERGOL
#define VIM_NAVIGATION
#define HRM_SHIFT

#define SELENIUM_KEYMAP_BINDINGS(LOUT1,  LROW1,  RROW1,  ROUT1, \
                                 LOUT2,  LROW2,  RROW2,  ROUT2, \
                                 LOUT3,  LROW3,  RROW3,  ROUT3, \
                                 LT1, LT2, LT3,  RT3, RT2, RT1) \
          LROW1   RROW1       \
          LROW2   RROW2       \
    LOUT3 LROW3   RROW3 ROUT3 \
    LT1 LT2 LT3   RT3 RT2 RT1


#include "../zmk-config-selenium/include/selenium/selenium.keymap"
```

`SELENIUM_KEYMAP_BINDINGS` must be defined to adjust Selenium to your keyboard geometry.
You can find some examples in [`zmk-config-selenium/config`](https://github.com/OneDeadKey/zmk-config-selenium/tree/main/config).

At the end of your keymap, you must have this line:
```
#include "../zmk-config-selenium/include/selenium/selenium.keymap"
```

The `#define` at the beginning are optional and allow to set some
`zmk-config-selenium` options. These are described at the [beginning of the
Selenium keymap]
(https://github.com/OneDeadKey/zmk-config-selenium/blob/main/include/selenium/selenium.keymap).

3. Build your keymap

Commit these changes and push them to your repository if you build your firmware using GitHub actions.

Run `west update` and compile your firmware as usual if you are building it locally.

Once the firmware is built, it should be using [Selenium](https://onedeadkey.github.io/selenium/)!

### Pros and cons
- Reuse existing ZMK config setup
- Can build for multiple keyboards at once
- Can use different `zmk-config-selenium` options for different keyboards if needed
- Cannot tweak the keymap
