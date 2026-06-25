# STM DFU Firmware Updater

Single-file HTML application for flashing STM32 firmware over USB DFU from a Chromium-based browser.

The app is implemented in [index.html](/home/ivaylo/projects/html/dfuupdate/index.html) and uses WebUSB to talk directly to STM DFU / DfuSe bootloaders without any build step or external dependencies.

## Features

- Flash ST DfuSe `.dfu` firmware images from the browser.
- Parse DfuSe targets and elements, including target addresses.
- Read DFU functional descriptors when the device exposes them.
- Recover STM interface memory descriptors from USB string descriptors.
- Support a manual memory-map override when the bootloader does not expose one.
- Erase STM flash sectors safely without re-erasing overlapping sectors in the same flash session.
- Auto-release the device after manifestation and update the UI for reconnect.

## Requirements

- Chromium-based browser with WebUSB support.
- Secure context: `https://` or `http://localhost`.
- STM32 device already in DFU mode.
- Firmware image in ST DfuSe `.dfu` format.

## Usage

1. Serve this directory from a local web server.
2. Open the page in a Chromium-based browser.
3. Put the STM32 board into DFU mode.
4. Select the `.dfu` firmware file.
5. If needed, enter a memory descriptor override such as `@Internal Flash  /0x08000000/128*002Kg`.
6. Click `Connect Device`.
7. Click `Flash Firmware`.
8. Wait for manifestation and automatic release.

## Local Hosting

Example with Python:

```bash
python3 -m http.server 8000
```

Then open:

```text
http://localhost:8000
```

## Notes

- Many STM bootloaders disconnect after successful manifestation. This is expected.
- If the device does not expose a DfuSe memory descriptor, use the manual override field in the UI.
- The updater is designed for STM DFU/DfuSe flows, not generic USB firmware upload protocols.
