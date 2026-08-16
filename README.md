# ZMK Keyboard for Cornix

ZMK board definitions and shields for the Cornix split ergonomic keyboard.

[Documentation: English / 简体中文](http://gh.bhee.online/zmk-keyboard-cornix/) ·
[简体中文 README](./README_zh.md) ·
[日本語 README (AI-generated)](./README_jp.md)

**Current board release:** [`v3.0.0`](https://github.com/hitsmaxft/zmk-keyboard-cornix/releases/tag/v3.0.0)

**ZMK baseline:** `main` with Zephyr 4.1

![Cornix with dongle](images/cornix_with_dongle.png)

## What is included

- `cornix_left//zmk` — left half for a standard split build
- `cornix_right//zmk` — right peripheral half
- `cornix_ph_left//zmk` — left peripheral half for a dongle build
- `cornix_dongle_adapter` — central dongle matrix and Bluetooth role
- `cornix_dongle_eyelash` — optional display hardware overlay
- `cornix_indicator` — production-ready RGB battery and connection indicators

Cornix uses a compact 3×6 column-staggered layout with three thumb keys per
half. The hardware supports USB-C, Bluetooth, Kailh Choc V2 hot-swap sockets,
and 10°, 18°, or 25° tenting.

## Zephyr 4.1 requirements

Always use qualified ZMK board names such as `nice_nano//zmk`. The unqualified
`nice_nano` target may select `CONFIG_SETTINGS_NONE=y`, discard the Bluetooth
identity after reboot, and break existing split bonds.

For every nice!nano dongle or reset build, verify the final `.config` contains:

<<<<<<< HEAD
This community firmware has been tested with Cornix using ZMK and provides full split-role configuration, battery power management, and Bluetooth central/peripheral setup per ZMK split guidelines

![image](images/cornix_with_dongle.png)
![image](images/cornix_layout.png)

## Warning: device breakdown recovery

the original cornix use flash layout without softdevice
so in the project. all board use nosd layout as default

if you flash firmware into dongle and found it can't work with the original firmware
you have two solutions

1. （recommend）flash the sd restore uf2 under boooader direcotry（its for nice nano 2 ，but i think it works for most of nrf52840 device） other boards <https://github.com/hitsmaxft/Adafruit_nRF52_Bootloader/actions/runs/18398554358>
2. build your firmware with snippet 'nrf
52840-nosd', make zmk ignore soft device

## TODO LIST

- [x] 52 keys full layout keymap, since v2.0
- [x] ec11 encoder, since v2.2
- [x] no-SD image, since v2.3
- [x] support various of dongles
- [x] upgrade to zephyr4.1 and lvgl9 , since v2.7, no dongle screen support yet
- [ ] rgb since in future v3

### About RGB

Cornix shield has 2 RGB LEDs on each side, controled by PWM in the stock firmware.

The replacement solution is adapting the RGB indicator module to light up these RGBs, to achieve the same effect as the stock firmware, which uses the RGB LEDs to indicate battery status and connection status.

But it is not supported yet in this repository. PR is welcome!

## Supported Hardware: Cornix Split Keyboard

Cornix Split Tented Low‑Profile Ergo Keyboard (Jezail Funder)

Cornix is a Corne‑inspired split ergonomic keyboard featuring a compact 3×6 column‑staggered layout with six thumb‑cluster keys (three per half). It offers adjustable tenting angles at 10°, 18°, and 25°, allowing users to reduce wrist strain and find a custom ergonomic alignment

- **Split, column‑staggered layout** (3×6 + thumb cluster layout).
- **Adjustable tenting support** at 10°, 18°, 25° (hardware‑based, no firmware hacks).
- **Kailh Choc V2 hot‑swap sockets** and support for LAK or LCK low‑profile keycaps.
- **Dual‑mode connectivity**: Wired USB‑C or Bluetooth wireless (left half as master).
- **Firmware**: Fully VIAL‑supported for keymaps and layer customization, stock firmware is RMK.
- Premium **CNC‑machined aluminum chassis**, custom damping foam, and portable storage pouch.

> this project owner is RMK contributor too, support RMK <https://rmk.rs/> please

## --Bootloader Recovery Instructions--

-- The original RMK firmware removed the SoftDevice, so before flashing `zmk.uf2`, you need to restore the SoftDevice first. For specific steps, please refer to [bootloader/README.md](./bootloader/README.md). --

Since v2.3 this board' flash partitions has updated, removed SD (reducing sd partitionsize size from 150K to 4K), so You can flash firmware directly.

> You may need to reset fw by reset.uf2 from ealier version
>
> You can rollback to stock firmware by flash orgin uf2 file, backup files under rmkfw/

## 🔰 Easy Method: Clone This Repository and Build with GitHub Actions

If you're new to ZMK and don't want to deal with `west.yml` or module management, you can simply use this repository directly to customize your firmware.

### Steps

1. **Fork or Clone This Repository**
   - Click **Fork** in the top right to copy this repo to your GitHub account, or
   - Run `git clone` locally.

   > Forking is recommended, because GitHub Actions will automatically build your firmware.

2. **Edit Your Keymap**
   - Locate the keymap file in `config/cornix.keymap` (or whichever `.keymap` file you want to customize).
   - You can edit it directly or use the [ZMK Keymap Editor](https://nickcoutsos.github.io/keymap-editor/):
     - Open the editor and load your `.keymap` file.
     - Make changes with the visual editor.
     - Download the updated file and replace it in your repository.
     - Commit and push the changes to GitHub.

3. **Build with GitHub Actions**
   - After pushing, GitHub Actions will automatically run the build.
   - Once the workflow finishes, go to **Actions → your latest run → Artifacts** and download the firmware (`.uf2`) files.

4. **Flash Your Keyboard**
   - Put your board into UF2 bootloader mode (usually by double-tapping the reset button).
   - Drag and drop the `.uf2` file onto the mounted drive.

### Who Is This For?

- Beginners to ZMK
- Users who only want to customize keymaps
- Anyone who doesn't need to modify drivers or hardware definitions

## How to build Cornix Zmk firmware from scratch

This section will guide you through building the Cornix ZMK firmware from scratch using the official ZMK firmware development process.

### Prerequisites

Before starting, ensure you have the following:

- A GitHub account
- Git installed on your system
- Basic understanding of Git and GitHub
- Your Cornix keyboard PCBs ready

### Step 1: Initialize ZMK Config Repository

1. **Create a new repository** using the official ZMK config template:
   - Visit: <https://github.com/zmkfirmware/unified-zmk-config-template>
   - Click "Use this template" → "Create a new repository"
   - Name your repository (e.g., `cornix-zmk-config`)
   - Choose "Public" or "Private" as preferred
   - Click "Create repository"

2. **Clone your new repository locally**:

   ```bash
   git clone https://github.com/YOUR_USERNAME/YOUR_REPO_NAME.git
   cd YOUR_REPO_NAME
   ```

3. **Initialize ZMK development environment**:

   ```bash
   west init -l config/
   west update
   west zephyr-export
   ```

> **Important**: You should thoroughly read the ZMK documentation before proceeding, as ZMK firmware development has a learning curve.
>
> - ZMK Customization Guide: <https://zmk.dev/docs/customization>
> - ZMK Configuration: <https://zmk.dev/docs/user-setup>

### Step 2: Add Cornix Shield to Your Project

After initializing your zmk-config repository, follow the steps in the next section to integrate the Cornix shield.

## How to Add Cornix Shield to Existing ZMK Project

For users with existing zmk-config, add this repository dependency via west.yml and pull the latest version via west update:

### 1. Modify west.yml

Edit the `config/west.yml` file, add to the `manifest/remotes` section:

```yaml
remotes:
  - name: zmkfirmware
    url-base: https://github.com/zmkfirmware
  - name: cornix-shield
    url-base: https://github.com/hitsmaxft
  - name: urob
    url-base: https://github.com/urob
=======
```text
CONFIG_NVS=y
CONFIG_SETTINGS_NVS=y
>>>>>>> upstream/main
```

It must not contain `CONFIG_SETTINGS_NONE=y`.

## Build targets

<<<<<<< HEAD
### 2. Update Dependencies

```bash
west update
```

### 3. Configure Build

Edit the `build.yaml` file, add:

> [!NOTE]
>
> 1. If you are using (default) cornix without dongle, choose "cornix_left", "cornix_right" and "reset".
> 2. If you are using cornix with dongle, choose "cornix_dongle". "cornix_left_for_dongle", "cornix_right" and "reset".
> 3. Add "cornix_indicator" shield to enable RGB led light. It consumes much more power, use at your own risk.
=======
Standard split:
>>>>>>> upstream/main

```yaml
include:
  - board: cornix_left//zmk
    artifact-name: cornix_left

  - board: cornix_right//zmk
    artifact-name: cornix_right

  - board: cornix_right//zmk
    shield: settings_reset
    artifact-name: cornix_reset
```

Dongle integration:

<<<<<<< HEAD
Use your preferred method to build

- no need to recovery the sd since 2.3
- falsh reset.uf2 both side of cornix
- flash left and right uf2 files
- reset both side at the same time.

### 5. Flash Firmware

Flash the generated `.uf2` files to the corresponding microcontroller:

- Left half: `build/left/zephyr/zmk.uf2`
- Right half: `build/right/zephyr/zmk.uf2`

## Dongle Adapter Shield for Custom Dongle Users

For users who want to create their own custom dongle configurations, this repository provides a adapter shield. The complete configuration for the Cornix dongle can use multiple shields:

1. **`cornix_dongle_adapter`** - This is the common shield for the matrix and Bluetooth functionality
2. **`dongle_display`** - This is the display module for the dongle screen (or another display project)
3. **`cornix_dongle_eyelash`** - This is an example shield for setting up display device for the board (if the board already has `zephyr,display` in the device tree, this display overlay shield is not needed)

The configuration in the `build.yaml` file shows how to use these shields for the eyelash dongle:

=======
>>>>>>> upstream/main
```yaml
include:
  - board: nice_nano//zmk
    shield: cornix_dongle_adapter cornix_dongle_eyelash dongle_display
    snippet: studio-rpc-usb-uart
    artifact-name: cornix_dongle

  - board: cornix_ph_left//zmk
    artifact-name: cornix_left_for_dongle

  - board: cornix_right//zmk
    artifact-name: cornix_right

  - board: nice_nano//zmk
    shield: settings_reset
    artifact-name: dongle_reset
```

<<<<<<< HEAD
To create a custom shield for the display part:

1. The `dongle_display` module is a module contains display widgets, included as part of the project dependencies via west or locally
2. If you need to create a custom shield for your display hardware, you can create a new shield that provides the appropriate display configuration. Here shows `cornix_dongle_eyelash` as an example
3. If your board already has `zephyr,display` in the device tree, you can omit the `cornix_dongle_eyelash` shield
4. Include your custom shield in the build configuration

For custom dongle screens, add a new target in build.yaml for your custom dongle:

```yaml
- board: nice_nano
  shield: cornix_dongle_adapter cornix_dongle_eyelash dongle_display
  snippet: studio-rpc-usb-uart zmk-usb-logging
  artifact-name: cornix_dongle
```

To create a custom shield for your display:

1. Use `cornix_dongle_adapter` as the base shield for the matrix and Bluetooth functionality
2. Add your custom shield in the `build.yaml` file with the appropriate board and configuration
3. Use `cornix_dongle_eyelash` as an example and modify the display parts to match your custom board
4. You can copy the `cornix_dongle_eyelash` into your project's `boards/shield/` directory, and use the same name or rename it as a new shield

The configuration in the `west.yml` file remains the same:

```yaml
remotes:
  - name: zmkfirmware
    url-base: https://github.com/zmkfirmware
  - name: cornix-shield
    url-base: https://github.com/hitsmaxft
  - name: urob
    url-base: https://github.com/urob
```

```yaml
projects:
  - name: zmk
    remote: zmkfirmware
    revision: main
    import: app/west.yml
  - name: zmk-keyboard-cornix
    remote: cornix-shield
    revision: main
  - name: zmk-helpers
    remote: urob
    revision: main
```
=======
Use `cornix_dongle_eyelash` only when the dongle board does not already expose
`zephyr,display`. The `dongle_display` module supplies the display widgets.

## RGB indicators

Version 3.0.0 makes the optional `cornix_indicator` shield production-ready.
It uses `zmk-rgbled-widget` to show battery and split-connection state.

The shield sets `CONFIG_RGBLED_WIDGET_EXT_POWER_TIMEOUT_MS=1000`. When no
animation or static indicator remains active, the WS2812 power rail is turned
off after 1000 ms to reduce idle consumption. LEDs that remain illuminated
still consume power. RGB is opt-in and is not enabled in the default v3.0.0
release artifacts.
>>>>>>> upstream/main

## Flashing and recovery

1. Flash the matching settings-reset UF2 to every role whose bonds must be
   cleared.
2. Flash the left, right, and optional dongle UF2 files to their matching
   devices.
3. Reset both halves together and then reconnect the host.

Cornix has used the no-SoftDevice flash layout since v2.3. For older firmware,
stock RMK interoperability, or a board that no longer enters UF2 mode, follow
the [bootloader recovery guide](./bootloader/README.md). Do not mix firmware
roles or flash layouts without resetting the affected devices.

## Documentation and support

<<<<<<< HEAD
### Build Steps

1. **Clone this repository**:

```bash
git clone https://github.com/hitsmaxft/zmk-keyboard-cornix.git
```

2. **Configure your ZMK build with the extra module**:

Edit your `.west/config` file and add the cmake argument under the `[build]` section:

```ini
[build]
cmake-args = -DCMAKE_EXPORT_COMPILE_COMMANDS=ON -DZMK_EXTRA_MODULES=/full/absolute/path/to/zmk-keyboard-cornix
```

Replace `/full/absolute/path/to/zmk-keyboard-cornix` with the actual absolute path where you cloned this repository.

3. **Build the firmware**:

```bash
# <<<<<<< HEAD
# west build -b cornix_main_left
# =======
# west build -b cornix_left
# >>>>>>> 16dcccb (migrate to zephyr4 , disable dongle screen)
west build -b cornix_right
```

This method allows you to use the Cornix shield without modifying your existing ZMK configuration's west.yaml file.

</details>
=======
- [English installation guide](http://gh.bhee.online/zmk-keyboard-cornix/en/)
- [中文安装指南](http://gh.bhee.online/zmk-keyboard-cornix/zh/)
- [ZMK documentation](https://zmk.dev/docs/)
- [Issue tracker](https://github.com/hitsmaxft/zmk-keyboard-cornix/issues)
- [RMK firmware project](https://rmk.rs/)
>>>>>>> upstream/main
