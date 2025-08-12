# ZMK Board Iris CE

[ZMK](https://zmk.dev) Board definition for the [Iris CE](https://keeb.io/products/iris-ce-keyboard).

## DISCLAIMER
This is not associated with or supported by Keebio. Using this may affect your warranty.

Currently this board builds against the 4.1 version of ZMK on the `main` branch, and will be tagged to `v0.4` once that
is released (will aim to keep this up to date with future releases).

## Building

### In ZMK Config
```yaml
manifest:
  defaults:
    revision: main # bump to v0.4 once released
  remotes:
    - name: zmkfirmware
      url-base: https://github.com/zmkfirmware
    - name: paulshir
      url-base: https://github.com/paulshir
  projects:
    - name: zmk
      remote: zmkfirmware
      import: app/west.yml
    - name: zmk-board-iris-ce
      remote: paulshir
  self:
    path: config
```

### Locally
1. Clone this board repository locally.
2. Follow the [building and flashing instructions from ZMK](https://zmk.dev/docs/development/build-flash)
3. For the build command, specify this board repository as an extra module when building.

Example usage
```
# After setting up zmk
git clone https://github.com/paulshir/zmk-keyboard-iris-ce
cd zmk/app
west update

west build -p -d build/iris_ce_left -b iris_ce_left -- -DZMK_EXTRA_MODULES=/path/to/zmk-board-iris-ce/
# Connect left half and press reset button underneath keyboard
west flash -d build/iris_ce_left

west build -p -d build/iris_ce_right -b iris_ce_right -- -DZMK_EXTRA_MODULES=/path/to/zmk-board-iris-ce/
# Connect left half and press reset button underneath keyboard
west flash -d build/iris_ce_right
```

## ZMK Studio
This board definition has support for ZMK Studio. See https://zmk.dev/docs/features/studio for building with ZMK Studio enabled.
When connecting to ZMK studio, you need to press a key bound to `@studio_unlock`. On the default keymap this has been added and is
accessed via the right shift button on the raise layer. This is located at the `CLEAR MEM` button shown on the Raise layer in
the [Keeb.io default Iris keymap](https://docs.keeb.io/assets/files/Iris%20Default%20Keymap-6d56a01516f12c3cc9727515d32d49a2.pdf).

ZMK Studio requires the number of layers to be defined in the code. Two extra empty layers have been added to the default keymap
so it has a total of 5 layers to work with.

To build for studio, your `build.yaml` needs to be updated [as documented here](https://zmk.dev/docs/features/studio). For
example, add the following definition

```
  - board: iris_ce_rev1_left
    snippet: studio-rpc-usb-uart
    cmake-args: -DCONFIG_ZMK_STUDIO=y
    artifact-name: iris_ce_rev1_left_studio
```

There are build artifacts with studio support [autobuilt here](https://github.com/paulshir/zmk-keyboard-iris-ce/actions/workflows/build.yml?query=is%3Acompleted+branch%3Amain)

