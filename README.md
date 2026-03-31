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

## Interactive firmware builder

1. Fork `zmk-config-selenium`

2. Build with GitHub Actions
  - Open your fork in your browser: https://github.com/username/zmk-config-selenium
  - Click on the *Actions* tab
  - In the *All workflows* column to the left, choose "Build ZMK firmware with user config"
  - Click on "Run workflow" on the right
  - Select the options that you want
  - Click on the green "Run workflow" button
  - Wait for the build to complete
  - Download the `firmware-selenium` artifact which appeared at the bottom of the *Summary* page

3. Flash the firmware to your keyboard

### Pros and cons

- Relatively easy build of a firmware
- Limited to a set of predefined keyboards
- Need to set the options every time
- Cannot build for multiple keyboards at once
- Cannot tweak the keymap


## Customizing `zmk-config-selenium` firmwares

`zmk-config-selenium` build firmwares for a number of keyboards with Selenium’s default options.
If you want to build a keyboard firmware with a different set of options, follow these steps:

1. Fork `zmk-config-selenium`

You can edit `build.yaml` to remove the keyboard firmwares you don’t need from the build.
If your keyboard is not listed in this file, follow the steps from [this section](#adding-a-new-keyboard-to-zmk-config-selenium).

2. Set the options you want

The available options are described in [`settings.h`](include/selenium/settings.h).
You can either `#define` them in this file, they will be shared by all the keyboards that you build.
Or you can `#define` these options in the keyboard keymap definition in
[`config`](config), in this case, they will only be set for this specific
keyboard.

3. Build the keymap

After committing your changes and pushing them to your fork, a firmware for
your keyboard should be built automatically in GitHub Actions!

### Pros and cons
- Only build the firmware(s) you need
- Must use `git rebase` or `git merge` to get newer `zmk-config-selenium` features
- Can build firmwares for multiple keyboards at once
- Will use the same set of options for all builds


## Using `zmk-config-selenium` with an existing ZMK project

If you already have a ZMK configuration for your keyboard in GitHub which you
use to build firmwares for your keyboard, you can use `zmk-config-selenium` as
an external Zephyr module.

1. Modify `west.yml`

In your ZMK project, edit `config/west.yml` and add to the `manifest/remotes` section:
```
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    - name: keebs34
      url-base: https://github.com/keebs34
```

Add to the `manifest/projects` section:
```
    - name: zmk-config-selenium
      remote: keebs34
      revision: main
```

2. Modify your keymap

Your keymap should be defined in `config/$keyboard.keymap`.
You will only need 2 things in this file:
- a [`SELENIUM_KEYMAP_BINDINGS`](include/selenium/bindings.h) definition for your keyboard geometry
- a `#include "../zmk-config-selenium/include/selenium/selenium.keymap"` line at the very end of the file
- optionally, you can `#define` some of the options documented in [`settings.h`](include/selenium/settings.h)

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


# Adding a new keyboard to `zmk-config-selenium`

1. Fork `zmk-config-selenium`

2. Add your keyboard to `build.yaml`

If your keyboard is already listed in this file, you are all set.
You can remove all the keyboards definitions you don’t need from it.
If your keyboard is not listed, you will need to add it.
The syntax is the same as the one for ZMK `build.yaml` files.

If your keyboard is not part of `zmkfirmware/zmk` and needs an external ZMK
module, add it to `config/west.yml`.

3. Create a keymap

Your keymap should be defined in `config/$keyboard.keymap`.
In our example, it will be `config/totem.keymap` for a TOTEM keyboard.

You will only need 2 things in this file:
- a [`SELENIUM_KEYMAP_BINDINGS`](include/selenium/bindings.h) definition for your keyboard geometry
- a `#include "../include/selenium/selenium.keymap"` line at the very end of the file
- optionally, you can `#define` some of the options documented in [`settings.h`](include/selenium/settings.h)

See the [TOTEM](config/totem.keymap) or the [Glove80](config/glove80.keymap) keymaps for some examples.

4. Build the keymap

After committing your changes and pushing them to your fork, a firmware for
your keyboard should be build automatically in GitHub Actions!

5. (optional) File a PR

You can contribute the integration of your new keyboard to
`zmk-config-selenium` so that others can also benefit from it.

[File a pull request](https://github.com/keebs34/zmk-config-selenium/compare)
with the `build.yaml` changes and the keymap definition without any options set.
