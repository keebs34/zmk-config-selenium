# zmk-config-selenium

This is a [ZMK](https://zmk.dev/) implementation of the [Selenium keymap](https://onedeadkey.github.io/selenium/).

This repository also contains Selenium firmware builds for these keyboards:
- [Corne](https://github.com/foostan/crkbd)
- [Ferris](https://github.com/pierrechevalier83/ferris)
- [Glove80](https://www.moergo.com/)
- [Quacken](https://github.com/Nuclear-Squid/Quacken)
- [Sweep](https://github.com/davidphilipbarr/Sweep)
- [TOTEM](https://github.com/GEIGEIGEIST/TOTEM)

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
You will only need 2 things in this file:
- a [`SELENIUM_KEYMAP_BINDINGS`](include/selenium/bindings.h) definition for your keyboard geometry
- a `#include "../zmk-config-selenium/include/selenium/selenium.keymap"` line at the very end of the file
- optionally, you can `#define` some of the options documented in [`selenium.keymap`](include/selenium/selenium.keymap)

See the [TOTEM](config/totem.keymap) or the [Glove80](config/glove80.keymap) keymaps for some examples.
Do not copy the `#include` line from these example, a different path is needed
when building with `zmk-config-selenium` as an external module.

3. Build your keymap

Commit these changes and push them to your repository if you build your firmware using GitHub actions.

Run `west update` and compile your firmware as usual if you are building it locally.

Once the firmware is built, it should be using [Selenium](https://onedeadkey.github.io/selenium/)!

### Pros and cons
- Reuse existing ZMK config setup
- Can build for multiple keyboards at once
- Can use different `zmk-config-selenium` options for different keyboards if needed
- Cannot tweak the keymap
